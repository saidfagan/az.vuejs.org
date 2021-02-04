---
title: Şablon sintaksisi
type: guide
order: 4
---

Vue.js render olunmuş DOM-u Vue surətinin verilənlərinə birləşdirən HTML-ə əsaslanmış şablon sintaksisi istifadə edir. Hər bir Vue.js şablonu brauzerlər və HTML parserlər tərəfindən pars oluna bilən düzgün HTML koddur.

Səhnə arxasında Vue şablonları virtual DOM-un render funksiyalarına kompliyasiya edir. Tətbiqin vəziyyəti dəyişəndə Vue reaktivlik sisteminin köməyi ilə düşünülmüş şəkildə yenidən render olunmalı komponentlərin minimal sayını təyin edə bilir və DOM-da minimal sayda manipulyasiyalar edir.

Virtual DOM konseptləri ilə tanışsınızsa və JavaScript-in həddsiz gücünü istifadə etməyə üstünlük verirsinizsə şablon yerinə birbaşa JSX dəstəkləyən [render funksiyaları](render-function.html) yaza bilərsiniz.

## İnterpolyasiyalar

### Mətn

Verilənləri bağlamağın ən sadə yolu "Mustache" (ikiqat fiqurlu mötərizə) sintaksisi istifadə edən tekst interpolyasiyasıdır:

``` html
<span>Mesaj: {{ msg }}</span>
```

Fiqurlu mötərizədəki ifadə uyğun verilənlər obyektinin `msg` xassəsi ilə əvəz olunacaq. Həmçinin obyektin `msg` xassəsi dəyişəndə bi ifadə də dəyişəcək.

[v-once direktivi](../api/#v-once) istifadə edərək xassənin dəyişməsini nəzərə almayan birdəfəlik interpolyasiya əldə edə bilərsiniz. Lakin nəzərə alın ki, bu direktiv həmin elementdəki bütün bağlamalara təsir edəcək:

``` html
<span v-once>Bu heç vaxt dəyişməyəcək: {{ msg }}</span>
```

### Xam HTML

İkiqat fiqurlu mötərizə verilənləri HTML yox sadə mətn kimi göstərir. HTML göstərmək üçün [`v-html` direktivi](../api/#v-html) istifadə etməlisiniz:

``` html
<p>Mötərizə istifadə edərək: {{ rawHtml }}</p>
<p>v-html direktivi istifadə edərək: <span v-html="rawHtml"></span></p>
```

{% raw %}
<div id="example1" class="demo">
  <p>Mötərizə istifadə edərək: {{ rawHtml }}</p>
  <p>v-html direktivi istifadə edərək: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
    return {
      rawHtml: '<span style="color: red">Bu qırmızı olmalıdır.</span>'
    }
  }
})
</script>
{% endraw %}

`span` elementinin məzmunu sadə HTML olan `rawHtml` xassəsinin dəyəri ilə əvəz olunacaq - verilənlərin bağlamaları nəzərə alınmayacaq. Nəzərə alın ki, Vue-nun şablon mühərriki sətirlərə əsaslanmadığından iç içə şablonlar yaratmaq üçün `v-html` direktivindən istifadə edə bilməzsiniz. Onun yerinə təkrar istifadə və kompozisiya üçün komponentlərdən istifadə edə bilərsiniz.

