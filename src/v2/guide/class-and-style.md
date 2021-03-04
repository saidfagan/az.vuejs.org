---
title: Sinif və stil bağlamaları
type: guide
order: 6
---

Verilənlərə bağlamanın istifadə yollarından biri də elementin siniflərinin və ya inlayn stillərin dəyişilməsidir. Hər ikisi atribut olduğundan bunu etmək üçün `v-bind` istifadə edə bilərik: yekun sətiri direktivdəki ifadə vasitəsi ilə hesablamaq kifayətdir.  Lakin sətir konkatenasiyası ilə işləmək bezdirici və xətaya meyillidir. Bu səbəbdən `v-bind` direktivi `class` və `style` atributları ilə istifadə olunduqda, Vue xüsusi imkanlar təqdim edir. Sətirdən başqa direktivdəki ifadə massiv və ya obyektdə ola bilər.

## HTML siniflərin bağlanması
<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-dynamic-classes?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Dynamic Classes Lesson">Vue School saytındakı pulsuz dərsə bax</a></div>

### Obyektlərin istifadə olunması

Sinifləri dinamik şəkildə dəyişmək üçün `v-bind:class` atributuna obyekt ötürə bilərik:

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Yuxarıdakı ifadə `active` sinfinin olub olmamasının `isActive` xassəsinin [həqiqiliyi (truthiness)](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) ilə müəyyən edildiyini bildirir.

Ötürülən obyektə bir neçə sahə əlavə edərək bir neçə sinifin dəyişməsinə nail ola bilərsiniz. Əlavə olara `v-bind:class`direktivi sadə `class` atributu ilə birgə istifadə oluna bilər. Aşağıdakı şablon:

``` html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

və bu verilənlər üçün:

``` js
data: {
  isActive: true,
  hasError: false
}
```

Renderin nəticəsi belə olacaq:

``` html
<div class="static active"></div>
```

`isActive` və ya `hasError` dəyişsə, siniflərin siyahısı uyğun olaraq dəyişəcək. Məsələn, `hasError` sahəsi `true` olsa, siniflərin siyahısı belə olacaq `"static active text-danger"`.

Bağlanan obyekt inlayn olmağa məvbur deyil:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Renderin nəticəsi eyni olacaq. Həmçinin obyekt qaytaran [hesablanan xassəyə](computed.html) bağlamaq mümkündür. Bu tez-tez istifadə olunan örnəkdir:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### Massivlərin istifadə olunması

`v-bind:class` atributuna siniflərin siyahısını massiv şəklində də ötürmək olar:

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Renderin nəticəsi belə olacaq:

``` html
<div class="active text-danger"></div>
```

Şərtdən asılı olaraq sinifin istifadə olunub olunmamasını ternar operatorla tənzimləyə bilərsiniz:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

Bu ifadə `errorClass` sinfini həmişə, `activeClass` sinfini isə yalnız `isActive` həqiqi olduqda əlavə edəcək.

Lakin bir neçə belə ifadə istifadə edilməsi uzun ola bilər. Ona görə də massivlərdə obyekt istifadə edilməsi mümkündür:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### With Components

> Bu bölmə [Vue komponentləri](components.html) bilməyinizi tələb edir. İndi buraxıb sonradan bu bölməyə qayıda bilərsiniz.

When you use the `class` attribute on a custom component, those classes will be added to the component's root element. Existing classes on this element will not be overwritten.

For example, if you declare this component:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Then add some classes when using it:

``` html
<my-component class="baz boo"></my-component>
```

The rendered HTML will be:

``` html
<p class="foo bar baz boo">Hi</p>
```

The same is true for class bindings:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

When `isActive` is truthy, the rendered HTML will be:

``` html
<p class="foo bar active">Hi</p>
```

## Binding Inline Styles

### Object Syntax

The object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case (use quotes with kebab-case) for the CSS property names:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Again, the object syntax is often used in conjunction with computed properties that return objects.

### Array Syntax

The array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Auto-prefixing

When you use a CSS property that requires [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) in `v-bind:style`, for example `transform`, Vue will automatically detect and add appropriate prefixes to the applied styles.

### Multiple Values

> 2.3.0+

Starting in 2.3.0+ you can provide an array of multiple (prefixed) values to a style property, for example:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

This will only render the last value in the array which the browser supports. In this example, it will render `display: flex` for browsers that support the unprefixed version of flexbox.
