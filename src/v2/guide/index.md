---
title: Introduction
type: guide
order: 2
---

## Vue.js nədir?

Vue (/vjuː/ kimi tələffüz olunur, **view**) istifadəçi interfeysi yaratmaq üçün **proqressiv freymvorkdur**. Digər monolit freymvorklardan fərqli olaraq, Vue-nun tədricən mənimsədilməsi mümkündür. Əsas kitabxana yalnız görünüş qatında fokuslanıb və nəticədə digər kitabxana və proyektlərlə inteqrasiyası asandır. Digər tərəfdən, Vue [müasir alətlər](single-file-components.html) və [əlavə kitabxanalar](https://github.com/vuejs/awesome-vue#components--libraries) ilə birgə istifadə olunduqda, qəliz təksəhifəli tətbiqlərin (Single-Page Application, SPA) yaradılması üçün mükəmməldir.

Başlamamışdan əvvəl Vue haqqında daha çox öyrənmək istəyirsinizsə, biz sizin üçün nümunə proyekt üzərində əsas prinsipləri izah edən <a id="modal-player"  href="#">video hazırlamışıq</a>.

Əgər siz təcrübəli frontend developersinizsə, və Vue-nu digər freymvork və kitabxanalarla müqayisə etmək istəyirsinizsə onda [Digər freymovrklarla müqayisə](comparison.html) bölməsinə nəzər yetirin.

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="sponsored noopener" title="Free Vue.js Course">Vue Mastery saytındakı pulsuz kursa baxın</a></div>

## Başla

<a class="button" href="installation.html">Quraşdırılması</a>

<p class="tip">Rəsmi bələdçi sizin HTML, CSS, və JavaScript-in orta səviyyədə bilməyinizi tələb edir. Əgər sizin frontend developmentdə təcrübəniz ümumiyyətlə yoxdursa onda yeni freymvork öyrənmək yaxşı fikir deyil - əsasları öyrənin və bura qayıdın! Digər freymvorklarla təcrübəyə malik olmaq sizə kömək edə bilər, amma məcbur deyil.</p>

Vue.js-i sınamaq üçün ən asan yol is [Hello World nümunəsinə](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-hello-world) baxmaqdır. Bu nümunəyi başqa tabda açıb bu bələdçidəki nümunələrə uyğun dəyişə bilərsiniz. Və ya <a href="https://github.com/vuejs/vuejs.org/blob/master/src/v2/examples/vue-20-hello-world/index.html" target="_blank" download="index.html" rel="noopener noreferrer"><code>index.html</code> faylı yaradıb</a> Vue-nu aşağıdakı şəkildə əlavə edə bilərsiniz:

``` html
<!-- development versiyası, konsolda faydalı xəbərdarlıqlar göstərir  -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

və ya:

``` html
<!-- prodakşn versiyası, sürət və ölçüsü optimizasiya olunub -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

[Quraşdırma](installation.html) səhifəsi Vue-nun quraşdırılmasının əlavə opsiyalarını təqdim edir. Qeyd: Yeni istifadəçilərin `vue-cli` istifadə etməsi **məsələhət görülmür**, xüsusən bu istifadəçinin Node.js yığma alətləri ilə təcrübəsi yoxdursa. 

Əgər interaktiv dərslərə üstünlük verirsinizsə, onda [Scrimba saytındakı dərslərə](https://scrimba.com/g/gvuedocs) baxa bilərsiniz. Bu dərslər skrinkast və oyun meydanşası qarışığı təqdim edir, istədyiniz zaman fasilə verib ətrafı araşdıra bilərsiniz.

## Deklarativ renderinq

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cQ3QVcr" target="_blank" rel="noopener noreferrer">Bu dərsə Scrimba saytında bax</a></div>

Vue.js-in mərkəzində sadə şablon sintaksisi istifadə edərək məlumatı DOM-a render edən sistem dayanır:

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Salam Vue!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Salam Vue!'
  }
})
</script>
{% endraw %}

