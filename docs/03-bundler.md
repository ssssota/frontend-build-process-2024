---
marp: true
---

# フロントエンドビルドプロセス

第3回 バンドラ（+ミニファイア）

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

## バンドラってなんだっけ

バンドラは、複数のJavaScriptファイルを1つにまとめるツール。
時として、JavaScript以外のファイルをまとめたり、逆に分割したりすることもある。

source-to-source compilerの一種とも言える。

---

## バンドラの動きを考える

```js
// add.js
module.exports = (a, b) => a + b;
// main.js
const add = require("./add");
console.log(add(1, 2));
```

↓

```js
const add = (a, b) => a + b;
console.log(add(1, 2));
```

リアルではもっと複雑。

---

## バンドラの役割

「ファイルをまとめる」といっても何十、何百といったファイルの複雑な依存関係を解決する必要がある。
これを行うのがバンドラの役割。

具体的には？

---

## フロントエンド開発に用いるバンドラ

- webpack
- Rollup
- esbuild
- Turbopack (Vercel)
- Rspack (ByteDance)
- browserify (新規に使われることはほぼない)
- spack/swcpack (使ってる人いない)

---

## webpack

webpackは、バンドラの中でも最も有名なものの一つ。設定が難解。

Next.js内部でも利用されている。

Rust製のTurbopackやRspackなどが後継として開発されている。

---

## Rollup

ES Modulesファーストなバンドラ。CommonJSはプラグインによって対応。

一時期はライブラリ向けなどと言われていたが、最近ではViteなどアプリケーション向けにも利用される。

Rust製のRolldownが後継として開発されている。

https://rollupjs.org/repl/

---

## esbuild

Go製のバンドラ。
ES ModulesもCommonJSもサポートしている。

Viteの開発モードで、依存関係のバンドルに用いられる。

https://esbuild.egoist.dev/ (公式はhttps://esbuild.github.io/try/)

---

## まとめ

バンドラは複雑な依存関係を解決し、複数のファイルを1つにまとめるツール。

ブラウザで効率よく読み込むために、予めまとめる・分割する。

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

## ミニファイアってなんだっけ

Minifier。

JavaScriptやCSSはネットワーク経由でユーザーのブラウザに配信される。
ファイルサイズが小さければ小さいほど高速に行うことができる。

具体的には？

---

## フロントエンド開発に用いるMinifier

- Terser
- Rollup
- esbuild
- SWC

---

## Minifierの動きを考える

```js
// Calculates the sum of all numbers from 1 to the given number.
const calculateSum = (n) => {
  let sum = 0;
  for (let i = 0; i < n; i++) {
    sum += i;
  }
  return sum;
};
const add = (a, b) => a + b;
console.log(calculateSum(10));
console.log(calculateSum(10));
console.log(calculateSum(10));
```

スペースは削減できそう...

---

## Minifierの基本方針

- 変数名の短縮
- 不要な空白の削除
- コメントの削除
- 不要なコードの削除
  - DCE (Dead Code Elimination)
  - Tree shaking

---

## DCE (Dead Code Elimination)

```js
function add(a, b) {
  return a + b;
  const c = a * b; // Dead code
  return c; // Dead code
}
```

---

## Tree shaking

```js
// math.js
export const add = (a, b) => a + b;
export const mul = (a, b) => a * b;
// main.js
import { add } from "./math";
console.log(add(1, 2));
```

~~`mul` 関数はimportされていないので、削除される。~~

**`add` 関数はimportされているので、出力にも含める。**

この操作はimportの追跡が必要なのでバンドラによって行われる。
特に外部依存モジュールで効果を発揮する。

---

## まとめ

Minifierはファイルサイズを小さくするためのツール。

最近はバンドラに統合されていることが多い。

---

## ビルドプロセス概観 (Updated!)

```
                                   ┌────────────────────┐
┌──────────┐      ┌─────────┐      │Bundle              │      ┌───────┐
│TypeScript│  TS  │Transpile│  JS  │          ┌─────────┤  JS  │Browser│
│File      ├─────►│         ├──┬──►│          │Minify   ├─────►│Runtime│
└──────────┘      └─────────┘  │   │          │         │      └───────┘
                               │   └─────────┬┴─────────┘
┌──────────┐      ┌─────────┐  │             │                 ┌───────┐
│TypeScript│  TS  │Transpile│  │             ▼             JS  │Server │
│File      ├─────►│         ├──┴──────────────────────────────►│Runtime│
└──────────┘      └─────────┘                                  └───────┘
```

<!--https://asciiflow.com/#/share/eJyrVspLzE1VslJKVNJRykmsTC0CsqtjlCpilKwszc11YpQqgSwjSyMgqyS1ogTIiVFSIAweTdlDIUIzMCYmj3hdxCtzKs1LyUnFcDtBYyDOCaksSA1OLsosKAHrCQkG6wkpSswrLsjMSQULegUjmUik46B6nIryy4tTi%2BDWuWXCnIpF37RdyLYQVABS4ZuZl5lWiUd1UGleSWZuKtwB1At%2BiFp0B5EaBYQT4VBIiug2YNpIpaSIYdE0VD402QWnFpWlFilQKdnRAtE4cRIC%2BCJDqVapFgCEedKW-->
