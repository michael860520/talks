---
highlighter: shiki
css: unocss
colorSchema: dark
transition: fade-out
title: Web future with VoidZero & antfu-eslint
mdc: true
layout: cover
growSeed: vue
growOpacity: 0.6
lang: zh-CN

monacoTypesIgnorePackages:
  - "@antfu/install-pkg"
  - "@clack/prompts"
  - "@typescript-eslint/*"
  - "eslint-plugin-*"
  - "*-eslint-parser"
  - "find-up"
  - "parse-*"
  - "globals"
  - "pkg-types"
  - "mlly"
  - "local-pkg"
  - "yargs"
  - "fast-glob"
  - "debug"
  - "globby"
  - "estraverse"
  - "pathe"
  - "acorn"
  - "acorn-*"
  - "pico*"
  - "eslint-visitor-keys"
---

# Web future with VoidZero & antfu-eslint

<div v-click op80 font-smiley>

## 🛠️ Vite、Vitest、Esbuild、Rolldown、Oxc

<p op80 mt4="!">Next Generation Toolchain for JavaScript</p>

</div>

<div abs-br mr-10 mb-10 flex scale-120 opacity-80>
  <div flex flex-col items-center justify-center gap1>
    <div text-sm>2025-01/15</div>
  </div>
</div>

<!--
Hi 大家好，很高興今天有機會來 JSDC 分享，然後這也是我第一次當講者，希望大家會喜歡。
今天要分享的主題是《一起探索 VoidZero 為 JS 生態系準備的瑞士刀》。

[click] 那這把瑞士刀裡會包含 Vite、Vitest、Rolldown、Oxc 這些工具，
接下來就會帶大家了解一下，並做一些 Demo。
-->

---
layout: intro
growSeed: 15
growOpacity: 0.3
class: p-20
---

# Michael Lo <sup opacity-80 font-hand text-4xl>ming</sup>

<div class="[&>*]:important-leading-10 opacity-90">

- Front-End Engineer
- Enjoy learning new things, focus on open source

</div>

<div my-10 w-min flex="~ gap-1" items-center justify-center op85>
  <div i-ri-global-line op80 ma text-xl />
  <div><a href="https://www.codefarmer.tw/" target="_blank" border-none="!">mingtech.tw</a></div>

  <div i-ri-instagram-line op80 ma text-xl ml4 text="#E1306C" />
  <div><a href="https://www.instagram.com/codefarmer.tw" border-none="!" target="_blank">mingtech.tw</a></div>

  <div i-ri-github-fill op80 ma text-xl ml4 />
  <div w-40><a href="https://github.com/ckchuang-dev" target="_blank" border-none="!">michael860520</a></div>
</div>

<img src="/avatar.png" rounded-full w-40 abs-tr mt-32 mr-30 />

---
layout: statement
growSeed: 25
growOpacity: 0.3
---

<div transition transition-500 :class="$clicks > 0 && 'translate-y--50 scale-60 op80'">
  <div transition transition-500 ease-in-out font-fast text-6 mb2 :class="$clicks > 0 ? 'op70' : 'op0'">建構工具 vs. 打包工具</div>
  <h1>Build Tool vs. Bundler</h1>
</div>

<div v-click transition transition-500 mt--10>
  <img src="/build-tool-intro.png" w="4/5" absolute left-27 top-30 rounded-3xl />
</div>

<div v-click transition transition-500 mt--10>
  <img src="/build-tool.png" w="4/5" absolute left-25 top-52 rounded-3xl />
  <div abs-br m-5 op60 text-xs font-semibold>圖片來源： https://rsbuild.dev/guide/start/index</div>
</div>

<!--
在最開始先快速帶過一些後面講解會需要知道的基本介紹。

首先是 Build Tool 和 Bundler 的差別。
中文也有人翻作「建構工具」跟「打包工具」。
大家可以暫停兩三秒，想一下這兩者的差別是什麼？

[click] 簡單說的話，當你今天想做一個網站，從在 local 開發到最後部署到雲端伺服器，
這個開發過程中在處理網頁畫面的部分，背後可能有機會去用上這兩種工具。

建構工具的部分可以理解為你在開發時下了 `npm run dev` 這樣的指令後，
會在 local 去啟一個 dev server 來協助你邊寫程式邊確認畫面。

因為在開發過程中，可能會用一些像是 typescript、scss 這些語法來協助開發。

等開發都完成後，要準備部署時，
因為瀏覽器本身是不認得這些語法的，
所以這時候就需要用「打包工具」去設定一些 loader 和 plugin，
來將這些語法轉譯後，再去做 minify、uglify 等動作。
最後打包出瀏覽器能看懂的 HTML、CSS、JS 檔案。

[click] 從這張圖上可以看到這兩者對應的工具，像大家比較常聽到的 Webpack 是打包工具，
而對應以前可能會有 Create React App 或 Vue CLI 或 webpack-dev-server 就是建構工具。

而今天要介紹的 Vite，本體其實是建構工具，背後打包的工作目前是使用 Rollup。

另外還有在今年剛正式釋出第一版的 Rspack，這套是由 ByteDance 的 infra 團隊用 Rust 去開發，也有分成 Rsbuild 與 Rspack。
-->

---
layout: statement
growSeed: 20
growOpacity: 0.3
---

<div transition transition-500 :class="$clicks > 0 && 'translate-y--55 scale-60 op80'">
  <div transition transition-500 ease-in-out font-fast text-6 mb2 :class="$clicks > 0 ? 'op70' : 'op0'">What is VoidZero?</div>
  <h1>VoidZero 是什麼</h1>
</div>

<div transition transition-600 text-xl font-semibold mt--10>
  <img v-click src="/void-zero-intro.png" w="3/4" absolute left-30 top-30 rounded-3xl />
  <img v-click src="/roadmap.png" w="3/4" absolute left-30 top-33 rounded-3xl />
</div>

<!--
再來講講 VoidZero 是什麼？

[click] VoidZero 是一間公司。
它是由 Vue 跟 Vite 的作者尤雨溪先生，在今年 ViteConf 時宣布募資成立的新公司。

[click] 那它的願景是希望能打造一個從建構工具的 Vite，
單元測試框架的 Vitest，
到打包工具的 Rolldown，
再到底層編譯工具的 Oxc，
一整個完整的開源工具鏈。

