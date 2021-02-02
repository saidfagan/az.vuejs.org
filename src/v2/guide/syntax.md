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

## Directives

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **a single JavaScript expression** (with the exception of `v-for`, which will be discussed later). A directive's job is to reactively apply side effects to the DOM when the value of its expression changes. Let's review the example we saw in the introduction:

``` html
<p v-if="seen">Now you see me</p>
```

Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `seen`.

### Arguments

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:

``` html
<a v-bind:href="url"> ... </a>
```

Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`.

Another example is the `v-on` directive, which listens to DOM events:

``` html
<a v-on:click="doSomething"> ... </a>
```

Here the argument is the event name to listen to. We will talk about event handling in more detail too.

### Dynamic Arguments

> New in 2.6.0+

Starting in version 2.6.0, it is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets:

``` html
<!--
Note that there are some constraints to the argument expression, as explained
in the "Dynamic Argument Expression Constraints" section below.
-->
<a v-bind:[attributeName]="url"> ... </a>
```

Here `attributeName` will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument. For example, if your Vue instance has a data property, `attributeName`, whose value is `"href"`, then this binding will be equivalent to `v-bind:href`.

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

``` html
<a v-on:[eventName]="doSomething"> ... </a>
```

In this example, when `eventName`'s value is `"focus"`, `v-on:[eventName]` will be equivalent to `v-on:focus`.

#### Dynamic Argument Value Constraints

Dynamic arguments are expected to evaluate to a string, with the exception of `null`. The special value `null` can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

#### Dynamic Argument Expression Constraints

Dynamic argument expressions have some syntax constraints because certain characters, such as spaces and quotes, are invalid inside HTML attribute names. For example, the following is invalid:

``` html
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

The workaround is to either use expressions without spaces or quotes, or replace the complex expression with a computed property.

When using in-DOM templates (i.e., templates written directly in an HTML file), you should also avoid naming keys with uppercase characters, as browsers will coerce attribute names into lowercase:

``` html
<!--
This will be converted to v-bind:[someattr] in in-DOM templates.
Unless you have a "someattr" property in your instance, your code won't work.
-->
<a v-bind:[someAttr]="value"> ... </a>
```

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

You'll see other examples of modifiers later, [for `v-on`](events.html#Event-Modifiers) and [for `v-model`](forms.html#Modifiers), when we explore those features.

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
