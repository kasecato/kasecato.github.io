## 5 分で作る!
## PostCSS で Dark テーマ

<a href="https://twitter.com/k_kato" target="_blank"><img style="border-radius: 50% !important;" src="img/twitter.jpg" width="200px" alt="Keisuke KATO"></a> 

#### 合同勉強会 in 福岡 @ [Nulab](https://connpass.com/event/49117/)

2017/2/11 <a href="https://twitter.com/k_kato" target="_blank">@k_kato</a>



## About me

```js
const profile = {timeline: [
    { year: 2010, lang: "Java" },
    { year: 2011, lang: "C#"   },
    { year: 2012, lang: "C#"   },
    { year: 2013, lang: "Java" },
    { year: 2014, lang: "C#"   },
    { year: 2015, lang: "Java" },
    { year: 2016, lang: "Java" }
]}
```
```js
const addTimeline2017 = (profile) => {
    profile.timeline.push({
        year: 2017,
        lang: "Java",
        company: "Nulab",
        team: "Nulab Apps"
    })
}
```



## Front-end Developer Handbook 2017
Cody Lindley [@codylindley](https://twitter.com/codylindley)

In 2017 expect...
<!-- .element: class="fragment" data-fragment-index="10" -->

> More people will move from Sass to PostCSS + cssnext.
<!-- .element: class="fragment" data-fragment-index="20" -->

> より多くの人が Sass から PostCSS + cssnext に変わるだろう
<!-- .element: class="fragment" data-fragment-index="20" -->

<table>
    <tr>
        <td style="vertical-align: middle;">
            <a href="http://sass-lang.com/" target="_blank"><img style="height: 100px;" src="img/sass.svg" /></a>
        </td>
        <td style="vertical-align: middle;">
            =>
        </td>
        <td style="vertical-align: middle;">
            <a href="http://postcss.org/" target="_blank"><img style="height: 100px;" src="img/mmm.svg" /></a>
        </td>
        <td style="vertical-align: middle;">
            <span style="font-size: 200%">+</span>
        </td>
        <td style="vertical-align: middle;">
            <a href="http://cssnext.io/" target="_blank"><img style="height: 40px;" src="img/cssnext.svg" /></a>
        </td>
    </tr>
</table>
<!-- .element: class="fragment" data-fragment-index="20" -->



## PostCSS + cssnext とは？

<table>
    <tr>
        <td style="vertical-align: middle;">
            <a href="http://postcss.org/" target="_blank"><img style="height: 100px;" src="img/mmm.svg" /></a>
        </td>
        <td style="vertical-align: middle;">
            ** PostCSS = CSS パーサ (構文解析器) **
<!-- .element: class="fragment" data-fragment-index="10" -->
        </td>
    </tr>
    <tr>
        <td style="vertical-align: middle;">
            <a href="http://cssnext.io/" target="_blank"><img style="height: 40px;" src="img/cssnext.svg" /></a>
        </td>
        <td style="vertical-align: middle;">
            PostCSS のプラグインを束ねたプラグイン
<!-- .element: class="fragment" data-fragment-index="20" -->
        </td>
    </tr>
</table>

<div>
[automatic vendor prefixes (Autoprefixer)](http://cssnext.io/features/#automatic-vendor-prefixes) <br/>
[custom properties &amp; var()](http://cssnext.io/features/#custom-properties-var) <br/>
[reduced calc()](http://cssnext.io/features/#reduced-calc) <br/>
[... 26 機能](http://cssnext.io/features/)
</div>
<!-- .element: class="fragment" data-fragment-index="50" -->



### PostCSS プラグインが使うパーサとは？

CSS を構文解析して AST (抽象構文木) を生成する
<!-- .element: class="fragment" data-fragment-index="10" -->

<table>
    <tr>
        <td>
            <svg width="470" height="440" xmlns="http://www.w3.org/2000/svg">  <g transform="matrix(1.00, 0.00, -0.00, -1.00, 0.0, 768.0)">    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="81" y="-718" style="fill: rgb(0, 20, 128); font-size: 30px; white-space: pre;">@</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="114" y="-718" style="fill: rgb(127, 154, 0); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">import</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="212" y="-718" style="fill: rgb(0, 163, 152); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">'editor.css'</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="355" y="-718" style="fill: rgb(88, 110, 117); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">;</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="81" y="-639" style="fill: rgb(128, 128, 128); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">/\* vim is sexy */</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="81" y="-559" style="fill: rgb(0, 140, 216); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">.emacs</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="188" y="-559" style="fill: rgb(88, 110, 117); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">{</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="135" y="-492" style="fill: rgb(188, 135, 0); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">font-family</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="283" y="-492" style="fill: rgb(88, 110, 117); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">: </text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="294" y="-492" style="fill: rgb(0, 163, 152); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">"MS Gothic"</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="458" y="-492" style="font-size: 30px; letter-spacing: 0.002px; white-space: pre;">;</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="135" y="-429" style="fill: rgb(188, 135, 0); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">z-index</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="234" y="-429" style="fill: rgb(88, 110, 117); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">: </text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="265" y="-429" style="fill: rgb(0, 163, 152); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">-1</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="292" y="-429" style="fill: rgb(88, 110, 117); font-size: 30px; letter-spacing: 0.002px; white-space: pre;">;</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="87" y="-379" style="fill: rgb(88, 110, 117); font-size: 30px; white-space: pre;">}</text>  </g></svg>
<!-- .element: class="fragment" data-fragment-index="20" -->
            <svg width="470" height="490" style="margin-top: -490px" xmlns="http://www.w3.org/2000/svg">  <g transform="matrix(1.00, 0.00, -0.00, -1.00, 0.0, 768.0)">    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="378" y="-701" style="font-size: 30px; font-style: italic; font-weight: bold; white-space: pre;">atrule</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="309" y="-624" style="font-size: 30px; font-style: italic; font-weight: bold; letter-spacing: 0.004px; white-space: pre;">comment</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="228" y="-547.009" style="font-size: 30px; font-style: italic; font-weight: bold; letter-spacing: 0.004px; white-space: pre;">rule</text>  </g></svg>
<!-- .element: class="fragment" data-fragment-index="30" -->
            <svg width="470" height="546" style="margin-top: -546px" xmlns="http://www.w3.org/2000/svg">  <g transform="matrix(1.00, 0.00, -0.00, -1.00, 0.0, 768.0)">    <text transform="matrix(1, 0, 0, -1, 0, 0)" y="-691" x="127" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">name</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="233" y="-690" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">params</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="158" y="-607" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">text</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="87" y="-529" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">selector</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="175" y="-465" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">prop</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="335" y="-462" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">value</text>    <text transform="matrix(1, 0, 0, -1, 0, 0)" x="250" y="-394" style="fill-opacity: 0.5; font-size: 30px; font-style: italic; white-space: pre;">decl</text>  </g></svg>
<!-- .element: class="fragment" data-fragment-index="40" -->
        </td>
        <td>
            <svg width="680" height="400" xmlns="http://www.w3.org/2000/svg">  <g transform="translate(80,40)">    <path class="link" d="M306.6666666666667,100C306.6666666666667,150 255.55555555555557,150 255.55555555555557,200"/>    <path class="link" d="M306.6666666666667,100C306.6666666666667,150 357.7777777777778,150 357.7777777777778,200"/>    <path class="link" d="M204.44444444444446,0C204.44444444444446,50 102.22222222222223,50 102.22222222222223,100"/>    <path class="link" d="M204.44444444444446,0C204.44444444444446,50 204.44444444444446,50 204.44444444444446,100"/>    <path class="link" d="M204.44444444444446,0C204.44444444444446,50 306.6666666666667,50 306.6666666666667,100"/>    <g class="node" transform="translate(357.7777777777778,200)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text y="80" x="60" style="text-anchor: middle; white-space: pre;"><tspan>decl &gt; </tspan><tspan style="fill: rgb(188, 135, 0);">z-index</tspan><tspan> &gt; </tspan><tspan style="fill: rgb(0, 163, 152);">-1</tspan></text>    </g>    <g class="node" transform="translate(255.55555555555557,200)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text y="40" x="-100" style="text-anchor: middle; white-space: pre;"><tspan>decl &gt; </tspan><tspan style="fill: rgb(188, 135, 0);">font-family</tspan><tspan> &gt; </tspan><tspan style="fill: rgb(0, 163, 152);">"MS Gothic"</tspan></text>    </g>    <g class="node" transform="translate(306.6666666666667,100)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text x="100" y="8" style="text-anchor: middle; white-space: pre;"><tspan>rule &gt; </tspan><tspan style="fill: rgb(0, 141, 216);">.emacs</tspan></text>    </g>    <g class="node" transform="translate(204.44444444444446,100)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text y="40" style="text-anchor: middle; white-space: pre;"><tspan>comment &gt; </tspan><tspan style="fill: rgb(128, 128, 128);">vim is sexy</tspan></text>    </g>    <g class="node" transform="translate(102.22222222222223,100)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text y="-19" x="0" style="text-anchor: middle; white-space: pre;"><tspan>atrule &gt; </tspan><tspan style="fill: rgb(126, 154, 0);">import</tspan><tspan> &gt; </tspan><tspan style="fill: rgb(0, 163, 152);">'editor.css'</tspan></text>    </g>    <g class="node" transform="translate(204.44444444444446,0)">      <circle r="10" style="fill: rgb(255, 255, 255);"/>      <text y="-18" text-anchor="middle" style="fill-opacity: 1; white-space: pre;">root</text>    </g>  </g></svg>
<!-- .element: class="fragment" data-fragment-index="50" -->
        </td>
    </tr>
</table>



### PostCSS をコンパイラのフェーズで理解する

<img src="img/compiler.png" height="580"/>
<!-- .element: class="fragment" data-fragment-index="10" -->




## PostCSS = CSS パーサ
## ということは…


## PostCSS を使えば全世界の CSS の色を機械的に Dark テーマ化できるはず 😎



## Dark テーマ作成歴

### Hatena Blackmark
<img src="img/hatena_blackmark.png" height="420"/>


### Backlog Dark
<img src="img/backlog_dark.png" height="460"/>



## 非公式 Dark テーマの辛さ

* 全ページの CSS を確認する必要がある
<!-- .element: class="fragment" data-fragment-index="10" -->
* 配色を考えることは難しい
<!-- .element: class="fragment" data-fragment-index="20" -->
* 公式デザインの微妙な変更マニアになる
<!-- .element: class="fragment" data-fragment-index="30" -->



## [postcss-stylish-dark-theme-gene](https://github.com/k--kato/postcss-stylish-dark-theme-gene)
を作ってみた

 | Before | After
-----|-------|------
色の抽出 | 手動 | AST<!-- .element: class="fragment" data-fragment-index="20" -->
配色 | 手動 | 反転計算<!-- .element: class="fragment" data-fragment-index="20" -->
<!-- .element: class="fragment" data-fragment-index="10" -->



## 色の抽出器

```javascript
// property > color
const PROP_COLOR = /(color)|(background)|(border)|(shadow)|(outline)|(fill)/i
const hasPropColor = (prop) => PROP_COLOR.exec(prop)
```
<!-- .element: class="fragment" data-fragment-index="20" -->

```javascript
// value > color 
const HEX_COLOR = /(#[\da-f]{3,6})/i // FIXME
const NME_COLOR = /(white)|(black)|(...)/i // OMMIT
const RGB_COLOR = /(rgba?\(\s*\d+\s*,\s*\d+\s*,\s*\d+\s*,?\s*\d*\.?\d*\s*\))/i
const HSL_COLOR = /(hsla?\(\s*\d+\s*,\s*\d+%\s*,\s*\d+%\s*,?\s*\d*\.?\d*\s*\))/i;
const HWB_COLOR = /(hwba?\(\s*\d+\s*,\s*\d+\s*%?\s*,\s*\d+\s*%?\s*,?\s*\d*\.?\d*\s*\))/i
const CMY_COLOR = /(cmyk?\(\s*\d+\s*%?\s*,\s*\d+\s*%?\s*,\s*\d+\s*%?\s*,\s*\d+\s*%?\s*\))/i
const NCL_COLOR = /([rygcbm]\s*\d+\s*,\s*\d+\s*%?\s*,\s*\d+\s*%?)/i

const REGX_COLOR_LIST = 
        [RGB_COLOR, HEX_COLOR, NME_COLOR, HSL_COLOR, HWB_COLOR, CMY_COLOR, NCL_COLOR]

const hasColor = (value) => REGX_COLOR_LIST.find(regx => regx.exec(value))
```
<!-- .element: class="fragment" data-fragment-index="30" -->



## 色の反転器

```javascript
// value
const negate = (r, g, b) => `rgb(${255-r}, ${255-g}, ${255-b})`
```
<!-- .element: class="fragment" data-fragment-index="10" -->

* RGB 変換が面倒
<!-- .element: class="fragment" data-fragment-index="20" -->
    * Name -> RGB
    * Hex -> RGB
    * Hsl -> RGB
    * Hwb -> RGB
    * Cmyk -> RGB
    * Ncol -> RGB

```javascript
const Color = require('color')
return new Color('white').negate()
```
<!-- .element: class="fragment" data-fragment-index="30" -->



### PostCSS 意味解析器

```javascript
const postcss = require('postcss')

module.exports = postcss.plugin('postcss-stylish-dark-theme-gene', () => {
    return (/*Root*/root) => {
        root.walkDecls(geneDarkTheme)
        return root
    }

    const geneDarkTheme = (/*Declaration*/decl) =>
        hasPropColor(decl.prop)
            ? negateColor(decl)
            : decl.remove()

    const negateColor = (/*Declaration*/decl) =>
        REGX_COLOR_LIST.find(regx =>
            regx.exec(decl.value)
                ? decl.value = decl.value.replace(regx, c => new Color(c).negate())
                : false
        )
})
```
<!-- .element: class="fragment" data-fragment-index="10" -->



# DEMO

## <a href="https://twitter.github.io/scala_school/" target="_blank">Scala School</a>



# Thanks!

[postcss-stylish-dark-theme-gene](https://github.com/k--kato/postcss-stylish-dark-theme-gene)


## 参考ノート

* Front-End Developer Handbook 2017, https://frontendmasters.gitbooks.io/front-end-handbook-2017/content/
* PostCSS, http://postcss.org/
* cssnext, http://cssnext.io/
* PostCss.parts, http://postcss.parts/
* Josh, "color", https://github.com/qix-/color
* Rob Sanchez, "css-color-extractor", https://github.com/rsanchez/css-color-extractor
* Катерина Торт, "Как написать postcss-плагин", http://cakeinpanic.github.io/hackathon/


* Wikipedia, 「Yacc」, https://ja.wikipedia.org/wiki/Yacc
* Florian Rappl, "Compiler construction", http://talks.florian-rappl.de/Compiler/
* W3Schools, "Color Converter", http://www.w3schools.com/colors/colors_converter.asp
* Qiita, 「PostCSSのASTを操作する」, http://qiita.com/morishitter/items/8f87ea4004e9152d9b61
* Qiita, 「cssnextから学ぶ次世代CSS」, http://qiita.com/howdy39/items/92a7de771bbea99dbc7c



## 付録 A　PostCSS AST JSON (抜粋)

```json
{
  "type": "root",
  "nodes": [
    {
      "type": "atrule",
      "name": "import",
      "params": "'editor.css'"
    },
    {
      "type": "comment",
      "text": "vim is sexy"
    },
    {
      "type": "rule",
      "selector": ".emacs",
      "nodes": [
        {
          "type": "decl",
          "prop": "font",
          "value": "\"MS Gothic\""
        },
        {
          "type": "decl",
          "prop": "z-index",
          "value": "-1"
        }
      ]
    }
  ]
}
```



## 付録 B　CSS の色の種類
* color
```css
color: #fff;
color: #ffffff;
```
* background
```css
background: white url(bar.svg) no-repeat center center;
background-color: white;
background-image: linear-gradient(to bottom right, blue, white);
```
* border
```css
border: 1px solid rgb(255, 255, 255);
border-top: 1px solid rgba(255, 255, 255, 0.5);
border-left-color: rgba(255, 255, 255, 0.5);
```


* shadow
```css
text-shadow: 1px 1px 1px black;
box-shadow: 2px 2px 2px black;
```
* outline
```css
outline: 1px solid white;
outline-color: white;
```
* fill
```css
fill: white;
```



## 類似のアプローチ

* Rob Sanchez, "postcss-colors-only", https://github.com/rsanchez/postcss-colors-only
* Shiwaforce.com, "postcss-high-contrast", https://github.com/shiwaforce/postcss-high-contrast
* Stephen Way, "postcss-contrast", https://github.com/stephenway/postcss-contrast