那為什麼會有這個想法呢，這邊就先來簡單介紹一下 Vite。
-->

---
layout: statement
clicks: 4
---

<div transition transition-500
  :class="$clicks > 0 && 'translate-y--50 scale-60 op80'">
  <div transition transition-500 ease-in-out font-fast text-6 mb2 :class="$clicks > 0 ? 'op70' : 'op0'">What is Vite?</div>
  <h1>什麼是 Vite？</h1>
</div>

<img :class="$clicks === 1 ? 'op100' : 'op0'" src="/esm-based.png" w="3/4" absolute left-30 top-30 rounded-3xl transition transition-500 />
<img :class="$clicks >= 2 && $clicks <= 4 ? 'op100' : 'op0'" src="/vite-1.png" w="3/4" absolute left-30 top-31 rounded-3xl transition transition-500 />

<!--
就像前面提到的，Vite 是一個建構工具。

[click] 如果你曾經有在開發一些前端專案時玩過 Vite，
會發現比起過往用其他 bundled-based dev server，
像是 CRA 或 Vue CLI 的體驗快很多。

其中一個主要原因是因為他利用了瀏覽器原生支援 ES Module 的特性，
來提升開發體驗。

[click]
再深入看到這張圖是 Vite 目前的架構。

簡單說的話會由 esbuild 負責 local 開發時的 pre-bundling。
把專案中的依賴套件做預先打包與快取，
可以解決像是 request waterfall、不同套件模組標準不同的問題。

而 Rollup 會負責 production 打包。

SWC (Speedy Web Compiler) 做為 Rollup 目前底層的編譯器。

[click] 而會想要開發整套工具鏈的原因是因為一些問題：
- esbuild 是用 Go 寫的，效能快，
但缺點是目前在 code-splitting、tree shaking、plugin 生態系這些功能的不齊全，在 production 打包時不能用。
- 而 Rollup 的好處是有完整的 plugin 生態系，但效能比不上其他靜態語言為基底的打包工具。
- 因為現在 dev/production 分成 esbuild 與 Rollup 兩套，所以有時會有兩種環境行為不一致或像是模組轉換的問題。造成 debug 不容易。
- 另外多套工具間的 build pipeline 中會有不少轉譯語言成本的浪費。

[click] 為了解決以上這些問題，在去年時 Vite 團隊原本與 Rspack 團隊，
決定一起開發 Rolldown 這套用 Rust 寫的打包工具。想換掉原本底層使用的 esbuild 與 Rollup。

但當團隊越鑽越深時，發現如果不一次做到好，底層的這些多套工具交互的問題可能仍會重演，
讓不同 JS 生態系的底層仍需要同時仰賴多套工具。
因此決定把夢做大成立一間獨立的開源公司來把整套工具鏈做得更完善。

因為這個過程就需要更多的開源開發者加入，也不能讓贊助資源只集中在某些人身上，所以成立公司來整合整個資源。
-->

---
class: ml20
growSeed: right
growOpacity: 0.6
---

<h1>VoidZero 工具鏈介紹 <sup text-5 op60 font-fast>toolchain intro.</sup></h1>

<img v-click="1" src="/tools.png" absolute right-8 top-33 rounded-xl w-120 />

<v-clicks mt-20 text-2xl>

- {Vite}
- {Vitest}
- {Esbuild} (v0.24)
- {Rolldown} (開發中)
- {Oxc} (開發中)

</v-clicks>

<!--
接下來講一下其他工具的細節。

[click] 看回右邊這張圖。
今天當我們用 React, Vue, Nuxt, Remix, Astro 等這些上層前端框架或 meta framework 時，
現在預設都能夠以 Vite 作為建構工具。

[click] 另外可以搭配能這個兼容 jest 的單元測試工具，Vitest。

以前如果在 Vite 專案中要能使用 jest 時，會遇到一些轉換與設定問題，
所以後來 vue 的核心開發者 sodatea 幫忙做了一個叫做 vite-jest 的工具。
後來為了能在設定時更方便去與 Vite 整合，
因此後來 Vite 核心團隊 (Antfu, Patak 等人) 開發這套 Vitest。

[click] 再來是 Rolldown 的部分，就像前面提到的目前正在開發中。

希望能利用 Rust 打造一個打包工具，
來解決目前底層在 dev/production 用兩套打包工具的問題。
就像另一套以 Rust 為基底打造的 Rspack 能兼容 Webpack 的方式一樣，
或許未來有機會能夠兼容目前所有的 bundler，
如果在生態系接受度夠高的狀況下。

[click] 最後是 Oxc。這個工具大家可能相對更陌生，另外拉出來特別講一下。
-->

---
class: ml20
growSeed: rust
growOpacity: 0.9
---

<h1>Oxc 介紹 <sup text-5 op60 font-fast>Intro.</sup></h1>

<v-clicks mt-10 text-2xl>

- Oxc (Oxidation Compiler)
- 包含以下工具
  - **Parser**：3x than `SWC`、5x than `Biome`
  - **Linter**：50-100x than `ESLint`
  - **Resolver**：28x than `webpack/enhanced-resolve`
  - 🚧 **Transformer**：對比 `SWC`、`Babel`、`tsc` 等工具
  - 🚧 **Minifier**：對比 `Terser` 等工具
  - 🚧 **Formatter**：對比 `Prettier` 等工具
- Inspired by `Biome` and `Ruff`

</v-clicks>

<!-- <img v-click src="/oxc-progress.png" absolute right-8 top-33 rounded-xl w-60 /> -->

<!--
[click] Oxc 是 Oxidation Compiler 的縮寫，
這算是一整套用 Rust 去寫的，比較底層的編譯與解析語法的工具。

Oxidation 這個字是氧化的意思，而 Rust 是生鏽的意思，
雖然沒查到相關資料，但猜測是想表示把一些工具改成 Rust 的版本，所以取這個名字。

[click] 像是比較常見的 linter 的部分可以對應到原本的 eslint，
formatter 的部分可以對應到常見的 prettier，
transformer 主要是在處理語法轉譯，對應到大家可能聽過的 Babel、SWC、tsc。

目前後面這三者都還在開發中，所以今天只會先試試看 linter 的部分。