<p class="tip">Öz saytınızda ixtiyari HTML-in render olunması [XSS zəyifliyinə](https://en.wikipedia.org/wiki/Cross-site_scripting) səbəb ola biləcəyinə görə təhlükəli ola bilər. HTML interpolyasiyasını yalnız güvənli kod üçün istifadə edin və istifadəçi göndərdiyi məzmun üçün **heç vaxt** tətbiq etməyin.</p>

### Atributlar

Fiqurlu mötərizə HTML atributların daxilində istifadə oluna bilməz. Əvəzinə [`v-bind` direktivi](../api/#v-bind) istifadə edin:

``` html
<div v-bind:id="dynamicId"></div>
```

Bul tipli atributlarda (hansılardakı atributun olması artıq `true` bildirir) `v-bind` bir qədər fərqli işləyir. Aşağıdakı nümunədə:

``` html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

Əgər `isButtonDisabled` xassəsinin dəyəri `null`, `undefined`, və ya `false`-dursa, `disabled` atributu render olunmuş `<button>` elementinə əlavə olunmayacaq.

### JavaScript ifadələrinin istifadə olunması

İndiyə qədər verilənləri yalnız şablonlardakı sadə xassələr ilə bağlayırdıq. Ancaq Vue.js bağlamalarda JavaScript ifadələrini tam dəstəkləyir:

``` html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Bu ifadələr cari Vue surətinin görünüş dairəsində adi JavaScript kod kimi yerinə yetiriləcək. Yeganə məhdudiyyət ondan ibarətdir ki, hər bir bağlamada yalnız **bir ifadə** ola bilər, yəni aşağıdakılar **işləməyəcək**:

``` html
<!-- Bu bəyanatdır, ifadə yox: -->
{{ var a = 1 }}

<!-- idarəetmə operatorları həmçinin işləməyəcək, ternar ifadə istifadə edin -->
{{ if (ok) { return message } }}
```

<p class="tip">Şablon ifadələr "qumluq" rejimində işləyir və onların ancaq `Math` və `Date` kimi [məhdud sayda qlobal verilənləri](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9) çağırma hüququ var. Şablon ifadələrdən istifadəçi təyin etmiş qlobal verilənləri də çağırmaq məsləhət görülmür.</p>

## Direktivlər

Direktivlər `v-` ilə başlayan xüsusi atributlardır. Direktivlərin dəyəri **tək JavaScript ifadəsi** olur (daha sonra müzakirə edəcəyimiz `v-for` direktivi istisna olmaqla). Direktivin işi dəyəri dəyişdiyi zaman lazımi dəyişiklikləri DOM-a tətbiq etməkdən ibarətdir. Gəlin giriş bölməsində gördüyümüz nümunəyə yenidən nəzər yetirək:

``` html
<p v-if="seen">Görünən element</p>
```

Burada `v-if` direktivi `seen` ifadəsinin true və ya false olmasından asılı olaraq `<p>` elementini göstərəcək və ya gizlədəcək.

### Arqumentlər

Bəzi direktivlər adından iki nöqtə işarəsi ilə ayırılmış "arqumentlər" qəbul edir. Məsələn, `v-bind` direktivi HTML atributu reaktiv şəkildə dəyişməyə imkan verir:

``` html
<a v-bind:href="url"> ... </a>
```

Burada `href` `v-bind` direktivinə elementin `href` atributunu `url` ifadəsinə bağlamağını deyən direktiv arqumentidir.

Başqa nümunə kimi `v-on` direktivini göstərmək olar. O, DOM hadisələrini dinləyir:

``` html
<a v-on:click="doSomething"> ... </a>
```

Burada arqument dinlədiyimiz hadisənin adıdır. Hadisələrin emalı haqqında da daha ətraflı danışacayıq.

### Dinamik arqumentlər

> 2.6.0 və daha yeni versiyalarda

2.6.0 versiyadan başlayaraq kvadrat mötərizələrin arasına yazmaqla JavaScript ifadələri direktiv arqumenti kimi ötürmək olar:

``` html
<!--
Nəzərə alın ki, "Dinamik arqument ifadələlərinə aid məhdudiyyətlər" 
bölməsində qeyd olunduğu kimi ötürülən ifadələr üçün bəzi məhdudiyyətlər var.
-->
<a v-bind:[attributeName]="url"> ... </a>
```

Burada `attributeName` JavaScript ifadəsi kimi hesablanacaq, və onun hesablanmış dəyəri arqument üçün yekun dəyər kimi istifadə olunacaq. Məsələn, Vue surətinin adı `attributeName` olan dəyəri `"href"` olan xassəsi varsa onda yuxarıdakı direktiv buna `v-bind:href` bərabər olacaq.

Analoji olaraq dinamik arqumentləri hadisə işləyicisini dinamik olaraq hadisə adına bağlamaq üçün istifadə edə bilərsiniz:

``` html
<a v-on:[eventName]="doSomething"> ... </a>
```

Bu nümunədə `eventName` ifadəsinin dəyəri `"focus"` olsa `v-on:[eventName]` direktivi `v-on:focus`-a bərabər olacaq.

#### Dinamik arqument dəyərlərinə aid məhdudiyyətlər

Dinamik arqumentlərin hesablandıqdan sonra sətir olması gözlənilir (`null`-dan başqa). `null` xüsusi dəyəri bağlamanı açıq şəkildə silinməsi üçün istifadə oluna bilər. Hər hansı sətir olmayan dəyər xəbərdarlığa səbəb olacaq.

#### Dinamik arqument ifadələlərinə aid məhdudiyyətlər

HTML atribut adlarında boşluq və dırnaq kimi bəzi xüsusi simvolların istifadəsi qadağan olunub. Buna görə dinamik arqument ifadələlərinə aid bəzi məhdudiyyətlər var. Məsələn aşağıdakı ifadə düzgün deyil:

``` html
<!-- Bu kompilyator xəbərdarlığına səbəb olacaq. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

Burada həll yolu kimi boşluq və dırnaqsız ifadənin istifadə olunması və ya mürəkkəb ifadənin hesablanan xassə ilə əvəz olunmasını.

DOM-dakı şablonları istifadə edərkən (yəni bir başa HTML faylda yazılmış şablonları) atribut adlarının bpyük hərflərlə yazmaqdan yayınmaq lazımdır, çünki brauzerlər atribut adlarındakı simvolları kiçik hərflərə dəyişəcək:

``` html
<!--
DOM-dakı şablonlarda bu bəyanat v-bind:[someattr] ilə əvəz olunacaq.
Surətinizin kodunda "someattr" xassəsi yoxdursa bu kod işləməyəcək.
-->
<a v-bind:[someAttr]="value"> ... </a>
```

### Dəyişdirici

Dəyişdiricilər (modifiers) direktivdən nöqtə ilə ayrılmış, direktivin fərqli üsulla bağlanmalı olduğunu bildirən xüsusi postfikslərdir. Məsələn, `.prevent` dəyişdiricisi `v-on` direktivində istifadə olunduqda hadisənin `event.preventDefault()` çağırmasını təmin edir:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

Dəyişdiricilərə aid başqa nümunələri daha sonra görəcəksiniz, [`v-on`](events.html#Event-Modifiers) və [`v-model`](forms.html#Modifiers) direktivlərini araşdıranda.

## Shorthands

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building a [SPA](https://en.wikipedia.org/wiki/Single-page_application), where Vue manages every template. Therefore, Vue provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

They may look a bit different from normal HTML, but `:` and `@` are valid characters for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
