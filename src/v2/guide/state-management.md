---
title: Управление состоянием приложения
type: guide
order: 502
---

## Официальная Flux-подобная библиотека

Сложность больших приложений нередко возрастает из-за распределения кусочков состояния по многим компонентам и связям между ними. Для решения этой проблемы, Vue предлагает [Vuex](https://github.com/vuejs/vuex): нашу собственную библиотеку управления состоянием, вдохновлённую языком Elm. Она даже интегрируется с [Vue-devtools](https://github.com/vuejs/vue-devtools), из коробки давая доступ к функциональности ["машины времени"](https://raw.githubusercontent.com/vuejs/vue-devtools/master/media/demo.gif).

<div class="vue-mastery"><a href="https://www.vuemastery.com/courses/mastering-vuex/intro-to-vuex/" target="_blank" rel="noopener" title="Изучение Vuex">Посмотрите объясняющее видео на Vue Mastery</a></div>

### Информация для React-разработчиков

Если вы переходите на Vue с React, может быть интересно, как связаны Vuex и [Redux](https://github.com/reactjs/redux), являющийся наиболее популярной имплементацией Flux в React-экосистеме. Redux агностичен по отношению к слою представления, так что его можно напрямую использовать с Vue, применив [простые биндинги](https://yarnpkg.com/en/packages?q=redux%20vue&p=1). Vuex же _знает_, что работает с приложением Vue, что позволяет достичь лучшей интеграции, использовать более интуитивно-понятный API и в результате делает разработку приятнее.

## Простой контейнер состояния с нуля

Часто упускается из виду тот факт, что «источником истины» во Vue-приложениях является исходный объект `data` — экземпляры Vue всего лишь проксируют доступ к нему. Поэтому состояние, которым должны совместно владеть несколько экземпляров, можно просто передать по ссылке:

```js
const sourceOfTruth = {};

const vmA = new Vue({
  data: sourceOfTruth
});

const vmB = new Vue({
  data: sourceOfTruth
});
```

Теперь при любых изменениях `sourceOfTruth` обновится и `vmA`, и `vmB`. Подкомпоненты этих экземпляров также имеют доступ — используя `this.$root.$data`. Эффект «единого источника истины» достигнут, но отладка превратится в сущее мучение: любая часть данных может быть изменена любой частью приложения в любой момент и без каких-либо следов.

Для решения этой проблемы, мы можем использовать простое **хранилище**:

```js
var store = {
  debug: true,
  state: {
    message: "Привет!"
  },
  setMessageAction(newValue) {
    if (this.debug) console.log("setMessageAction вызвано с ", newValue);
    this.state.message = newValue;
  },
  clearMessageAction() {
    if (this.debug) console.log("clearMessageAction вызвано");
    this.state.message = "";
  }
};
```

Обратите внимание, что все действия, изменяющие состояние хранилища, сами помещены в него. Такой подход к глобальному управлению состоянием приложения облегчает понимание возможных изменений и источников их появления. Кроме того, если что-то пойдёт не так — у нас будет лог, по которому можно отследить последовательность действий, приводящую к возникновению бага.

Каждый экземпляр/компонент по-прежнему может иметь собственное, частное состояние:

```js
var vmA = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
});

var vmB = new Vue({
  data: {
    privateState: {},
    sharedState: store.state
  }
});
```

![Управление состоянием](/images/state.png)

<p class="tip">Важно заметить, что никогда не стоит заменять оригинальный объект состояния в действиях — компоненты и хранилище должны ссылаться на один и тот же объект, иначе отследить изменения будет невозможно.</p>

Если мы продолжим развивать концепцию, при которой компонентам запрещается прямое изменение состояния хранилища, а вместо этого предполагается обработка событий, указывающих хранилищу на необходимость выполнения тех или иных действий, мы можем прийти к архитектуре [Flux](https://facebook.github.io/flux/). Плюсом такого подхода является возможность записи всех происходящих с хранилищем изменений, что позволяет применять продвинутые техники отладки, такие как логи изменений, слепки данных и «машину времени».

Это вновь приводит нас к [Vuex](https://vuex.vuejs.org/ru/), так что если вы дочитали до этого места — пожалуй, пора его попробовать!