[click] 從上面這些比較，如果稍微了解過一些 Rust 的特性的話，
會知道 Rust 因為提供更有彈性的記憶體管理系統，
所以被應用在這些底層工具上能有效提升效能。

可以理解為 Oxc 的目標想把以前我們常用的一些工具像是 Babel、ESLint、tsc、prettier 等，
用 Rust 改寫來做一個效能提速，
但本質上並不是想取代這些工具，後面會提到。

基本上跟另外一個叫做 Biome 的工具鏈是同樣的目標，
Oxc 的文件上也有提到他也受到 Biome 跟 Python 生態系的 linter — Ruff 的啟發。
-->

---
layout: two-cols
growSeed: right
growOpacity: 0.6
---

# Oxc linter demo

<img v-click="1" src="/evan-oxlint.png" absolute right-12 top-24 rounded-xl w-100 />
<img v-click="3" src="/oxlint-demo.png" absolute right-12 top-24 rounded-xl w-100 />
<img v-click="5" src="/oxc-vscode.png" absolute right-12 top-24 rounded-xl w-100 />

<div v-click="2" mt-10 text-2xl>

```shell
$ git clone https://github.com/vuejs/core.git --depth=1
$ pnpm dlx oxlint@latest # 使用 pnpm
$ npx oxlint@latest # 或是用 npm
```

</div>

<v-clicks at="+4" text-2xl mt-5>

- 能被用在 `lint-staged` 這類的 workflow
- 有提供 VSCode 擴充
- `eslint-plugin-oxlint`

</v-clicks>

<div v-click="6" mt-5>

```json
{
  "lint-staged": {
    "**/*.{js,mjs,cjs,jsx,ts}": "oxlint"
  }
}
```

</div>

<!--
最後來看一下 Oxc 裡的 linter 要怎麼試玩。

[click] 這裡因為手邊沒有太複雜的專案，
參考 Evan 當初發現這個工具時的敘述，
我們就直接用 vue 的原始碼來跑跑看。

[click] 這裡如果試著將專案載下來後，
去用不安裝套件的方式執行指令，看要使用哪種 package manager 都可以。

[click] 執行完後會看到像圖上這樣，
對 500 多個檔案去做 lint 分析，
只跑了 200 多毫秒。

[click] 目標不是在取代 ESLint，
畢竟 ESLint 生態已經廣為使用，
主要是當今天有複雜專案在部份 workflow 像是 lint-staged 或 CI 上去跑 ESLint 檢查程式碼品質，
當今天在這部份遇到瓶頸時，可以嘗試用 oxlint 來改善效能。

[click] 目前也有提供 VSCode 體系的擴充，
但這部份我沒研究太多，可以再試試看。

[click] 然後參考文件說明，目前 oxlint 只支援約 400 個規則，
如果想提前與 ESLint 整合的話，可試試這個 plugin，
並在 package.json 裡的 lint-staged 設定這樣改。
-->

---
class: ml10
---

<h1>總結 <sup text-5 op60 font-fast>recap</sup></h1>

<div mt10>
<v-clicks>

- VoidZero 生態系期待能提供一整套完整且高效的工具鏈：
  - **Vite**：目前已廣泛與多個前端框架生態系整合，提供良好的開發體驗
  - **Vitest**：能高度兼容 jest，並能與 testing-library 整合的單元測試工具
  - **Rolldown** (WIP)：做為未來提升打包速度的 bundler
  - **Oxc** (WIP)：做為未來改善 workflow 的工具組合

- 新東西很潮很快，但學好基本功才是首要任務。心有餘力可以先簡單實驗看看，畢竟新技術能不能成為經典還有待時間的考驗

- 有趣的觀點：VoidZero 做為一個 OSS 生態系如何盈利？為何 VC 願意投資？

</v-clicks>
</div>

<!--
最後做一下總結。

[click] 今天我們介紹了 VoidZero 底下的生態系，包含 Vite、Vitest、Rolldown、Oxc 這些工具

[click] 然後也要提醒大家，不要覺得新技術很多感到焦慮，可以先了解觀望即可。

[click] 最後就是有個有趣的觀點是我在看國外社群討論 VoidZero 時看到的，
就是畢竟 VoidZero 是一間「公司」，
但本體是個開源軟體生態系他本身該如何賺錢，
而創投的 Accel 又為什麼願意投資他們？
「過往在 VC 介入開源的經驗並不好」，可能是想想 Joynt 之於 Node.js、Vercel 之於 Next.js、Meta 之於 React 等等。
有一說是未來 VoidZero 這間公司會開發一些非開源的工具解決方案，來協助大企業做一些工具效能的提升，並有效減少機器成本來盈利。
或是利用 Vite 的廣大使用者，未來跟這些投資的雲端公司合作，提供一些一鍵完成像是發電子信、監控等整合方案。
-->

---
layout: center
glow: bottom
---
# ESLint v9.0.0 release flag-config


<div flex="~ col items-center justify-center w-full">

  <h2 flex="~ col" text-center>
<div text-center flex="~ col gap-2" transition duration-500 :class="$clicks < 2 ? 'translate-y-40' : ''">
  <span
    flex="~ gap-2 items-center justify-center"
    text-hex-8080f2 transition duration-500 text-1.5em
    :class="$clicks < 1 ? 'scale-150 translate-y' : ''"
  >
    <div i-logos-eslint />
    ESLint v9.0.0
  </span>
  <span v-click op75 forward:delay-400 text-2xl>於 <TimeAgo date="2024-04-05" font-bold /> 發布</span>
</div>
</h2>

  <img src="/eslint-v9.png" v-click mt4 w-60 rounded-lg shadow forward:delay-400 />
</div>

<!--
自從 ESLint 發布以來已經有 11 年了，目前它毫無疑問是 JavaScript 生態系統中最受歡迎的工具之一，幾乎是任何用到 JavaScript 的專案的標配。但盡管它已經存在了這麽長時間，但它仍然在不斷改進和發展中。

ESLint 9.0 [click] 在八個月前發布了。

[click] 這個大版本的亮點是推出了一個新的設定檔系統，稱為 Flat Config 扁平化設定檔。

-->

---

<div grid="~ cols-2 gap-6" h-full>
<div>

## 傳統設定檔 <sup text-base op50 translate-y--2 inline-block>.eslintrc</sup>

