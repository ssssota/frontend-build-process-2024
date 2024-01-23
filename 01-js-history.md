---
marp: true
---

# フロントエンドビルドプロセス

第1回 JavaScriptの歴史と異質さ

---

## なぜJavaScriptの歴史を振り返るのか

現在のフロントエンドビルドプロセスを語る上で、JavaScriptの歴史を振り返る必要がある。

これはJavaScriptという言語が、ほかのプログラミング言語では考えられないような **異質さ** を持っているからである。

---

## JavaScriptの異質さ

- **エンジンと隔離された仕様**(ECMAScript)
  - 「コンパイラが仕様です」という世界(Rust)もある
- **複数のエンジン**
  - SpiderMonkey (Firefox)
  - V8 (Chromium)
  - ~~Chakra (Edge)~~
  - JavaScriptCore (Safari)
  - QuickJS etc...

---

## JavaScriptの登場

1995年、Brendan Eich氏が開発した。
1996年、Netscape Navigator 2.0に搭載された。

> Netscape NavigatorはWWW初期のWebブラウザ。

1996年、Internet Explorerは独自にJScriptというJavaScriptの方言を開発、搭載した。
1997年、ECMAScript 1が標準化された。

IEは着実にシェアを伸ばしたが、2000年代に入るとFirefoxが台頭し、2008年にはChromeが登場した。それぞれが独自のJavaScriptエンジンを搭載していた。
（この時点でlibjsなどと唯一のエンジンがでていたら、あるいは）

---

## 2000年代のJavaScript

HTML上のscriptタグにコードを記述。
複数のscriptタグでライブラリを読み込むなど。

```html
<script src="/jquery.min.js"></script>
<script>
  <!--
  $(function () {
    // ...
  });
  //-->
</script>
```

---

## Node.jsの登場

2009年、Ryan Dahl氏が開発した。
サーバーサイドでのJavaScript実行を可能にした。

JavaScriptのモジュール化を可能にした。(CommonJS)

```js
// add.js
module.exports = function (a, b) {
  return a + b;
};
// main.js
const add = require("./add");
console.log(add(1, 2));
```

---

## モジュールの成長

CommonJSモジュールは、Node.js/npmの登場により、急速に成長した。

2011年、BrowserifyによりComomonJSモジュールをブラウザ上で実行可能にした。
原初のフロントエンドビルドツール（バンドラ）。

> bower、AMD、UMD、RequireJSは省略。

---

## JavaScriptのトランスパイル

少し年代を遡るが、2006年のGWT、2009年のCoffeeScriptと、JavaScriptを別の言語から生成する動きが徐々に出てきた。(source-to-sourceコンパイラ)

2011年のJSConfにて、Brendan Eich氏がtranspilerという言葉を提唱した。
2012年にはMicrosoftからTypeScriptが登場した。

JavaScriptの**言語仕様自体を拡張するのは、ブラウザのサポートが必要なため困難**。
それゆえに、トランスパイルが主流となった。

2014年に登場したBabelは、下位互換性のためのトランスパイラとして、JSからJSへの変換を行う。

---

## webpackの登場

2014年、webpackの登場によりフロントエンドはビルドする世界が始まる。

webpackは、CommonJSモジュールをブラウザ上で実行可能にするだけでなく、トランスパイルやCSSや画像などのリソースもバンドルできるようになった。(`css-loader`, `ts-loader` など)

webpackの設定は複雑になりがちで当時頭を悩ませたひとは数知れず。

---

## ReactやVue.jsの登場

2010年、GoogleがAngularJS
2013年、FacebookがReact
2014年、Evan You氏がVue.jsを発表した。

宣言的UIは、モジュール化されたJavaScriptと相性が良く、フロントエンドの開発を大きく変えた。
これもビルドツールの普及に一役買っていると言っても過言ではない。

また `create-react-app` や `vue-cli` などのCLIツールが登場し、webpackの設定が隠蔽されるように。

---

## 現在

いくつもあった**AltJSは、TypeScriptに収束**しつつある。

**ES Modulesが標準化**され、CommonJSは徐々に減少傾向。
（TypeScript使用時はあまり気にならないがモジュール間で挙動に微妙な差異がある）

Rust製、Go製のツールによる**高速化**。
（Rust: swc,Turbopack,Rspack,oxc,ezno,... / Go: esbuild）

**DenoやBun、エッジコンピューティング**など、ブラウザ、Node.js以外のJavaScript実行環境が登場。言語仕様以外の標準化も進む。

---

## まとめ

JavaScriptは他の言語に比べ下位互換性が重視されるため、特殊な進化を遂げてきた。

JavaScriptをそのまま人間が書くのはしんどい（scriptタグもしんどいし、JavaScript自体しんどい）。なので、トランスパイルやビルドツールが必要。

フロントエンド開発は、JavaScriptだけではない。HTMLやCSS、画像などのリソースも配備する必要がある。
