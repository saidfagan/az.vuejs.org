---
title: Hesablanan xassə və izləyicilər
type: guide
order: 5
---

## Hesablanan xassələr

<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-computed-properties?friend=vuejs" target="_blank" rel="sponsored noopener" title="Learn how computed properties work with Vue School">Hesablanan xassələrin necə işlədiyini Vue School saytındakı pulsuz dərsdən öyrən</a></div>

Şablonların içindəki ifadələr olduqca rahatdır, lakin onlar sadə əməliyyatlar üçün nəzərdə tutulub. Şablonlara çoxlu məntiqin əlavə olunması onların dəstəklənməsini çətinləşdirir. Misal üçün:

``` html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

Burada şablon artıq sadə və aydın deyil. İlk baxışdan bu ifadənin `message` sahəsini əks ardıcıllıqda göstərdiyini anlamaq olmur. İfadəni şablonunuzda bir neçə dəfə istifadə etsəniz problem daha da kəskinləşəcək.

Ona görə də mürəkkəb ifadələr üçün **hesablanan xassələr** istifadə etməlisiniz.

### Sadə nümunə

``` html
<div id="example">
  <p>İlkin mesaj: "{{ message }}"</p>
  <p>Hesablanan çevrilmiş mesaj: "{{ reversedMessage }}"</p>
</div>
```

``` js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // hesablanan xassə
    reversedMessage: function () {
      // `this` vm surətinə yönəldir
      return this.message.split('').reverse().join('')
    }
  }
})
```

Nəticə:

{% raw %}
<div id="example" class="demo">
  <p>İlkin mesaj: "{{ message }}"</p>
  <p>Hesablanan çevrilmiş mesaj: "{{ reversedMessage }}"</p>
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

Burada `reversedMessage` hesablanan xassəsi elan olunub. Ötürülən funksiya `vm.reversedMessage` xassəsi üçün qetter kimi istifadə olunacaq:

``` js
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```

Konsolu açıb bu nümunə ilə təcrübə apara bilərsiniz. `vm.reversedMessage` xassəsinin dəyəri həmişə `vm.message` xassəsinin dəyərindən asılıdır.

Hesablanan xassələr adi xassələr kimi verilənlərə bağlana bilər. Vue `vm.reversedMessage` xassəsinin `vm.message`-dan asılı olduğunu bilir, ona görə `vm.message` xassəsi dəyişəndə `vm.reversedMessage` xassəsinə bağlı bütün elementlər də dəyişəcək. Ən vacib məqam burada odur ki, bu asılılığı biz deklarativ şəkildə yaratdıq: hesablanan xassənin qetterinin əks təsiri yoxdur, onu test etmək və anlamaq daha asandır.

### Hesablanan xassələrin keşlənməsi və metodlar

Fikir versəniz eyni nəticəni metod vasitəsi ilə də əldə etməyin mümkün olduğunu görərsiniz:

``` html
<p>Çevrilmiş mesaj: "{{ reverseMessage() }}"</p>
```

``` js
// in component
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

Hesablanan xassə yerinə həmin funksiyanı metod kimi təyin edə bilərik. Nəticə etibarı ilə bu iki yanaşma doğrudan da eynidir. Lakin fərq budur ki, **hesablanan xassələr reaktiv asılılıqlarından asılıolaraq keşlənir.** Hesablanan xassə ancaq onun reaktiv asılılıqları dəyişəndə yenidən hesablanır. Bu o deməkdir ki, `message` xassəsi dəyişməyibsə `reversedMessage` xassəsininin istifadəsi qetter funksiyasını çağırmayaraq sonuncu hesablanan dəyəri qaytaracaq.

Bu həm də o deməkdir ki, aşağıdakı hesablanan xassə heç vaxt yenilənməyəcək, çünki `Date.now()` reaktiv asılılıq deyil:

``` js
computed: {
  now: function () {
    return Date.now()
  }
}
```

Müqayisə üçün, metod çağırılması **hər dəfə səhifə render olduqda** funksiyanı icra edəcək.

Keşləmə nə üçün lazımdır? Təsəvvür edin ki massiv üzərindən keçən və çoxlu hesablama aparan "ağır" **A** hesablanan xassəsi var. Əlavə olaraq **A** xassəsindən asılı olan başqa hesablanan xassələr də mövcud ola bilər. Keşləmə olmasaydı **A** xassəsini qetteri lazım olandan xeyli çox çağırılardı! Keşləmədən yararlanmaq istəmirsinizsə metod istifadə edin.

### Computed vs Watched Property

Vue does provide a more generic way to observe and react to data changes on a Vue instance: **watch properties**. When you have some data that needs to change based on some other data, it is tempting to overuse `watch` - especially if you are coming from an AngularJS background. However, it is often a better idea to use a computed property rather than an imperative `watch` callback. Consider this example:

``` html
<div id="demo">{{ fullName }}</div>
```

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

The above code is imperative and repetitive. Compare it with a computed property version:

``` js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

Much better, isn't it?

### Computed Setter

Computed properties are by default getter-only, but you can also provide a setter when you need it:

``` js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

Now when you run `vm.fullName = 'John Doe'`, the setter will be invoked and `vm.firstName` and `vm.lastName` will be updated accordingly.

## Watchers

While computed properties are more appropriate in most cases, there are times when a custom watcher is necessary. That's why Vue provides a more generic way to react to data changes through the `watch` option. This is most useful when you want to perform asynchronous or expensive operations in response to changing data.

For example:

``` html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

``` html
<!-- Since there is already a rich ecosystem of ajax libraries    -->
<!-- and collections of general-purpose utility methods, Vue core -->
<!-- is able to remain small by not reinventing them. This also   -->
<!-- gives you the freedom to use what you're familiar with.      -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```

Result:

{% raw %}
<div id="watch-example" class="demo">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
{% endraw %}

In this case, using the `watch` option allows us to perform an asynchronous operation (accessing an API), limit how often we perform that operation, and set intermediary states until we get a final answer. None of that would be possible with a computed property.

In addition to the `watch` option, you can also use the imperative [vm.$watch API](../api/#vm-watch).