<div mt-4 h-42>

<v-clicks at="3">

- 多種檔案來源 `.eslintrc` `.eslintrc.js` `.eslintrc.json` `package.json` 等
- 基於約定的 `extends`
- 基於 npm 包名的外掛系統
- 繼承樹可能很複雜

</v-clicks>

</div>
<div v-click="1" transition duration-800 :class="$clicks < 3 ? 'translate-y--160px': ''">

```json {*|3-6|7-10|*}{at:4}
// .eslintrc.json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "plugins": [
    "vue",
    "n"
  ],
  "rules": {
    "vue/html-indent": ["error", 2]
  },
  "overrides": [
    // ...
  ]
}
```

</div>
</div>
<div>

## 扁平設定檔

<div mt-4 h-42>

<v-clicks at="3">

- 單一來源 `eslint.config.js` <sup op75>或是 `.cjs` `.mjs`</sup><br><span op0>-</span>
- 基於 JS 原生的顯式匯入
- 外掛系統基於匯入的對象 <sup op75>可以重命名外掛</sup>
- 可組合，更容易追溯

</v-clicks>

</div>
<div v-click="2" transition duration-800 delay-50 :class="$clicks < 3 ? 'translate-y--160px': ''">

```js {*|2-3,8-9|4-5,11-14|*}{at:4}
// eslint.config.js
import typescript from '@eslint-typescript/eslint-plugin'
import eslint from '@eslint/js'
import n from 'eslint-plugin-n'
import vue from 'eslint-plugin-vue'

export default [ // 導出一個設定檔數組
  eslint.configs.recommended,
  ...typescript.configs.recommended,
  {
    plugins: {
      vue,
      node: n, // 重命名外掛
    },
    rules: {
      'vue/html-indent': ['error', 2]
    }
  },
  // ...
]
```

</div>
</div>
</div>

<!--
如果你從未聽說過它或還沒有深入研究文件。
這裏，讓我為你快速比較一下傳統的 eslintrc 設定檔 [click] 和新的扁平化設定檔 [click]。

區分這兩種設定檔格式相當簡單。[click] 傳統設定檔的檔案名為 `.eslintrc` 支持各種擴展名，也可能從你的 `package.json` 中讀取。而扁平化設定檔則只會從 `eslint.config.js` 中加載，這是一個 JavaScript 設定檔檔案，作為唯一的來源。

[click] 在覆用共享設定檔時，傳統設定檔格式隱式地使用基於約定的 `extends` 屬性，它接受字符串，並根據提供的名字從本地 `node_modules` 中加載該設定檔。你需要稍微了解一下約定，才能知道它是如何解析的。而在扁平設定檔中，我們使用原生的 ESM 匯入，更加顯式，並且給你更多的控制權。

[click] 對於外掛，舊的設定檔接受一個字符串數組，這同樣是基於約定，並與外掛的 npm 包名耦合。現在在扁平設定檔中，它接受一個對象。這意味著你現在可以輕松重命名外掛，或者切換到一個 fork 而不必強制更改設定檔中的每個規則。

[click] 此外，`extends` 的繼承性質可能會導致非常覆雜的樹結構，因為共享設定檔中也可以有嵌套的 `extends`。在 flat 設定檔中，它被大大簡化了，你顯式地將共享設定檔匯入為多個對象或數組，並將它們組合成一個單一的平面設定檔。
-->

<div mt-4 h-42>

<v-clicks at="3">

- 單一來源 `eslint.config.js` <sup op75>或是 `.cjs` `.mjs`</sup><br><span op0>-</span>
- 基於 JS 原生的顯式匯入
- 外掛系統基於匯入的對象 <sup op75>可以重命名外掛</sup>
- 可組合，更容易追溯

</v-clicks>

</div>

<div v-click="2" transition duration-800 delay-50 :class="$clicks < 3 ? 'translate-y--160px': ''">

```js {*|2-3,8-9|4-5,11-14|*}{at:4}
// eslint.config.js
import typescript from '@eslint-typescript/eslint-plugin'
import eslint from '@eslint/js'
import n from 'eslint-plugin-n'
import vue from 'eslint-plugin-vue'

export default [ // 導出一個設定檔數組
  eslint.configs.recommended,
  ...typescript.configs.recommended,
  {
    plugins: {
      vue,
      node: n, // 重命名外掛
    },
    rules: {
      'vue/html-indent': ['error', 2]
    }
  },
  // ...
]
```

<div>
</div>
</div>

<!--
如果你從未聽說過它或還沒有深入研究文件。
這裏，讓我為你快速比較一下傳統的 eslintrc 設定檔 [click] 和新的扁平化設定檔 [click]。

區分這兩種設定檔格式相當簡單。[click] 傳統設定檔的檔案名為 `.eslintrc` 支持各種擴展名，也可能從你的 `package.json` 中讀取。而扁平化設定檔則只會從 `eslint.config.js` 中加載，這是一個 JavaScript 設定檔檔案，作為唯一的來源。

[click] 在覆用共享設定檔時，傳統設定檔格式隱式地使用基於約定的 `extends` 屬性，它接受字符串，並根據提供的名字從本地 `node_modules` 中加載該設定檔。你需要稍微了解一下約定，才能知道它是如何解析的。而在扁平設定檔中，我們使用原生的 ESM 匯入，更加顯式，並且給你更多的控制權。

[click] 對於外掛，舊的設定檔接受一個字符串數組，這同樣是基於約定，並與外掛的 npm 包名耦合。現在在扁平設定檔中，它接受一個對象。這意味著你現在可以輕松重命名外掛，或者切換到一個 fork 而不必強制更改設定檔中的每個規則。

[click] 此外，`extends` 的繼承性質可能會導致非常覆雜的樹結構，因為共享設定檔中也可以有嵌套的 `extends`。在 flat 設定檔中，它被大大簡化了，你顯式地將共享設定檔匯入為多個對象或數組，並將它們組合成一個單一的平面設定檔。
-->

---

## 扁平設定檔推出時間線

<Timeline mt2 />

<v-clicks>

- RFC 於 2019 年 1 月創建
- 在 `v8.21.0` 中作為實驗性功能推出
- 在 `v8.45.0` 中變為穩定版
- 在 `v9.0.0` 中成為默認設定檔
- JavaScript 檔案作為設定檔，具有完全控制權
- 簡化的繼承和覆蓋
- 靈活、動態、可定制

