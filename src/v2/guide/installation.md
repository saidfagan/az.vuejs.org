---
title: Quraşdırılması
type: guide
order: 1
vue_version: 2.5.16
gz_size: "30.90"
---

### Uyğunluq haqqında qeyd

Vue IE8 və daha köhnə versiyaları **dəstəkləmir**, çünki o, IE8-də heç bir halda emulyasiya oluna bilməyən ECMAScript 5 xüsusiyyətlərini istifadə edir. Lakin o, [ECMAScript 5 ilə uzlaşan](https://caniuse.com/#feat=es5) bütün brauzerləri dəstəkləyir.

### Semantik versiyalaşdırma

Vue sənədləşmiş xüsusiyyətləri və davranışları üçün bütün rəsmi proyektlərində [semantik versiyalaşdırmanı](https://semver.org/) təqib edir. Sənədləşməmiş davranışlar və üzə çıxan daxili məntiq üçün isə dəyişikliklər [reliz qeydlərində](https://github.com/vuejs/vue/releases) təsvir olunur.

### Reliz qeydləri

Son stabil versiya: {{vue_version}}

Bütün versiyalar üçün ətraflı reliz qeydləri [GitHub](https://github.com/vuejs/vue/releases)-da əlçatandır.

## Vue developer alətləri

Vue istifadə etdikdə brauzerinizdə [Vue developer alətləri genişləndirməsini](https://github.com/vuejs/vue-devtools#vue-devtools) quraşdırmağı məsləhət görürük. Bu alət Vue tətbiqinizi daha rahat mühitdə təhlil və debaq etməyə şərait yaradacaq.

## `<script>` teqi ilə qoşmaq

Sadəcə yükləyin və script teqi ilə qoşun. `Vue` qlobal dəyişən kimi qeydə alınacaq.

<p class="tip">Development zamanı kiçildilmiş versiyadan istifadə etməyin. Ümumi xətalar üçün bütün xəbərdarlıqları qaçıra bilərsiniz!</p>

<div id="downloads">
  <a class="button" href="/js/vue.js" download>Development versiyası</a><span class="light info">Uzun xəbərdarlıqlar və debaq rejimi ilə</span>

  <a class="button" href="/js/vue.min.js" download>Prodakşn versiyası</a><span class="light info">Qısaldılmış xəbərdarlıqlar ilə, {{gz_size}}KB min+gzip</span>
</div>

### CDN

Prototiplər yaratmaq və ya öyrənmək üçün son versiyanı belə istifadə elə bilərsiniz:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Prodakşnda yeni versiyalar gözlənilməz xətalara səbəb olmaması üçün konkret versiya nömrəsi yazmaq məsləhət görülür:

``` html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>
```

ES modulları istifadə edirsinizsə onlarla uzlaşan yığma istifadə edə bilərsiniz:

``` html
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.0/dist/vue.esm.browser.js'
</script>
```

Bu [linkdən](https://cdn.jsdelivr.net/npm/vue/) NPM paketin ilkin koduna baxa bilərsiniz.

Vue həmçinin [unpkg](https://unpkg.com/vue@{{vue_version}}/dist/vue.js) və [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) saytlarında əlçatandır (cdnjs bir müddət sonra sinxronizasiya edir, ən son versiya bu saytda olmaya bilər).

[Vue-nun müxtəlif yığmaları](#Explanation-of-Different-Builds) haqqında mütləq oxuyun və öz saytınızda `vue.js`-i `vue.min.js` ilə əvəz edərək **prodakşn versiyasını** istifadə edin. Bu development yığmasından fərqli olaraq sürətli və optimallaşdırılmış yığmadır.

## NPM

Vue ilə irimiqyaslı tətbiqlər yazanda NPM istifadə etmək məsləhət görülür. O, [Webpack](https://webpack.js.org/) və [Browserify](http://browserify.org/) kimi yığma alətləri ilə yaxşı uzlaşır. Vue həmçinin [tək fayllı komponentlər](single-file-components.html) tərtib etmək üçün alətlər təqdim edir.

``` bash
# son stabil versiya
$ npm install vue
```

## CLI

Yüksək keyfiyyətli tək səhifəli tətbiqlər yaratmaq üçün Vue [rəsmi komanda sətiri interfeysi (CLI)](https://github.com/vuejs/vue-cli). CLI müasir frontend development üçün hər bir alətlə təmin edir. Bu alətlə cəmi bir neçə dəqiqəyə qaynar yenidən yükləməli, yadda saxlama zamanı kodu lint edən və prodakşna hazır tətbiq yaratmaq mümkündür. Əlavə məlumat üşün [Vue CLI dokumentasiyasına](https://cli.vuejs.org) nəzər yetirin.

<p class="tip">CLI Node.js və ona bağlı yığma alətləri haqqında biliyiniz olmasını tələb edir. Əgər sizin Vue və ya ümumiyyətlə frontend developmentdə təcrübəniz yoxdursa <a href="./">bələdçinin</a> yığma alətləri olmayan hissələrəni oxuyun, daha sonra CLI istifadə edə bilərsiniz

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/real-world-vue-js/vue-cli" target="_blank" rel="sponsored noopener" title="Vue CLI">Vue Mastery saytında izahlı videoya bax</a></div>

## Fərqli yığmaların açıqlaması

[NPM paketin `dist/` qovluğunda](https://cdn.jsdelivr.net/npm/vue/dist/) Vue.js-in çoxlu fərqli yığmalarını görə bilərsiniz. Ağağıdakı cədvəldə onlar arasındakı fərqlər cəmləşdirilib:

| | UMD | CommonJS | ES Module (for bundlers) | ES Module (for browsers) |
| --- | --- | --- | --- | --- |
| **Tam** | vue.js | vue.common.js | vue.esm.js | vue.esm.browser.js |
| **Runtime-only** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js | - |
| **Tam (prodakşn)** | vue.min.js | - | - | vue.esm.browser.min.js |
| **Runtime-only (prodakşn)** | vue.runtime.min.js | - | - | - |

### Terminlər

- **Tam**: tərkibində rantaym və kompilyator olan yığma.

- **Kompilyator**: sətir şablonlarını JavaScript render funksiyalarına çevirən kod.

- **Runtime**: Vue obyektləri yaradan, render edən, virtual DOM-u dəyişən kod. Qısaca kompilyatordan başqa hər şey.

- **[UMD](https://github.com/umdjs/umd)**: UMD yığmaları birbaşa brauzerdə `<script>` teqində istifadə oluna bilər. jsDelivr CDN saytındakı ([https://cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue)) defolt fayl (`vue.js`) rantaym və kompilyatoru olan UMD yığmasıdır.

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS yığmaları [browserify](http://browserify.org/) və [webpack 1](https://webpack.github.io) kimi köhnə yığma alətlər üçün nəzərdə tutulub. Bu alətlər üçün defolt fayl (`pkg.main`) tərkibində yalnız rantaym olan CommonJS yığmasıdır (`vue.runtime.common.js`).

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: starting in 2.6 Vue provides two ES Modules (ESM) builds:
- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: 2.6 və daha yeni versiyalarda Vue iki ES Modules (ESM) yığmaları təqdim edir:

  - yığma alətləri üçün ESM: [webpack 2](https://webpack.js.org) və [Rollup](https://rollupjs.org/) kimi müasir yığma alətləri üçün nəzərdə tutulub. ESM format is designed to be statically analyzable so the bundlers can take advantage of that to perform "tree-shaking" and eliminate unused code from your final bundle. The default file for these bundlers (`pkg.module`) is the Runtime only ES Module build (`vue.runtime.esm.js`).

  - ESM for browsers (2.6+ only): intended for direct imports in modern browsers via `<script type="module">`.

### Runtime + Compiler vs. Runtime-only

If you need to compile templates on the client (e.g. passing a string to the `template` option, or mounting to an element using its in-DOM HTML as the template), you will need the compiler and thus the full build:

``` js
// this requires the compiler
new Vue({
  template: '<div>{{ hi }}</div>'
})

// this does not
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

When using `vue-loader` or `vueify`, templates inside `*.vue` files are pre-compiled into JavaScript at build time. You don't really need the compiler in the final bundle, and can therefore use the runtime-only build.

Since the runtime-only builds are roughly 30% lighter-weight than their full-build counterparts, you should use it whenever you can. If you still wish to use the full build instead, you need to configure an alias in your bundler:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': require.resolve('vue/dist/vue.esm.js')
    })
  ]
})
```

#### Browserify

Add to your project's `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

#### Parcel

Add to your project's `package.json`:

``` js
{
  // ...
  "alias": {
    "vue" : "./node_modules/vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Mode

Development/production modes are hard-coded for the UMD builds: the un-minified files are for development, and the minified files are for production.

CommonJS and ES Module builds are intended for bundlers, therefore we don't provide minified versions for them. You will be responsible for minifying the final bundle yourself.

CommonJS and ES Module builds also preserve raw checks for `process.env.NODE_ENV` to determine the mode they should run in. You should use appropriate bundler configurations to replace these environment variables in order to control which mode Vue will run in. Replacing `process.env.NODE_ENV` with string literals also allows minifiers like UglifyJS to completely drop the development-only code blocks, reducing final file size.

#### Webpack

In Webpack 4+, you can use the `mode` option:

``` js
module.exports = {
  mode: 'production'
}
```

But in Webpack 3 and earlier, you'll need to use [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Use [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Apply a global [envify](https://github.com/hughsk/envify) transform to your bundle.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Also see [Production Deployment Tips](deployment.html).

### CSP environments

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The full build depends on this feature to compile templates, so is unusable in these environments.

On the other hand, the runtime-only build is fully CSP-compliant. When using the runtime-only build with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), your templates will be precompiled into `render` functions which work perfectly in CSP environments.

## Dev Build

**Important**: the built files in GitHub's `/dist` folder are only checked-in during releases. To use Vue from the latest source code on GitHub, you will have to build it yourself!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Only UMD builds are available from Bower.

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

All UMD builds can be used directly as an AMD module.
