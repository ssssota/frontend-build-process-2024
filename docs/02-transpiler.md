---
marp: true
---

# フロントエンドビルドプロセス

第2回 トランスパイラ

---

## ビルドプロセス概観

```
┌──────────┐      ┌─────────┐      ┌──────┐      ┌──────┐      ┌───────┐
│TypeScript│  TS  │Transpile│  JS  │Bundle│  JS  │Minify│  JS  │Browser│
│File      ├─────►│         ├──┬──►│      ├─┬───►│      ├─────►│Runtime│
└──────────┘      └─────────┘  │   └──────┘ │    └──────┘      └───────┘
                               │            │
┌──────────┐      ┌─────────┐  │            │                  ┌───────┐
│TypeScript│  TS  │Transpile│  │            │              JS  │Server │
│File      ├─────►│         ├──┘            └─────────────────►│Runtime│
└──────────┘      └─────────┘                                  └───────┘
```

<!--https://asciiflow.com/#/share/eJyrVspLzE1VssorzcnRUcpJrEwtUrJSqo5RqohRsrI0N9eJUaoEsowsjYGsktSKEiAnRunRlD0EkQIYUKiMElkgionJA5IhlQWpwclFmQUlYD0hwWA9IUWJecUFmTmpYEEviKBTaV4KqohvZl5mWiWKmqL88uLUIrjxbkAzcLpk2i64OwkqIE82qDSvJDM3Fe4c6kUMRC1OeahRlESOAn6AHHQQLm28iMtCKqYvQhZB01ZwalFZahHCp2SmLSKdjw%2FROG0RAvgCXalWqRYAGD%2BMig%3D%3D-->

---

## トランスパイラってなんだっけ

(前回のおさらい)

GWTやCoffeeScript、TypeScriptなどは、**別の言語からJavaScriptを生成する**トランスパイラ。

Babelは、**JavaScriptからJavaScriptへの変換を行う**トランスパイラ。

ソースを入力し、ソースを出力するため、source-to-source compilerとも呼ばれる。

---

## source-to-source compiler

ソースコードを入力し、ソースコードを出力するソフトウェアの総称。
語義が広い。

- formatter?
- linter?
- minifier?
- bundler?

今回は、狭義のトランスパイラとして上記4つは除外する。
具体的には？

---

## フロントエンド開発に用いるトランスパイラ

- TypeScript (tsc)
- Babel
- SWC (ただしminifierとしても使える)
- esbuild (ただしminifier,bundlerとしても使える)
- SASS/SCSS (sass), PostCSS (postcss)

---

## TypeScript (tsc)

一番わかりやすいところ。
JavaScriptに型を導入したような言語。

静的型付けにより、型チェックを行えるようにした。
これにより、ランタイムでのエラーを減らすことができる。

ただし、**フロントエンド開発のプロダクションビルドでは利用されない**ことが多い。
（詳細は後で）

---

## Babel

ECMAScript 2015以降のコードを、ECMAScript 5以前のコードに変換するために開発された。IEサポートには必須とも言えるツール。

プラグインを用いることでTypeScriptなども変換できる。
あくまでも変換にとどまり、型チェックは行わない。

Next.jsも内部でBabelが利用されることがある。

https://babeljs.io/repl

---

## SWC

Rust製のトランスパイラ。Denoでも内部的に使われている。
Babelのような過去バージョンサポートも可能（ES2015以降）。

TypeScriptを標準でサポートしている（当然、型チェックは行わない）。

Next.jsでは、v12からデフォルトのトランスパイラとして採用されている。
（オプションでBabelに変更可能）

---

## esbuild

Go製のトランスパイラ。
簡単なバンドル、ミニファイ、開発サーバーなども提供している。

TypeScriptを標準でサポートしている（当然、型チェックは行わない）。

Viteでは、デフォルトのトランスパイラとして採用されている。
（プラグインでBabelに変更可能）

---

## tscをプロダクションビルドで使わない理由

現在はIDEやCIで型チェックを行うことが多いため、プロダクションビルド時に型チェックを行う必要はない。
（Next.jsはビルド時に型チェックされるが、トランスパイラとしては利用されない）

トランスパイルのみに焦点を当てるとSWC,esbuildの方が速い。
出力のコントロールが比較的容易。

---

## まとめ

トランスパイラは、現代においてはTypeScriptをJavaScriptに変換するためのツール（といっても過言ではない）。
基本的にはJavaScriptを書くのがしんどいというモチベーション。

tscは基本ビルドでは用いない。
Babelは古いブラウザサポートのために使うことが多い。
基本的に現代はSWC,esbuildを使うことが多い。