</v-clicks>

<!--
[click] 交代一下背景故事，這裏有一個我繪制的時間線圖來展示一下歷史。雖然扁平設定檔對一些人來說可能是新的，但實際上它已經計劃了 5 年之久。[click] 這個 RFC 於 2019 年 1 月創建，[click] 第一個實驗性實現出現在 v8.21.0 版本中，那是兩年前的事了。[click] 它在 v8.45.0 版本中變為穩定版，[click] 並在最近的 v9.0.0 版本中成為默認設定檔。在此期間，ESLint 團隊發布了多篇博客文章來解釋他們引入新格式的原因，並分享了推出的路線圖。這是一個跨越 5 年的宏大計劃的，盡管在實施的過程中不能說是無縫遷移，但是還是不可否認 ESLint 團隊付出的巨大的努力，這在開源專案中也是非常罕見的。

所以，正如我們在前一張投影片中提到的，[click] 扁平設定檔的最大好處是現在它是用 JS 編寫的，你可以完全控制。[click] 它使用原生的 import 來解析外掛和設定檔，使得繼承和覆蓋變得更加簡化。[click] 因為它完全在 JavaScript 中，共享設定檔可以是接受用戶選項的工廠函數；用戶可以有更多的能力根據他們的具體需求進行定制。
-->

---

# 遷移工具 [`@eslint/migrate-config`](https://www.npmjs.com/package/@eslint/migrate-config) <sup text-lime bg-lime:15 px1.5 rounded text-sm>官方</sup>

CLI 工具將舊設定檔轉換為扁平設定檔

```bash
npx @eslint/migrate-config .eslintrc.json
```

<div grid="~ cols-[1fr_max-content_1fr] gap-4" mt-4 v-click>

```json
// .eslintrc.json
{
  "env": {
    "node": true,
    "es6": true
  },
  "parserOptions": {
    "ecmaVersion": 2020,
    "sourceType": "module"
  },
  "extends": [
    "eslint:recommended",
    "plugin:ava/recommended",
    "prettier"
  ],
  "plugins": ["prettier", "import"],
  "rules": {
    "prettier/prettier": 2,
    "ava/no-ignored-test-files": 0,
    "ava/no-import-test-files": 0,
    "import/no-unresolved": [
      2,
      {
        "ignore": ["ava", "got"]
      }
    ],
    "import/no-unused-modules": 2,
    "import/order": [
      2,
      {
        "newlines-between": "never"
      }
    ]
  }
}
```

<span i-carbon:arrow-right mt-40 />

```js
// eslint.config.mjs
import { FlatCompat } from '@eslint/eslintrc'
import _import from 'eslint-plugin-import'
import prettier from 'eslint-plugin-prettier'

const compat = new FlatCompat()
export default [
  ...compat.extends(
    'eslint:recommended',
    'plugin:ava/recommended',
    'prettier'
  ),
  {
    plugins: {
      prettier,
      import: _import,
    },
    languageOptions: {
      ecmaVersion: 2020,
      sourceType: 'module',
    },
    rules: {
      'prettier/prettier': 2,
      'ava/no-ignored-test-files': 0,
      'ava/no-import-test-files': 0,

      'import/no-unresolved': [2, {
        ignore: ['ava', 'got'],
      }],
      'import/no-unused-modules': 2,
      'import/order': [2, {
        'newlines-between': 'never',
      }],
    },
  },
]
```

</div>

<!--
對於還在使用舊設定檔的同學，官方提供了一個 CLI 遷移工具 `@eslint/migrate-config`，它可以自動將你的舊設定檔檔案轉換為扁平設定檔。[click] 在此過程中，還會自動引入一些兼容性的運行時庫來模擬新設定檔的行為。

我建議你查看 ESLint 文件和遷移指南以獲取更詳細的說明。
-->

---
layout: fact
---

# One for All{.important-text-3em}

一個設定檔適配各種不同的專案

<!--
所以在這裏，我想再次強調主題，一個打十個。

通過最大化的靈活性和可定制性，現在可以擁有一個單一的共享設定檔，覆蓋所有不同類型的專案。
-->

---

<div grid="~ cols-2 gap-8">

<div flex="~ col gap-2">

### 舊設定檔

```json {*|3-7|*|10-14|*}{at:1}
{
  "extends": [
    "@antfu/eslint-config",
    "@antfu/eslint-config-ts",
    "@antfu/eslint-config-vue",
    "@antfu/eslint-config-vue-ts"
    // ...需要提供所有的組合
  ],
  "rules": {
    // 為了修改一項設定檔需要非常的多的手動覆蓋
    "indent": ["error", 4],
    "@typescript-eslint/indent": ["error", 4],
    "jsx-indent": ["error", 4],
    "vue/indent": ["error", 4]
  }
}
```

</div>
<div flex="~ col gap-2">

### 扁平設定檔

```ts {*|4-5|*|6-8|*}{at:1}
import antfu from '@antfu/eslint-config'

export default antfu({
  vue: true,
  typescript: true,
  stylistic: {
    indent: 4
  }
  // ...
})
```

<div flex="~ col gap-2" mt-3>

<div v-click class="slidev-vclick-target" :class="$clicks === 1 ? 'text-green' : ''">
  <div i-ph-check-circle-duotone text-green inline-block translate-y-2px />
  設定檔可以接受高階用戶選項
</div>

<div v-click class="slidev-vclick-target" :class="$clicks === 2 ? 'text-green' : ''">
  <div i-ph-check-circle-duotone text-green inline-block translate-y-2px />
  一個設定檔適用於所有專案
</div>

<div v-click class="slidev-vclick-target" :class="$clicks === 3 ? 'text-green' : ''" >
  <div i-ph-check-circle-duotone text-green inline-block translate-y-2px />
  <span v-mark.green.delay400="5">
    像 Prettier 一樣提供最小設定檔，開箱即用
  </span>
</div>

<div v-click class="slidev-vclick-target">
  <div i-ph-check-circle-duotone text-green inline-block translate-y-2px />
  <span v-mark.green.delay400="5">
    同時擁有 ESLint 完整靈活的可自定義能力
  </span>
</div>

