---
title: Vue surəti
type: guide
order: 3
---

## Vue surətinin yaradılması

Hər bir Vue tətbiqi `Vue` funksiyası vasitəsi ilə **Vue surətinin** yaradılmasından başlayır:

```js
var vm = new Vue({
  // opsiyalar
})
```

Vue-nun dizaynı ona tamamilə bağlı olmasa da [MVVM şablonundan](https://en.wikipedia.org/wiki/Model_View_ViewModel) ilhamlanıb. Vue surəti üçün `vm` adlı dəyişən (ViewModel-dən qısaldılıb) ənənəvi olaraq istifadə olunur.

Vue surəti yaradılanda ona **opsiyalar obyekti** ötürülür. Bu bələdçinin böyük hissəsi həmin opsiyalardan istədiyiniz funksionalı almaq üçün necə istifadə etməyi təsvir edir. Əlavə məlumat üçün bütün opsiyaların siyahısına [API kitabçasında](../api/#Options-Data) baxa bilərsiniz.

Vue tətbiqi `new Vue` bəyanatı ilə yaranmış **təməl Vue surətindən** və ağac şəklində iç içə təkrar istifadə oluna bilən komponentlərdən ibarətdir. Məsələn, todo tətbiqin komponentlər ağacı belə qurula bilər:

```
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ TodoButtonDelete
   │  └─ TodoButtonEdit
   └─ TodoListFooter
      ├─ TodosButtonClear
      └─ TodoListStatistics
```

[Komponentlər sistemi](components.html) barədə bir az sonra daha ətraflı danışacayıq. Hələlik isə Vue komponentlərinin Vue surətləri olduğunu və eyni opsiyalar obyekti qəbul etdiyini (bəzi təməl Vue surətinə xas opsiyalardan başqa) bilməyiniz kifayətdir.

## Verilənlər və metodlar

Vue surəti yaradılanda, onun `data` obyektində olan bütün xassələri Vue-nun **reaktivlik sisteminə** əlavə olunur. Həmin xassələrin dəyərləri dəyişəndə görünüş yeni dəyərlərə uyğun yenilənəcək, başqa cür desək dəyişikliyə "reaksiya verəcək".

```js
// Bizim data obyektimiz
var data = { a: 1 }

// Bu obyekt Vue surətinə ötürülür 
var vm = new Vue({
  data: data
})

// Surətdən xassəni alanda
// ilkin data obyektindəki həmin xassə alınır
vm.a == data.a // => true

// Surətin xassəsini dəyişəndə
// ilkin data obyektinin həmin xassəsi də dəyişir
vm.a = 2
data.a // => 2

// ... əks əməliyyat da eyni nəticə verir
data.a = 3
vm.a // => 3
```

Xassələrin dəyəri dəyişəndə görünüş yenidən render olunacaq. Qeyd etmək lazımdır ki, `data` obyektindəki xassələr yalnız surət yaradıldığı zaman əlavə olunduqda **reaktiv** olurlar. Yəni əgər aşağıdakı şəkildə yeni xassə əlavə etsək:

```js
vm.b = 'hi'
```

`b` xassəsinə olan dəyişikliklər görünüşü dəyişməyəcək. Əgər hansısa xassənin daha sonra lazım olacağını bilirsinizsə, lakin onun dəyəri boşdursa, ona ilkin dəyər vermək lazımdır. Məsələn:

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Bu qaydaya tək istisna `Object.freeze()` metodunun istifadə olunması halıdır. Bu metod mövcud xassələrin dəyişməsinə icazə vermir, nəticədə reaktivlik sistemi dəyişiklikləri _izləyə_ bilmir.

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```

```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- `foo` dəyişəni artıq dəyişməyəcək! -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```

Data obyektindən başqa, Vue surətlərinin bir sıra lazımlı xassələri və metodları var. Onlar istifadəçinin təyin etdiyi xassələrdən seçilməsi üçün `$` işarəsi ilə başlayır. Məsələn:

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch surətin metodudur
vm.$watch('a', function (newValue, oldValue) {
  // Bu kolbək `vm.a` dəyişəndə çağırılacaq
})
```

Surətin bütün xassələri və metodlarının siyahısına baxmaq üçün [API kitabçasına](../api/#Instance-Properties) keçə bilərsiniz.

## Surətin həyat dövrü hukları

<div class="vueschool"><a href="https://vueschool.io/lessons/understanding-the-vuejs-lifecycle-hooks?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Lifecycle Hooks Lesson">Watch a free lesson on Vue School</a></div>

Hər bir Vue surıti yaradılarkən bir sıra inisizalizasiya mərhələlərindən keçir. Məsələn, verilənlər üzərində müşahidənin qurulması, çablonların kompilyasiyası, surətin DOM-a qoşulması, və verilənlər dəyişəndə DOM-un yenilənməsi. Bu mərhələlər arasında o, istifadəçilərə istədiyi zaman öz kodunu çağırmağa imkan verən **həyat dövrü hukları** funksiyalarını icra edir.

Məsələn [`created`](../api/#created) huku surət yaradılandan sonra kod çağırmaq üçün istifadə oluna bilər:

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` vm surətinə yönlənir
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

[`mounted`](../api/#mounted), [`updated`](../api/#updated), və [`destroyed`](../api/#destroyed) kimi surətin həyat dövrünün müxtəlif mərhələlərində çağırılan huklar da var. Bütün həyat dövrü huklarında `this` kontekst veriləni huku çağıran Vue surətinə yönlənir.

<p class="tip"> Surətin xassələrində və ya kolbəklərdə [ox funksiyalarından](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) aşağıda göstərildiyi kimi istifadə etməyin: `created: () => console.log(this.a)` və ya `vm.$watch('a', newValue => this.myMethod())`. Ox funksiyalarında `this` olmadığından, `this` digər verilənlər kimi rəftar olunacaq və tapılanadək daha yuxarıdakı görünüş dairələrində axtarılacaq. Nəticədə `Uncaught TypeError: Cannot read property of undefined` və ya `Uncaught TypeError: this.myMethod is not a function` kimi xətalara rast gələ bilərsiniz.</p>

## Həyat dövrü diaqramı

Aşağıda surətin həyat dövrləri üçün diaqramı görə bilərsiniz. Hal hazırda hər şeyi anlamağa məcbur deyilsiniz, zamanla öyrəndikcə bu şəkilə istinad edə bilərsiniz.

![The Vue Instance Lifecycle](/images/lifecycle.png)