Beləliklə, biz ilk Vue tətbiqimizi yaratdıq! Bu sadə sətir şablonu renderinqinə bənzəyir, lakin səhnə arxasında Vue xeyli iş görür. Məlumatlar və DOM bir birinə birləşib və burada hər şey **reaktivdir**. Buna necə əmin olmaq olar? Brauzeriniz JavaScript konsolunu açın (elə indi, bu səhifədə) və `app.message` sahəsini fərqli dəyərə dəyişin. Yuxarıdakı nümunə uyğun olaraq dəyişəcək.

Fikir verin ki, biz HTML-ə bir başa müraciyət etmirik. Vue tətbiqi özünü bir DOM elementinə bərkidir (bu nümunədə `#app`) və onu tam idarə edir. HTML giriş nöqtəsidir, amma qalan hər şey yeni yaradılmış Vue obyektinin içində baş verir.

Mətn interpolyasiyasından başqa, biz element atributlarına aşağıdakı kimi dəyər bağlaya bilərik:

``` html
<div id="app-2">
  <span v-bind:title="message">
    Dinamik bağlanmış `title` atributunu görmək üçün
    mausun kursorunu bunun üzərində bir neçə saniyə saxla!
  </span>
</div>
```
``` js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Bu səhifə ' + new Date().toLocaleString() + ' tarixində yükləndi'
  }
})
```
{% raw %}
<div id="app-2" class="demo">
  <span v-bind:title="message">
    Dinamik bağlanmış `title` atributunu görmək üçün
    mausun kursorunu bunun üzərində bir neçə saniyə saxla!
  </span>
</div>
<script>
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'Bu səhifə ' + new Date().toLocaleString() + ' tarixində yükləndi'
  }
})
</script>
{% endraw %}

Burada biz yeni bir şeylə qarşılaşırıq. Gördüyünüz `v-bind` atributu **direktiv** adlanır. Direktivlər Vue-ya aid olduqları üçün `v-` ilə başlayır. Gördüyümüz kimi onlar DOM-a xüsusi davranış əlavə edir. Bu nümunədə direktiv `title` atributunun Vue obyektindəki `message` xassəsi ilə eyni olmasını təmin edir.

Yenidən JavaScript konsolunu açıb `app2.message = 'yeni mesaj'` daxil etsəniz, bağlantısı olan HTML kodun (`title` atributu) uyğun olaraq dəyişdiyini görə biləciniz.

## Şərt və dövr direktivləri

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQe4SJ" target="_blank" rel="noopener noreferrer">Bu dərsə Scrimba saytında bax</a></div>

Elementin görünüb-görünməməsini dəyişmək də olduqca asandır:

``` html
<div id="app-3">
  <span v-if="seen">Görünən element</span>
</div>
```

``` js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```

{% raw %}
<div id="app-3" class="demo">
  <span v-if="seen">Görünən element</span>
</div>
<script>
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
</script>
{% endraw %}

Konsolda `app3.seen = false` daxil edin. Mesaj gizlənəcək.

Bu nümunə verilənlərin mətn və atributlardan başqa DOM-un **strukturu** ilə də bağlanmasının mümkün olduğunu göstərir. Əlavə olaraq, Vue-da elementlər DOM-a əlavə olunanda, dəyişəndə və silinəndə onlara [keçid effektləri](transitions.html) tətbiq edə bilən güclü keçid effektləri sistemi var.

Müxtəlif funksiyaları olan bir neçə direktiv də var. Məsələn, `v-for` direktivi massivin elementlərini göstərməyə imkan verir:

``` html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
``` js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript öyrən' },
      { text: 'Vue öyrən' },
      { text: 'Yeni bir şey yarat' }
    ]
  }
})
```
{% raw %}
<div id="app-4" class="demo">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript öyrən' },
      { text: 'Vue öyrən' },
      { text: 'Yeni bir şey yarat' }
    ]
  }
})
</script>
{% endraw %}

Konsola `app4.todos.push({ text: 'New item' })` daxil edin. Siyahıya yeni elementin əlavə olunduğunu görə bilərsiniz.

## Handling User Input

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/czPNaUr" target="_blank" rel="noopener noreferrer">Bu dərsə Scrimba saytında bax</a></div>

To let users interact with your app, we can use the `v-on` directive to attach event listeners that invoke methods on our Vue instances:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app-5" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Note that in this method we update the state of our app without touching the DOM - all DOM manipulations are handled by Vue, and the code you write is focused on the underlying logic.

Vue also provides the `v-model` directive that makes two-way binding between form input and app state a breeze:

``` html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
{% raw %}
<div id="app-6" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
</script>
{% endraw %}

## Composing with Components

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQVkA3" target="_blank" rel="noopener noreferrer">Try this lesson on Scrimba</a></div>

The component system is another important concept in Vue, because it's an abstraction that allows us to build large-scale applications composed of small, self-contained, and often reusable components. If we think about it, almost any type of application interface can be abstracted into a tree of components:

![Component Tree](/images/components.png)

In Vue, a component is essentially a Vue instance with pre-defined options. Registering a component in Vue is straightforward:

``` js
// Define a new component called todo-item
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue(...)
```

Now you can compose it in another component's template:

``` html
<ol>
  <!-- Create an instance of the todo-item component -->
  <todo-item></todo-item>
</ol>
```

But this would render the same text for every todo, which is not super interesting. We should be able to pass data from the parent scope into child components. Let's modify the component definition to make it accept a [prop](components.html#Props):

``` js
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

Now we can pass the todo into each repeated component using `v-bind`:

``` html
<div id="app-7">
  <ol>
    <!--
      Now we provide each todo-item with the todo object
      it's representing, so that its content can be dynamic.
      We also need to provide each component with a "key",
      which will be explained later.
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
``` js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
{% raw %}
<div id="app-7" class="demo">
  <ol>
    <todo-item v-for="item in groceryList" v-bind:todo="item" :key="item.id"></todo-item>
  </ol>
</div>
<script>
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
</script>
{% endraw %}

This is a contrived example, but we have managed to separate our app into two smaller units, and the child is reasonably well-decoupled from the parent via the props interface. We can now further improve our `<todo-item>` component with more complex template and logic without affecting the parent app.

In a large application, it is necessary to divide the whole app into components to make development manageable. We will talk a lot more about components [later in the guide](components.html), but here's an (imaginary) example of what an app's template might look like with components:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### Relation to Custom Elements

You may have noticed that Vue components are very similar to **Custom Elements**, which are part of the [Web Components Spec](https://www.w3.org/wiki/WebComponents/). That's because Vue's component syntax is loosely modeled after the spec. For example, Vue components implement the [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) and the `is` special attribute. However, there are a few key differences:

1. The Web Components Spec has been finalized, but is not natively implemented in every browser. Safari 10.1+, Chrome 54+ and Firefox 63+ natively support web components. In comparison, Vue components don't require any polyfills and work consistently in all supported browsers (IE9 and above). When needed, Vue components can also be wrapped inside a native custom element.

2. Vue components provide important features that are not available in plain custom elements, most notably cross-component data flow, custom event communication and build tool integrations.

Although Vue doesn't use custom elements internally, it has [great interoperability](https://custom-elements-everywhere.com/#vue) when it comes to consuming or distributing as custom elements. Vue CLI also supports building Vue components that register themselves as native custom elements.

## Ready for More?

We've briefly introduced the most basic features of Vue.js core - the rest of this guide will cover them and other advanced features with much finer details, so make sure to read through it all!

<div id="video-modal" class="modal"><div class="video-space" style="padding: 56.25% 0 0 0; position: relative;"><iframe src="https://player.vimeo.com/video/247494684?dnt=1" style="height: 100%; left: 0; position: absolute; top: 0; width: 100%; margin: 0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script><p class="modal-text">Video by <a href="https://www.vuemastery.com" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Vue Mastery</a>. Watch Vue Mastery’s free <a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Intro to Vue course</a>.</div>