</div>
</div>

</div>

<!--
這裏我們可以快速比較一下

[click] 在新的扁平設定檔中，共享設定檔可以是一個接受用戶選項的工廠函數，這是在舊設定檔中無法做到的。想象一下，如果我希望我的設定檔同時適用於 TypeScript 和非 TypeScript 專案，Vue 和非 Vue 專案，我將需要做一個 monorepo 來發布不同組合的設定檔。正如你所看到的，這種方式並不具備良好的擴展性，因為每增加一個選項，我們的組合數量就會翻倍。

[click] 扁平設定檔允許你提供語義化的選項來動態切換每個功能，使得一個單一的設定檔能夠適應不同的專案。

[click] 因此，我們還可以進行高級抽象來吸收底層的覆雜性，並提供一個像 Prettier 一樣的最小設定檔接口，最終用戶甚至不需要擔心底層的細節，[click] 但在他們真正需要時仍然可以完全控制。
-->

---
layout: fact
---

# 工具生態{.important-text-3em}

隨著新的 Flat Config 帶來的新工具和可能性

<!--
談談這種新格式所帶來的有趣的新工具和可能性。
-->

---

## Flat Config Utils <sup text-teal bg-teal:15 px1.5 rounded text-sm>社區</sup>

<Repo name="antfu/eslint-flat-config-utils" op50 />

<div grid="~ cols-[1fr_max-content_1fr] gap-4" mt2>
<div v-click>

```ts {*|*|7-9|10-17}{at:3}
import eslint from '@eslint/js'
import unocss from '@unocss/eslint-plugin'
import vue from 'eslint-plugin-vue'
import typescript from 'typescript-eslint'

export default [
  eslint.configs.recommended,
  ...typescript.configs.recommand,
  ...await unocss(),
  {
    files: ['*.vue'],
    ...vue.configs['vue3-recommand'],
    rules: {
      ...vue.configs['vue3-recommand'].rules,
      'vue/html-indent': ['error', 2]
    }
  }
]
```

</div>
<span i-carbon:arrow-right ma v-click />
<div v-after>

```ts {*|1,7|8-10|11-21}{at:3}
import eslint from '@eslint/js'
import unocss from '@unocss/eslint-plugin'
import { compose } from 'eslint-flat-config-utils'
import vue from 'eslint-plugin-vue'
import typescript from 'typescript-eslint'

export default compose(
  eslint.configs.recommended,
  typescript.configs.recommand, // 數組，自動解構
  unocss() // 自動解析 Promise
)
  .append( // 可鏈式擴展
    vue.configs['vue3-recommand']
  )
  // 使用名稱或索引覆蓋任何設定檔
  .override('vue', {
    files: ['*.vue'],
    rules: {
      'vue/html-indent': ['error', 2]
    }
  })
```

</div>
</div>

<!--
為了讓設定檔自定義更容易，我還制作了一個名為 `eslint-flat-config-utils` 的小型庫。

[click] 例如，這裏有一個我們可能會使用的扁平設定檔。根據每個共享設定檔的構造方式，有些可能是一個普通的設定檔對象，有些可能是一個數組，還有些可能是一個返回對象、數組甚至是 Promise 的構造函數。當你將它們一起使用時，通常需要你自己將它們合並成一個單一的數組。

[click] 使用設定檔工具，[click] 我制作了一個名為 `compose` 的實用函數，[click] 它會自動解析不同類型的設定檔，解析 Promise 並將它們合並在一起。

[click] 它還提供了一些可鏈式操作，你可以在任何你想要的位置插入額外的設定檔，或者覆蓋某些設定檔而無需手動處理合並。
-->

---

## ESLint Typegen <sup text-teal bg-teal:15 px1.5 rounded text-sm>社區</sup>

<Repo name="antfu/eslint-typegen" op50 />

<v-clicks>

![](/eslint-typegen.png){.w-200.rounded-lg.shadow.border.border-main}

</v-clicks>

<!--
然後，由於扁平設定檔的靈活性以及其提供的完整上下文，它還使類型生成成為可能。[click]

只需將你導出的整個設定檔數組用 typegen 函數包裹，它將基於你安裝的所有外掛生成一個本地的 .d.ts 檔案。這為你提供了所有使用規則的自動補全和類型檢查。
-->


---
glow: left
---

<div w="40%">

## Config Inspector <sup text-lime bg-lime:15 px1.5 rounded text-sm>官方</sup>

<div mt-4 />
<v-click>

```bash
eslint --inspect-config
```

</v-click>
<div mt-4 />

<v-clicks>

- 設定檔可視化
- 理解設定檔組合
- 內置設定檔
- 檔案路徑測試器

</v-clicks>
<div mt-4 />
<v-click>

<<< ./eslint.demo.config.ts {monaco-write}{height:'220px'}

</v-click>
</div>

<InspectorIframe />

<div v-show="false">
<!-- This block is for type discovery -->

```ts {monaco}
import antfu from '@antfu/eslint-config'
```

</div>

<!--
第一個介紹的是 ESLint Config Inspector - 一個可視化的開發工具，允許你檢查和操作最終解析的設定檔。

[click] 你可以在專案根目錄下運行 `eslint --inspect-config` 來嘗試它，它會在瀏覽器中打開一個帶有 UI 的頁面，就像你在右邊看到的那樣。

[click] 首先，它會渲染你所有的設定檔項。你可以看到所有的設定檔都列在這裏，因為它是扁平的。這裏我有一個相當覆雜的設定檔設置，包含了很多設定檔項。但通過每個設定檔提供的名稱，你可以輕松地看到和理解它們的用途。

[click] 你還可以展開每個專案，查看它們如何貢獻到最終的設定檔中，比如啟用了多少規則，或者它們的目標檔案類型是什麽，等等。

[click] 在每個規則中，你還可以看到它們的選項，一個簡短的描述，以及一個指向它們文件頁面的鏈接。

[click] 由於在 ESLint 中，你可以有不同的規則集應用於不同的檔案類型，或者更細粒度地應用於它們的確切檔案路徑。在設定檔檢查器中，你還可以輸入檔案路徑來測試這些規則是如何為該檔案啟用的。

在另一個標簽中，你還可以瀏覽每個可用的規則，鑒於你已經安裝的外掛。你可以過濾它們，查看你正在使用哪些規則，哪些規則你沒有使用，哪些是推薦的，哪些是已棄用的。

