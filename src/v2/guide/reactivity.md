---
title: Подробно о реактивности
type: guide
order: 601
---

Мы уже разобрали большую часть основ, так что пришло время нырнуть поглубже! Одна из наиболее примечательных возможностей Vue — это ненавязчивая реактивность. Модели представляют собой простые JavaScript-объекты. По мере их изменения обновляется и представление данных, благодаря чему управление состоянием приложения становится простым и очевидным. Тем не менее, у механизма реактивности есть ряд особенностей, знакомство с которыми позволит избежать распространённых ошибок. В этом разделе руководства мы подробно рассмотрим низкоуровневую реализацию системы реактивности Vue.

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/advanced-components/build-a-reactivity-system" target="_blank" rel="noopener" title="Реактивность Vue">Посмотрите объясняющее видео на Vue Mastery</a></div>

## Как отслеживаются изменения

Когда простой JavaScript-объект передаётся в экземпляр Vue в качестве опции `data`, Vue обходит все его поля и превращает их в пары [геттер/сеттер](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Working_with_Objects#Defining_getters_and_setters#%D0%9E%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_%D0%B3%D0%B5%D1%82%D1%82%D0%B5%D1%80%D0%BE%D0%B2_%D0%B8_%D1%81%D0%B5%D1%82%D1%82%D0%B5%D1%80%D0%BE%D0%B2), используя [`Object.defineProperty`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty). Эта возможность появилась в JavaScript только начиная с версии ES5, и в более ранних версиях её эмулировать не получится — по этой-то причине Vue и не поддерживает IE8 и ниже.

Геттеры и сеттеры не видны пользователю, но именно они являются тем внутренним механизмом, который позволяет Vue отслеживать зависимости и изменения данных. К сожалению, при таком подходе выведенные в консоль браузера геттеры и сеттеры выглядят не так, как обычные объекты, поэтому для более наглядной визуализации лучше использовать [инструменты разработчика Vue-devtools](https://github.com/vuejs/vue-devtools).

К каждому экземпляру компонента приставлен связанный с ним **экземпляр наблюдателя**, который помечает все поля, затронутые при отрисовке компонента, как зависимости. В дальнейшем, когда вызывается сеттер поля, помеченного как зависимость, этот сеттер уведомляет наблюдателя, который, в свою очередь, инициирует повторную отрисовку компонента.

![Цикл реактивности](/images/data.png)

## Особенности отслеживания изменений

В силу ограничений современного JavaScript (и отказа от `Object.observe`), Vue **не может отследить добавление или удаление свойства объекта**. Чтобы поле стало реактивным, Vue превращает его в пару геттер/сеттер в ходе инициализации экземпляра. Поэтому все поля должны изначально быть заданы в объекте `data`. Например:

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// теперь `vm.a` — реактивное поле

vm.b = 2
// `vm.b` НЕ реактивно
```

Во Vue нельзя динамически добавлять новые корневые реактивные свойства в уже существующий экземпляр. Тем не менее, можно добавить реактивное свойство во вложенные объекты, используя метод `Vue.set(object, propertyName, value)`:

```js
Vue.set(vm.someObject, 'b', 2)
```

Также можно использовать метод экземпляра `vm.$set`, который представляет собой псевдоним к глобальному `Vue.set`:

```js
this.$set(this.someObject, 'b', 2)
```

Иногда нужно добавить несколько свойств в существующий объект, например, с помощью `Object.assign()` или `_.extend()`. Если так поступить, добавленные свойства не станут реактивными. Для решения этой задачи придётся создать новый объект, содержащий поля как оригинального объекта, так и объекта-примеси:

```js
// вместо `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

Существует также несколько ограничений, связанных с массивами. Их мы рассмотрели ранее в [разделе об отображении списков](list.html#Предостережения).

## Объявление реактивных свойств

Поскольку Vue не позволяет динамически добавлять корневые реактивные свойства, это означает, что все корневые поля необходимо инициализировать изначально, хотя бы пустыми значениями:

```js
var vm = new Vue({
  data: {
    // объявляем поле message, содержащее пустую строку
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// впоследствии задаём значение `message`
vm.message = 'Привет!'
```

Если не задать поле `message` в опции data, Vue выведет предупреждение, что функция отрисовки пытается получить доступ к несуществующему свойству.

Существуют технические причины для этого ограничения: оно позволяет исключить целый класс граничных случаев в системе учёта зависимостей, а также упростить взаимодействие Vue с системами проверки типов. Но более важно то, что с этим ограничением становится проще поддерживать код, так как теперь объект `data` можно рассматривать как схему состояния компонента. Код, в котором реактивные свойства компонента перечислены заранее, намного проще для понимания.

## Асинхронная очередь обновлений

На всякий случай напомним, что во Vue обновление DOM выполняется **асинхронно**. Каждый раз, когда обнаруживается изменение в данных, создаётся очередь, которая используется в качестве буфера для этого и последующих изменений, происходящих в текущей итерации ("tick") цикла событий. Если один и тот же наблюдатель срабатывает несколько раз, в очередь он попадёт всё равно лишь единожды. Благодаря использованию буфера и устранению дубликатов, вычисления и манипуляции DOM сводятся к минимуму. В следующей итерации цикла событий Vue разбирает очередь и выполняет актуальные (уже не содержащие дубликатов) обновления. На низком уровне для асинхронной постановки задач в очередь используются `Promise.then`, `MutationObserver` и `setImmediate`, а если они недоступны, то `setTimeout(fn, 0)`.

Итак, если выполнить код `vm.someData = 'новое значение'`, компонент не будет отрисован сразу же. Он обновится в следующей итерации при разборе очереди. Чаще всего эту особенность можно не принимать в расчёт, но иногда бывает нужно дождаться состояния, в которое DOM перейдёт после обновления данных. Хотя прямая манипуляция DOM нежелательна, а системы в целом предпочтительнее проектировать так, чтобы в них были первичные данные, иногда всё же её не избежать. Чтобы выполнить какой-нибудь код только после того, как завершится обновление DOM, можно передать коллбэк в метод `Vue.nextTick(callback)` после изменения данных. Он будет вызван после обновления DOM. Например:

```html
<div id="example">{{ message }}</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'новое сообщение' // изменяем данные
vm.$el.textContent === 'новое сообщение' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'новое сообщение' // true
})
```

Существует также метод экземпляра `vm.$nextTick()`, особенно подходящий для использования внутри компонентов, поскольку он не требует обращения к глобальной переменной `Vue`, а также автоматически связывает контекст `this` коллбэка с текущим экземпляром Vue:

```js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: 'не обновлено'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = 'обновлено'
      console.log(this.$el.textContent) // => 'не обновлено'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => 'обновлено'
      })
    }
  }
})
```

Поскольку `$nextTick()` возвращает Promise, вы можете достичь того же, используя новый синтаксис [async/await из ES2016](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/async_function):

 ```js
  methods: {
    updateMessage: async function () {
      this.message = 'обновлено'
      console.log(this.$el.textContent) // => 'не обновлено'
      await this.$nextTick()
      console.log(this.$el.textContent) // => 'обновлено'
    }
  }
```
