---
title: Giriş
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

## İstifadəçi inputunun emal olunması

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/czPNaUr" target="_blank" rel="noopener noreferrer">Bu dərsə Scrimba saytında bax</a></div>

İstifadəçilərin tətbiqimizə təsir göstərə bilməsi üçün `v-on` direktivindən istifadə edə bilərik. Bu direkltiv Vue obyektlərinə hadisə dinləyiciləri bərkidərək metodları çağıra bilmək imkanı yaradır:

``` html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">İsmarıcı çevir</button>
</div>
```
``` js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Salam Vue.js!'
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
  <button v-on:click="reverseMessage">İsmarıcı çevir</button>
</div>
<script>
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Salam Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Fikir verin ki, bu metodda biz tətbiqimizin halını DOM-a toxunmadan dəyişirik - DOM-la olan bütün işi Vue görür, və siz yalnız tətbiqin məntiqinə aid kod yazırsınız.

Vue həmçinin `v-model` direktivini təmin edir. Bu direktiv formanın elementləri və tətbiqin halını bir birinə ikitərəfli bağlayır:

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
    message: 'Salam Vue!'
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
    message: 'Salam Vue!'
  }
})
</script>
{% endraw %}

## Komponentlərin yığılması

<div class="scrimba"><a href="https://scrimba.com/p/pXKqta/cEQVkA3" target="_blank" rel="noopener noreferrer">Bu dərsə Scrimba saytında bax</a></div>

Komponent sistemi Vue-nun önəmli konseptlərindən biridir. Bu abstraksiya bizə kiçik, təkrar istifadə oluna bilən hissələrdən iri miqyaslı tətbiq qurmağa imkan verir. Hər bir tətbiqin interfeysi komponent ağacı şəklində göstərilə bilər:

![Component Tree](/images/components.png)

Komponent mahiyyətcə əvvəlcədən təyin edilmiş opsiyaları olan Vue obyektidir. Yeni komponentin yaradılması sadədir:

``` js
// todo-item adında yeni komponent təyin edir
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})

var app = new Vue(...)
```

Daha sonra bu komponent başqa komponentin şablonunda istifadə oluna bilər:

``` html
<ol>
  <!-- todo-item komponentinin nüsxəsini yaradır -->
  <todo-item></todo-item>
</ol>
```

Amma bu halda bütün `todo-item` komponentlərində eyni mətn olacaq. Komponentə mətn ötürə bilmək imkanı olması vacibdir. Buna nail olmaq üçün komponentə [prop](components.html#Props) ötürmək olar:

``` js
Vue.component('todo-item', {
  // todo-item komponenti "prop"
  // parametri qəbul edir.
  // Bu prop-un adı todo-dur.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```

İndi isə `v-bind` istifadə edərək hər komponentə mətn ötürə bilərik:

``` html
<div id="app-7">
  <ol>
    <!--
      İndi biz istənilən todo-item komponentinə uyğun mətn
      ötürə bilərik. Nəticədə onun içindəki mətn dinamik ola bilir.
      Bundan başqa hər bir komponentə "key" atributu ötürülməlidir.
      Bu barədə daha sonra danışılacaq.
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
      { id: 0, text: 'Tərəvəz' },
      { id: 1, text: 'Pendir' },
      { id: 2, text: 'İnsanlar yediyi başqa şeylər' }
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
      { id: 0, text: 'Tərəvəz' },
      { id: 1, text: 'Pendir' },
      { id: 2, text: 'İnsanlar yediyi başqa şeylər' }
    ]
  }
})
</script>
{% endraw %}

Trivial nümunə olmasına baxmayaraq, tətbiqimizi iki hissəyə böldük. Alt komponent üst komponentdən ayrılıb. Nəticədə `<todo-item>` komponenti üst komponentə təsir etmədən daha mürəkkəb şablonla və məntiqlə təkmilləşdirilə bilər.

İri tətbiqlərdə development prosesini idarə etməyi asanlaşdırmaq üçün tətbiqi komponentlərə bölmək lazımdır. Komponentlər haqqında [daha sonra](components.html) daha ətraflı danışacıq. İndi isə komponentlər istifadə edən tətbiq nümunəsinə nəzər yetirin:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

### İstifadəçi yaradan elementlər ilə əlaqəsi

Fikir verdinizsə Vue komponentləri [W3C Web Components spesifikasiyanın](https://www.w3.org/wiki/WebComponents/) hissəsi olan **İstifadəçi yaradan elementlərə (Custom Elements)** bənzəyir. Bunun səbəbi Vue komponentlər sisteminin bu spesifikasiya üzərində qurulmasıdır. Məsələn, Vue komponentləri [Slot API-sini](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) və xüsusi `is` atributunu reallaşdırır. Lakin bir neçə önəmli fərq var:

1. Web Components spesifikasiyası yekunlaşdırılsa da, bütün brauzerlərdə dəstəklənmir. Safari 10.1+, Chrome 54+ və Firefox 63+ brauzerləri veb komponentləri dəstəkləyir. Müqayisə üçün Vue komponentləri heç bir polifil tələb etmir və bütün brauzerlərdə (IE9 və daha yeni versiyalarda) davamlı işləyir. Ehtiyac yaranarsa Vue komponentləri istifadəçi yaradan elementlər daxilində də istifadə oluna bilir.

2. Vue komponentləri istifadəçi yaradan elementlərdə olmayan önəmli imkanlar təqdim edir: komponentlər arası məlumat ötürülməsi, istifadəçi yaradan hadisələr vasitəsi ilə kommunikasiya və yığma aləti inteqrasiyası.

Daxilində istifadəçi yaradan elementlər istifadə etməsə də, Vue komponentləri onlar kimi [istifadə oluna bilər](https://custom-elements-everywhere.com/#vue). Həmçinin Vue CLI özünü istifadəçi yaradan element kimi təqdim edən Vue komponentlərin yığılmasını dəstəkləyir.

## Daha çoxuna hazırsınız?

Bu modulda Vue.js-in əsas imkanlarına qısaca nəzər yetirdik. Bələdçinin qalan hissəsi bu və daha mürəkkəb imkanları daha ətraflı əhatə edəcək. Sona qədər təqib edin!

<div id="video-modal" class="modal"><div class="video-space" style="padding: 56.25% 0 0 0; position: relative;"><iframe src="https://player.vimeo.com/video/247494684?dnt=1" style="height: 100%; left: 0; position: absolute; top: 0; width: 100%; margin: 0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe></div><script src="https://player.vimeo.com/api/player.js"></script><p class="modal-text">Video by <a href="https://www.vuemastery.com" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Vue Mastery</a>. Watch Vue Mastery’s free <a href="https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/" target="_blank" rel="sponsored noopener" title="Vue.js Courses on Vue Mastery">Intro to Vue course</a>.</div>