[click] 這裏我有我的設定檔檔案作為示例。這個設定檔是一個工廠函數，接受一些相當高級的選項。通過設定檔檢查器，我們可以看到它是如何根據我們提供的選項解析的。我們還可以嘗試更改選項，看看它如何影響結果。例如，我還可以提供我的 tsconfig 的路徑，這將自動為我啟用類型感知的規則。
-->

---

# 專案感知的設定檔

<div text-gray flex="~ items-center gap-1" v-click>
例子： <div i-logos-nuxt-icon inline-block /> Nuxt ESLint
</div>

<div grid="~ cols-2 gap-4" h="80%">
<div
  v-click="1"
  flex="~ col gap-2 items-center justify-center"
  transition duration-500
  :class="$clicks < 2 ? 'scale-130 translate-x-55' : ''"
>
  <img src="/nuxt-eslint.png" w-90 rounded-lg shadow border="~ main" />
  <a href="https://eslint.nuxt.com" text-sm>eslint.nuxt.com</a>
</div>

<div flex="~ col items-center justify-center" forward:delay-500 pb-10 v-click>

```ts
// 由 Nuxt 根據當前專案的設定檔生成
import withNuxt from './.nuxt/eslint.config.mjs'

export default withNuxt(
  // 你的其他自定義設定檔
)
```

</div>
</div>

<!--
扁平設定檔還使得元框架可以提供專案感知的設定檔。

[click] 例如，在 Nuxt 中，我們有基於檔案的路由、自動匯入的組件、服務器 API 目錄等。不同檔案夾或不同名稱下的檔案可能有不同的用途和不同的約束。

因此在 Nuxt 中，我們有 Nuxt ESLint 模塊 [click]，它根據用戶的專案設置生成一個子 ESLint 設定檔檔案。用戶可以從中擴展並繼續添加他們的自定義規則。

這是我們目前正在探索的扁平設定檔的一個方向，但我們相信社區會有更多有趣的方法。
-->

---
layout: fact
---

# ESLint 不只是一個 Linter{.important-text-3em}

<div v-click op50>他還是一個成熟且強大的 AST 工具包</div>

<!--
Another topic I'd like to bring up today, is the fact that ESLint is much more than a Linter.

To me, I see ESLint as a mature and powerful AST Toolkit that has a large ecosystem on its back.
-->

---

# <span op50>ESLint 也可以是...</span> <b v-click font-800>格式化工具</b>

<div grid="~ cols-2 gap-4" h="80%">
<div flex="~ col items-center justify-center">
  <img src="/eslint-stylistic.png" w-80 v-click />

  <div op75 text-lg v-click>
    集合了風格相關的 ESLint 規則。<br>格式化和 Lint 一步到位。
  </div>

  <a href="https://eslint.style" text-sm v-click>eslint.style</a>
</div>

<div flex="~ col items-center justify-center" v-click>

<div flex="~ gap-2 items-center">
  在 <div i-logos-visual-studio-code inline-block /> VS Code 中的設定檔
</div>

```json
{
  // 在保存時自動修覆
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },

  // 在編輯器中靜默錯誤顯示，但仍然應用自動修覆
  "eslint.rules.customizations": [
    { "rule": "@stylistic/*", "severity": "off" }
  ]
}
```

</div>
</div>

<!--
ESLint 也可以是 [click] 格式化工具。

這當然不是新鮮事，因為自從一開始，許多專案就一直在以這種方式使用 ESLint。

雖然這個話題實際上有點爭議，你可能會聽到有人說你應該使用專用的格式化工具，比如 Prettier 或 dprint。對我來說，我認為這完全歸結於那些 ESLint 的風格規則需要大量的維護工作。去年，ESLint 和 TypeScript ESLint 團隊決定從核心中棄用這些風格規則。[click] 然後，社群上發起了 ESLint Stylistic 專案，將所有這些 JS、TS 和 JSX 的風格規則收集到這個組織中，並讓社區繼續維護它們。我繼續使用 ESLint 作為格式化工具，因為我認為它比 Prettier 更靈活和可定制，這歸功於 ESLint 的特性。

[click] 如果你使用 VS Code，你可以設置 `editor.codeActionOnSave` 在保存時自動修覆 eslint 錯誤。並使用 `eslint.rules.customizations` 在你的 IDE 中靜默風格規則，使它們更像一個格式化工具。對於其他代碼編輯器，我相信也有類似的設定檔可以做到這一點。
-->

---

# <span op50>ESLint 也可以是...</span> <b v-click font-800>遷移工具 Codemod</b>

<div v-click>

<repo name="antfu/eslint-plugin-command" />

<video src="/eslint-plugin-command-half.mov" mt-4 w-130 saturate-110 rounded shadow border="~ main" controls />

</div>

<!--
一個 ESLint 規則本質上是一個函數，它接收代碼和 AST，並報告錯誤以及可選的修覆信息。

這意味著 ESLint 也可以是一個很好的 [click] codemod 工具。

例如，[click] 我制作了 `eslint-plugin-command` 來進行按需的微型 codemod。

正如你在視頻中看到的，我們可以在箭頭函數上方放置一個魔法註釋 `to-function`。保存後，箭頭函數將自動轉換為函數聲明，而無需手動移動代碼。

類似地，你也可以使用 `to-arrow` 將其轉換回箭頭函數，使用 `keep-sorted` 對對象或數組進行排序，使用 `keep-unique` 確保數組唯一... 等等。

如果你了解一些 AST 的知識，撰寫你自己的一次性 codemod 規則來遷移你的代碼庫也不難。
-->

---

# <span op50>ESLint 也可以是...</span> <b v-click font-800>多種語言的 Linter</b>

<div scale-75 origin-left-top mb--28 mt--3 class="[&_td]:py1 [&_table]:w-130%" v-click="2">
<v-clicks>

