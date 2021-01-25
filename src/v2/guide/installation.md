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

  - yığma alətləri üçün ESM: [webpack 2](https://webpack.js.org) və [Rollup](https://rollupjs.org/) kimi müasir yığma alətləri üçün nəzərdə tutulub. ESM formatı statik analiz imkanı ilə yaradılıb. Yığma alətləri bundan faydalanıb yekun yığmadan istifadə olunmayan kodu "tree-shaking" metodu ilə silə bilər. Bu yığma alətləri üçün defolt fayl (`pkg.module`) tərkibində yalnız rantaym olan ESM yığmalarıdır (`vue.runtime.esm.js`).

  - brauzerlər üçün ESM (yalnız 2.6+): müasir brauzerlərdə `<script type="module">` vasitəsi ilə istifadə üçün nəzərdə tutulub.

### Rantaym + Kompilyator və yalnız rantaym yığmalarının fərqi

Əgər şablon kompilyasiya etməyə ehtiyac duysanız (məsələn, `template` opsiyasına sətir ötürsəniz, və ya elementin DOM-dakı HTML-ni şablon kimi istifadə edərək ona qoşulsanız), sizə şablon kompilyatoru lazım olacaq. Ona görə tam yığma istifadə etmək məsləhət görülür:

``` js
// bu şablon kompilyatoru tələb edir
new Vue({
  template: '<div>{{ hi }}</div>'
})

// bu isə tələb etmir
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

`vue-loader` və ya `vueify` istifadə etdikdə, `*.vue` fayllarının daxilindəki şablonlar yığma zamanı JavaScript koda kompilyasiya olur. Yekun yığmada kompilyatora ehtiyac qalmır, ona görə də tərkibində yalnız rantaym olan yığma istifadə etmək olar .

Since the runtime-only builds are roughly 30% lighter-weight than their full-build counterparts, you should use it whenever you can. If you still wish to use the full build instead, you need to configure an alias in your bundler:
Yalnız rantaym olan yığmaların həcmi tam yığmaların həcmindən təxmini 30% az olduğuna görə mümkün olanda onları istifadə etmək lazımdır. Tam yığma istifadə etmək istəyirsinizsə yığma alətində psevdonim quraşdırmalısınız:

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

Proyektiniz `package.json` faylına əlavə edin:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

#### Parcel

Proyektiniz `package.json` faylına əlavə edin:

``` js
{
  // ...
  "alias": {
    "vue" : "./node_modules/vue/dist/vue.common.js"
  }
}
```

### Development və prodakşn rejimləri arasında fərq

UMD yığmalarda development və prodakşn rejimləri əvvəlcədən təyin olunub: sıxılmamış fayl development üçündür, sıxılmış fayl isə prodakşn üçün.

CommonJS və ES Module yığmaları yığma alətləri üçün nəzərdə tutulub. Buna görə də onların sıxılmış versiyaları yoxdur. Yekun yığmada faylların sıxılmasını özünüz etməlisinz. 

Rejimi təyin etmək üçün CommonJS və ES Module yığmaları həmçinin `process.env.NODE_ENV` verilənini yoxlayır. Vue-nun hansı rejimdə işləməsinə nəzarət etmək üçün bu mühit verilənini yığma aləti vasitəsi ilə dəyişmək lazımdır. `process.env.NODE_ENV` verilənini sətir literalı ilə dəyişmək UglifyJS kimi fayl sıxanlara yalnız development zamanı istifadə olunan kodları kəsib yekun fayl həcmini azalatmağa imkan verir.

#### Webpack

In Webpack 4+, you can use the `mode` option:
Webpack 4+ versiyalarda `mode` opsiyasından istifadə edə bilərsiniz:

``` js
module.exports = {
  mode: 'production'
}
```

Lakin Webpack 3 və daha yeni versiyalarda [DefinePlugin](https://webpack.js.org/plugins/define-plugin/) istifadə etməli olacaqsınız:

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

[rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace) istifadə edin:

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

Yığmanızda [envify](https://github.com/hughsk/envify) transformasiyası istifadə edin.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Həmçinin [prodakşna yerləşdirmə məsləhətlərinə](deployment.html) nəzər yetirin.

### CSP mühitləri

Google Chrome Apps kimi bəzi mühitlər Content Security Policy (CSP) əməl olunmasını tələb edir. CSP ifadələrin yerinə yetirilməsi üçün `new Function()` konstruksiyasının istifadəsini qadağan edir. Tam yığmalar şablonları kompilyasiya etmək üçün bu konstruksiyadan istifadə edir, ona görə də bu mühitlərdə istifadə oluna bilməz.

Digər tərəfdən tərkibində yalnız rantaym olan yığmalar CSP mühitlərini tam dəstəkləyir. Belə yığmaları [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) və ya [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple) ilə istifadə etdikdə, şablonlarınız `render` funksiyalarına çeviriləcək, hansıları ki CSP mühitlərində problemsiz işləyir.

## Development yığmaları

**Vacib qeyd**: GitHub-da `/dist` qovluğundakı fayllar yalnız reliz zamanı yenilənir. Vue-nun Github-dakı ən aktual versiyasını istifadə etmək üçün özünüz yığmalısınız!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Bower vasitəsi ilə yalnız UMD yığmaları əlçatandır.

``` bash
# son stabil versiya
$ bower install vue
```

## AMD modul yükləyiciləri

Bütün UMD yığmaları birbaşa AMD modulu kimi istifadə oluna bilər.
