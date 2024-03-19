---
marp: true
---

# フロントエンドビルドプロセス

第4回 ビルドツール

---

## ビルドプロセス概観

```
             Build tool
             ┌────────────────────────────────────────────┐
             │                                            │
             │                     ┌────────────────────┐ │
┌──────────┐ │    ┌─────────┐      │Bundle              │ │    ┌───────┐
│TypeScript│ │TS  │Transpile│  JS  │          ┌─────────┤ │JS  │Browser│
│File      ├─┼───►│         ├──┬──►│          │Minify   ├─┼───►│Runtime│
└──────────┘ │    └─────────┘  │   │          │         │ │    └───────┘
             │                 │   └─────────┬┴─────────┘ │
┌──────────┐ │    ┌─────────┐  │             │            │    ┌───────┐
│TypeScript│ │TS  │Transpile│  │             ▼            │JS  │Server │
│File      ├─┼───►│         ├──┴──────────────────────────┼───►│Runtime│
└──────────┘ │    └─────────┘                             │    └───────┘
             │                                            │
             └────────────────────────────────────────────┘
```

<!--https://asciiflow.com/#/share/eJyrVspLzE1VslJKVNJRykmsTC0CsqtjlCpilKwszc11YpQqgSwjSwMgqyS1ogTIiVFSQAaPpuwZJAjFWTExeejOVCABkGscFXyBxWpS9BEZTECGU2leSk4qpr8JGwRxUkhlQWpwclFmQQlUV0gwWFdIUWJecUFmTirYIK9gtNAiyidQXU5F%2BeXFqUVwK90yYQ7GonPaLmR7CCoAqfDNzMtMq8SjOqg0ryQzNxXuAGpGA0QlupNIjwqCCRNuEYWIDkkT3QY0PlHh8YjYpIlh2TR0y6DJMDi1qCy1SOERdZIhLRDNEyseQFSkEEykhC2AAyzGDRKE6kylWqVaAGDN4xs%3D-->

---

## ビルドプロセスってなんだっけ

そもそもこの勉強会の主題であるビルドプロセスとは？

→ 開発者が書いたコードをブラウザ（またはサーバー）で実行する形に変換すること
　（トランスパイルやバンドル、ミニファイを含む）（この勉強会独自の定義）

---

## ビルドツール

ビルドプロセスを自動化・隠蔽するためのツール。
この定義に則れば、以下のすべてがビルドツールであると言える。

- webpack
  - Next.js
- Vite
  - SvelteKit
  - Nuxt
  - and more...
- esbuild

---

## ビルドツールの役割(1)

- エントリーポイントの解決

プログラムには(必ず)エントリーポイントがある。デベロッパー自身で指定することもあるが、今日のフロントエンド開発ではフレームワークの規則に従うことが多い。

ビルドツールはエントリーポイントを解決し、それを元にビルドを行う。

Next.jsでは `pages` / `app` ディレクトリのファイル。

---

## ビルドツールの役割(2)

- モジュールの解決

ビルドツールはJS/TS以外のファイルも扱うことがある。
画像、CSS、WASM、Vue.jsやSvelteコンポーネントも。
`import` された時の挙動はビルドツールやそのプラグインによって提供されている。
画像の場合、URLとして扱うか、Base64化するか、ArrayBufferにするかなど。

Next.jsでは `next/image` で静的な画像を表示することができ、最適化の恩恵が受けられる。

---

### 解決って？

「エントリーポイントを解決」「モジュールを解決」という言葉がビルドツールの文脈でよく出てくる。英語だと _resolve_ という単語に相当。

イメージとしては、見つける、探し出す、という感じ。

`/www/app/index.js` が `./add` をimportしていたら `/www/app/add.js` を見つけるイメージ。
設定によっては `/www/app/add.ts` や `/www/app/add/index.js` などになる可能性も。

---

## ビルドツールの役割(3)

- バンドルと出力のコントロール

今日のフロントエンド開発では、すべてを1つのファイルにまとめあげることはない。

サーバー向け、クライアント向けはもちろん、クライアント向けでもリソース効率化のために分割することがある。

ビルドツールはこれらのバンドルを制御する。
これに伴い、出力されるファイルの名前やディレクトリ構造も制御する。

---

## ビルドツールの役割 番外編

- 開発サーバーとHMR

フロントエンドビルドツールは、開発用のサーバーを提供している。
また、付随する形でHMR(Hot Module Replacement)を提供することが多い。

HMRでは、開発者がコードを書き換えた際にその変更をブラウザに通知、影響範囲を最小限に留めながら変更を適用する。
当たり前のように使われているが、仕組みは複雑で、ネイティブのモバイルアプリ開発ではWebほど柔軟には利用できない。

---

## おわりに

フロントエンド開発においてビルドツールは欠かせない存在になった。
同時に複雑度は増している。

問題発生時の問題切り分け、パフォーマンス改善、開発効率向上など、今回の内容を活かしてほしい。