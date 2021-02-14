---
title: Hesablanan və izləyici xassələr
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

Digər tərəfdən şablonda metodun istifadə olunması **hər dəfə səhifə render olduqda** çağırılacaq.

Keşləmə nə üçün lazımdır? Təsəvvür edin ki massiv üzərindən keçən və çoxlu hesablama aparan "ağır" **A** hesablanan xassəsi var. Əlavə olaraq **A** xassəsindən asılı olan başqa hesablanan xassələr də mövcud ola bilər. Keşləmə olmasaydı **A** xassəsini qetteri lazım olandan xeyli çox çağırılardı! Keşləmədən yararlanmaq istəmirsinizsə metod istifadə edin.

### Hesablanan və izləyici xassələr

Vue surətdəki verilənlərin dəyişməsini müşahidə edən və ona reaksiya verən ümumi üsul təqdim edir: **izləyici xassələr**. Bir verilən digər verilənin dəyəri dəyişəndə dəyişməlidirsə `watch` opsiyası istifadə etmək sizə cəlbedici gələ bilər, xüsusilə əvvəllər AngularJS istifadə edibsinizsə. Lakin çox vaxtı izləyici xassə yerinə hesablanan xassə istifadə etmək daha məqsədəuyğundur. Bu nümunəyə nəzər yetirin:

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

Yuxarıdakı kod imperativdir və təkrarlanır. Hesablanan xassə olan versiya ilə müqayisə edin:

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

Xeyli yaxşıdır, elə deyil mi?

### Hesablanan setter

Hesablanan xassələr defolt olaraq ancaq qetter ilə işləyir, amma lazım gələrsə setter də əlavə edə bilərsiniz:

``` js
// ...
computed: {
  fullName: {
    // qetter
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

Bu kodu `vm.fullName = 'John Doe'` icra etsəniz, setter çağırılacaq, `vm.firstName` və `vm.lastName` uyğun olaraq dəyişəcək.

## İzləyicilər

Hesablanan xassələr əksər hallarda daha uyğun olsalar da, izləyicilərin əvəzolunmaz olduğu hallar var. Buna görə Vue verilənlər dəyişməsinə reaksiya verməyin ümumi üsulunu `watch` opsiyası vasitəsi ilə təqdim edir. O, verilənlərin dəyişməsi nəticəsində icra edilən asinxron və ya ağır əməliyyatların yerinə yetirilməsi üçün faydalıdır.

Məsələn:

``` html
<div id="watch-example">
  <p>
    "Hə" və ya "yox" ilə cavablandırıla bilən sual soruşun:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

``` html
<!-- Ajax və ümumi məqsədli metodlar olan çoxlu kitabxana    -->
<!-- mövcud olduğundan Vue-nun nüvəsi belə kiçik qala bilir. -->
<!-- Bu həm də sizə tanış olduğunuz kitabxananı              -->
<!-- seçmək azadlığı verir                                   -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'Sual verənədək sizə cavab verə bilmərəm!'
  },
  watch: {
    // question xassəsi dəyişəndə bu funksiya çağırılacaq
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Yazıb qurtarmanızı gözləyirəm...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce lodash tərəfindən təqdim olunan ağır əməliyyatın 
    // çağırılma sayına məhdudiyyət qoyan funksiyadı.
    // Bu nümunədə istifadəçinin ajax sorğudan əvvəl sualı yazıb 
    // bitirməsini gozləyərkən yesno.wtf/api-nin çağırılma sayına 
    // məhdudiyyət qoymaq üçün istifadə edirik.
    // _.debounce funksiyası (və ona bənzər _.throttle) haqqında
    // əlavə məlumat almaq üçün bu linkə keçin: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Suallar adətən sual işarəsi ilə bitir. ;-)'
        return
      }
      this.answer = 'Fikirləşirəm...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Xəta! API əlçatan deyil. ' + error
        })
    }
  }
})
</script>
```

Nəticə:

{% raw %}
<div id="watch-example" class="demo">
  <p>
    "Hə" və ya "yox" ilə cavablandırıla bilən sual soruşun:
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
    answer: 'Sual verənədək sizə cavab verə bilmərəm!'
  },
  watch: {
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Yazıb qurtarmanızı gözləyirəm...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Suallar adətən sual işarəsi ilə bitir. ;-)'
        return
      }
      this.answer = 'Fikirləşirəm...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Xəta! API əlçatan deyil. ' + error
        })
    }
  }
})
</script>
{% endraw %}

Bu halda `watch` opsiyasının istifadə edilməsi asinxron əməliyyatın yerinə yetirilməsinə (API çağırılması), əməliyyat sayına məhdudiyyət qoyulmasına, və son cavab alınanadək aralıq vəziyyətin qoyulmasına imkan verir. Hesablanan xassə istifadə olunsa idi bunların heç birini etmək mümkün olmazdı.

`watch` opsiyasına əlavə olaraq, imperativ [vm.$watch API](../api/#vm-watch) istifadə edə bilərsiniz.