| 語言 | 外掛 | 維護者 |
| --- | --- | --- |
| <span i-catppuccin-markdown inline-block scale-110 translate-y-0.5 mr1 /> Markdown | [`@eslint/markdown`](https://github.com/eslint/markdown) | {@eslint} |
| <span i-catppuccin-css inline-block scale-110 translate-y-0.5 mr1 /> CSS | [`@eslint/css`](https://github.com/eslint/css) | {@eslint} |
| <span i-catppuccin-typescript inline-block scale-110 translate-y-0.5 mr1 /> TypeScript | [`@typescript-eslint`](https://typescript-eslint.io) | {@typescript-eslint} {@bradzacher} {@JoshuaKGoldberg} |
| <span i-catppuccin-vue inline-block scale-110 translate-y-0.5 mr1 /> Vue | [`eslint-plugin-vue`](https://github.com/vuejs/eslint-plugin-vue) | {@ota-meshi} {@vuejs} |
| <span i-catppuccin-svelte inline-block scale-110 translate-y-0.5 mr1 /> Svelte | [`eslint-plugin-svelte`](https://github.com/sveltejs/eslint-plugin-svelte) | {@ota-meshi} {@sveltejs} |
| <span i-catppuccin-astro inline-block scale-110 translate-y-0.5 mr1 /> Astro | [`eslint-plugin-astro`](https://github.com/ota-meshi/eslint-plugin-astro) | {@ota-meshi} |
| <span i-catppuccin-json inline-block scale-110 translate-y-0.5 mr1 /> JSON | [`eslint-plugin-jsonc`](https://github.com/ota-meshi/eslint-plugin-jsonc) | {@ota-meshi} |
| <span i-catppuccin-yaml inline-block scale-110 translate-y-0.5 mr1 /> YAML | [`eslint-plugin-yml`](https://github.com/ota-meshi/eslint-plugin-yaml) | {@ota-meshi} |
| <span i-catppuccin-toml invert hue-rotate-180 inline-block scale-110 translate-y-0.5 mr1 /> TOML | [`eslint-plugin-toml`](https://github.com/ota-meshi/eslint-plugin-toml) | {@ota-meshi} |
| <span i-catppuccin-graphql inline-block scale-110 translate-y-0.5 mr1 /> GraphQL | [`graphql-eslint`](https://github.com/dimaMachina/graphql-eslint) | {@dimaMachina} |
| <span i-catppuccin-html inline-block scale-110 translate-y-0.5 mr1 /> HTML | [`html-eslint`](https://github.com/yeonjuan/html-eslint) | {@yeonjuan} |
| <span i-catppuccin-markdown-mdx inline-block scale-110 translate-y-0.5 mr1 /> MDX | [`eslint-mdx`](https://github.com/mdx-js/eslint-mdx) | {@JounQin} |
| <span i-catppuccin-prettier inline-block scale-110 translate-y-0.5 mr1 /> 其他格式 | [`eslint-plugin-format`](https://github.com/antfu/eslint-plugin-format) | {@antfu} |

</v-clicks>
</div>
<div v-click absolute right-15 bottom-15 bg-main border="~ main rounded-xl" text-purple p3 px4>
<a href="https://github.com/eslint/rfcs/blob/main/designs/2022-languages/README.md" target="_blank">
<span text-xl font-bold>ESLint RFC #99</span><br>
<span op75>ESLint Language Plugins</span>
</a>
</div>

---
layout: fact
---

# One for All{.important-text-3em}

<div text-lime text-2xl my2>一個設定檔適配 <b italic font-bold>所有</b> 專案</div>
<div text-purple text-2xl my2>一個工具適配 <b italic font-bold>所有</b> 需求</div>

<!--
總結一下今天的主題，我想 ESLint 的 One for All 可以體現在兩個方面。
你可以擁有一個設定檔適用於各種不同的專案，
然後一個工具處理所有與代碼檢查和修改相關的事情。
-->

---
class: "grid grid-cols-[1fr_1fr] p0 h-full"
clicks: 1
glow: left
---

<div p4 flex="~ col gap-1 items-center justify-center" transition duration-500 :class="$clicks >= 1 ? '' : 'translate-x-65'">

<div mt-4 />

<Repo name="antfu/eslint-config" /> <span flex="~ inline gap-0.5 items-center" text-amber bg-amber:15 px1 rounded text-xs><div i-carbon-star-filled text="[0.8em]" /> 4.1k</span>

</div>

<div
  bg-hex-5552 p8 border="l main" transition duration-500
  :class="$clicks >= 1 ? '' : 'translate-x-100%'"
>
<div scale-70 origin-left-top w-160 mb--100 mr--40>

# @antfu/eslint-config

<div mb-10>

[![代碼風格](https://antfu.me/badge-code-style.svg)](https://github.com/antfu/eslint-config)

</div>

## 安裝向導

<div mt-2 />

```bash
npx @antfu/eslint-config@latest
```

<div mt-6 />

## 特性

<div mt-2 />

- 自動修覆格式 <sup>旨在獨立使用，**無需** Prettier</sup>
- 合理的默認設置，最佳實踐，只需一行設定檔
- 支持 TypeScript、JSX、Vue、JSON、YAML、Toml、Markdown，開箱即用
- 帶有強主見，但高度可定制
- 輕松組合通過 Flat Config 進行組合
- 可選支持 React、Svelte、UnoCSS、Astro、Solid
- 可選格式化器支持格式化 CSS、HTML、XML 等
- **風格原則**：閱讀最簡，穩定 diff，一致性
  - 排序的匯入，尾隨逗號
  - 單引號，無分號
  - 使用 ESLint Stylistic
- 默認尊重 `.gitignore`

</div>
</div>

<!--
如果你想了解更多，可以查看我的個人 ESLint 設定檔，其中使用了我今天提到的所有技巧。

雖然最初這個設定檔主要是用於我的個人專案，我並沒有打算被其他專案使用。後來慢慢變得如此受歡迎，說實話我也有些受寵若驚。它在 GitHub 上有 4.1K Star，並且在 GitHub 上被將近 4 萬個開源倉庫使用。

這裏也要感謝 三咲智子 為提供了最初的 eslint-config 架構設計，讓我鉆入這個坑，研究出了這些實踐。

[click] 我在這裏介紹我的設定檔並不是想要安利你用它，但主要是希望它能成為你構建屬於自己或團隊的，強大且靈活的共享設定檔的一個參考。
-->

---
layout: intro
class: text-center pb-5
glowX: 50
glowY: 120
---

# 謝謝

投影片在 [b&s michael-lo](https://github.com/michael860520/talks)
