---
title: Сравнение с другими фреймворками
type: guide
order: 801
---

Определённо, этот раздел руководства — самый трудный для написания, но он очень важен. Вероятно, вы уже решаете определённые задачи, используя тот или иной фреймворк или библиотеку, а сюда вас привело желание узнать, не позволит ли Vue упростить и улучшить вашу работу. На этот вопрос мы и надеемся ответить.

Мы очень постараемся не быть предвзятыми. Будучи членами основной команды разработки Vue, мы, разумеется, сами его очень любим. На наш взгляд, с некоторыми задачами Vue справляется лучше, чем какой-либо другой существующий фреймворк. Если бы мы не верили в это, мы бы наверное и не работали над этим проектом, верно? И тем не менее, нам бы хотелось быть предельно честными и точными в оценках. В тех случаях, когда альтернативные библиотеки имеют существенные преимущества, как например обширнейшая экосистема альтернативных средств отрисовки React'а или поддержка браузеров вплоть до IE6 Knockout'ом, мы постараемся не забыть о них упомянуть.

Кроме того, мы очень высоко ценим **вашу** помощь в деле поддержания актуальности этого документа, потому что мир JavaScript развивается стремительно! Если вы заметите какую-либо неточность или ошибку — пожалуйста, дайте нам знать, [открыв issue на GitHub](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).

## React

React и Vue во многом похожи. Они оба:

- используют Virtual DOM
- предоставляют реактивность и компонентную структуру
- фокусируются на корневой библиотеке, вынося прочие вопросы, такие как роутинг или управление глобальным состоянием приложения, в дополнительные библиотеки

Из-за столь похожих ниш, мы уделили сравнению этих фреймворков больше всего времени. Нашей целью было не только удостовериться в технической точности, но также и сохранить баланс. Мы указываем, где React превосходит Vue, например — в богатстве экосистемы и изобилии доступных пользовательских средств отрисовки.

Тем не менее, многие рассматриваемые моменты — в известной мере субъективны, и видится неизбежным то, что некоторым пользователям React дальнейшее сравнение всё равно покажется предвзятым. Мы понимаем, что и в технических вопросах существуют элементы вкуса, и в основном ставим своей целью в этом сравнении указать причины, по которым Vue может оказаться вам полезным — на случай если ваши вкусы совпадают с нашими.

Некоторые из приведённых ниже разделов могут быть немного устаревшими из-за недавних обновлений в React 16+, и мы планируем работать с сообществом React для актуализации этого раздела в ближайшем будущем.

### Быстродействие выполнения

Как React, так и Vue — исключительно быстрые, поэтому скорость вряд ли будет решающим фактором при выборе между ними. Если вас интересуют цифры, то можете изучить [стороннее сравнение производительности](https://stefankrause.net/js-frameworks-benchmark8/table.html), которое фокусируется на сырой производительности отрисовки/обновления дерева очень простых компонентов.

#### Усилия для оптимизации

В React когда состояние компонента изменяется, оно запускает повторную отрисовку всего поддерева компонента, начиная с себя. Чтобы избежать ненужной повторной отрисовки дочерних компонентов, вам нужно либо использовать `PureComponent`, либо реализовывать `shouldComponentUpdate` везде где это возможно. Вам также может потребоваться использовать неизменяемые (immutable) структуры данных, чтобы сделать изменения вашего состояния более удобными к оптимизации. Однако, в некоторых случаях вы не можете рассчитывать на такую оптимизацию, потому что `PureComponent/shouldComponentUpdate` предполагают, что отображение всего поддерева определяется данными текущего компонента. Если это не так, то такая оптимизация может привести к несогласованному состоянию DOM.

Во Vue зависимости компонента автоматически отслеживаются во время отрисовки, поэтому система точно знает, какие компоненты действительно необходимо повторно отрисовывать при изменении состояния. Каждый компонент можно рассматривать как имеющий `shouldComponentUpdate`, автоматически реализованный для вас, без ограничений для вложенных компонентов.

В целом, это устраняет необходимость целого класса оптимизаций производительности для разработчика, что позволяет больше сосредоточиться на построении самого приложения по мере его масштабирования.

### HTML & CSS

В React абсолютно всё — это JavaScript. Не только структуры HTML, выраженные через JSX, последние тенденции также включают управление CSS внутри JavaScript. Этот подход имеет свои преимущества, но также заставляет идти на компромиссы, которые могут показаться неэффективными для каждого разработчика.

Vue охватывает классические веб-технологии и основывается на них. Чтобы показать вам что это значит, мы рассмотрим несколько примеров.

#### JSX vs Шаблоны

В React все компоненты реализуют свой UI в render-функциях с использованием JSX, декларативным XML-подобным синтаксисом, который работает в JavaScript.

Render-функции с JSX имеют несколько преимуществ:

- Вы можете использовать все возможности языка программирования (JavaScript) для построения своего представления. Это включает в себя временные переменные, управление ветвлением и прямые ссылки на значения JavaScript в области видимости.

- Поддержка инструментов (например, линтинг, проверка типов, автодополнение в редакторе) для JSX в некоторых отношениях более продвинута, чем то, что доступно в настоящее время для шаблонов Vue.

Во Vue у нас также есть [`render`-функции](render-function.html) и даже [поддержка JSX](render-function.html#JSX), потому что иногда вам требуются эти возможности. Тем не менее, по умолчанию мы предлагаем шаблоны как более простую альтернативу. Любой валидный HTML также будет валидным шаблоном Vue, и это приводит к нескольким преимуществам:

- Для многих разработчиков, которые работают с HTML, шаблоны просто более естественны для чтения и написания. Само это предпочтение может быть несколько субъективным, но если это делает разработчика более продуктивным, то преимущество налицо.

- HTML-шаблоны облегчают постепенную миграцию существующих приложений для использования возможностей реактивности Vue.

- Это также облегчает дизайнерам и менее опытным разработчикам разбираться и вносить доработки в текущую кодовую базу.

- Вы можете даже использовать препроцессоры, такие как Pug (ранее известный как Jade), чтобы создавать ваши шаблоны во Vue.

Некоторые утверждают, что важно изучить дополнительный язык DSL (Domain-Specific Language) для написания шаблонов — мы считаем, что эта разница в лучшем случае поверхностна. Во-первых, JSX не означает, что пользователю не нужно ничего учить — это дополнительный синтаксис на основе простого JavaScript, поэтому он прост в освоении для любого, кто знаком с JavaScript, но говорить, что это просто для всех, было бы заблуждением. Точно также шаблон является просто дополнительным синтаксисом поверх простого HTML и, следовательно, имеет очень низкий порог вхождения для тех, кто уже знаком с HTML. С помощью DSL мы также можем помочь пользователю сделать больше с меньшим количеством кода (например, с `v-on` модификаторами). Похожая задача может включать в себя намного больше кода при использовании простых функций JSX или render-функций.

На более высоком уровне мы можем разделить компоненты на две категории: презентационные и логические. Мы рекомендуем использовать шаблоны для презентационных компонентов и `render`-функции / JSX для логических. Процентное соотношение этих компонентов зависит от типа вашего приложения, но обычно презентационные компоненты более распространены.

#### Модульный (компонентный) CSS

За исключением случаев разделения компонентов на несколько файлов (например, посредством [CSS-модулей](https://github.com/gajus/react-css-modules)), для ограничения области видимости CSS в React обычно используется подход CSS-in-JS (например, [styled-components](https://github.com/styled-components/styled-components), [glamorous](https://github.com/paypal/glamorous) и [emotion](https://github.com/emotion-js/emotion)). Это представляет собой новый компонентно-ориентированный подход к стилизации, который отличается от обычного процесса разработки CSS. Кроме того, несмотря на поддержку извлечения CSS в отдельный файл стилей на этапе сборки, по-прежнему может быть необходимо, чтобы во время выполнения были подключены в сборку для корректной работы стилизации. В то время, как вы получаете доступ к динамичности JavaScript при создании ваших стилей, этот компромисс зачастую увеличивает размер сборки и время исполнения.

Если вы поклонник подхода CSS-in-JS — многие популярные библиотеки поддерживают Vue (например, [styled-components-vue](https://github.com/styled-components/vue-styled-components) и [vue-emotion](https://github.com/egoist/vue-emotion)). Главным отличием между React и Vue здесь будет то, что по умолчанию стилизация Vue выполняется через знакомые теги `style` в [однофайловых компонентах](single-file-components.html).

[Однофайловые компоненты](single-file-components.html) предоставляют вам полный доступ к CSS в том же файле, что и остальная часть кода вашего компонента.

```html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

Опциональный атрибут `scoped` автоматически ограничивает область видимости CSS текущим компонентом, добавляя элементам уникальные атрибуты (такие как `data-v-1-21e5b78`), и компилируя `.list-container:hover` во что-нибудь вроде `.list-container[data-v-1-21e5b78]:hover`.

Наконец, стилизация в однофайловых компонентах Vue очень гибка. С помощью [vue-loader](https://github.com/vuejs/vue-loader), вы можете использовать любой препроцессор, постпроцессор и даже глубокую интеграцию с [CSS-модулями](https://vue-loader.vuejs.org/ru/features/css-modules.html) — всё в элементе `<style>`.

### Масштабирование

#### Масштабирование вверх

Для крупных приложений, как Vue так и React предоставляют надёжные решения для роутинга. Сообщество React также породило весьма инновационные решения в области управления состоянием приложения (см. Flux/Redux). Эти подходы, и [даже сам Redux](https://yarnpkg.com/en/packages?q=redux%20vue&p=1) легко интегрируются в приложения на Vue. В действительности, Vue сделал следующий шаг, создав [Vuex](https://github.com/vuejs/vuex) — вдохновлённую Elm реализацию паттерна управления состоянием приложения. Vuex глубоко интегрирован с Vue, что, на наш взгляд, изрядно облегчает жизнь разработчикам.

В качестве ещё одного важного различия между React и Vue можно упомянуть тот факт, что все дополнительные библиотеки Vue, включая библиотеки для управления состоянием приложения и для роутинга (среди [прочих задач](https://github.com/vuejs)), официально поддерживаются в актуальном соответствии с ядром библиотеки. React, напротив, предпочитает отдать эти вопросы на откуп сообществу, тем самым создавая более фрагментированную экосистему. Впрочем, как уже замечалось ранее, в силу популярности React, его экосистема значительно обширнее, чем у Vue.

Наконец, у Vue есть [CLI генератор проектов](https://github.com/vuejs/vue-cli), который позволяет легко начать новый проект с помощью интерактивного мастера. Его также можно использовать для [мгновенного прототипирования](https://cli.vuejs.org/ru/guide/prototyping.html#%D0%BC%D0%B3%D0%BD%D0%BE%D0%B2%D0%B5%D0%BD%D0%BD%D0%BE%D0%B5-%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D1%82%D0%B8%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5) компонента. React также делает успехи в этой области с помощью [create-react-app](https://github.com/facebookincubator/create-react-app), но в настоящее время у него есть несколько ограничений:

- Он не допускает никакой конфигурации во время создания проекта, в то время как Vue CLI работает поверх обновляемой runtime-зависимости, которая легко расширяется с помощью [плагинов](https://cli.vuejs.org/ru/guide/plugins-and-presets.html#%D0%BFn%D0%B0%D0%B3%D0%B8%D0%BD%D1%8B).
- Существует только один шаблон для одностраничного приложения, в то время как Vue предлагает широкий выбор опций по умолчанию для различных целей и систем сборки.
- Нет возможности создавать проекты из пользовательских [пресетов настроек](https://cli.vuejs.org/ru/guide/plugins-and-presets.html#%D0%BF%D1%80%D0%B5%D1%81%D0%B5%D1%82%D1%8B-%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BA), что может быть особенно полезно для enterprise-окружений с установившимися ранее соглашениями.

Важно заметить, что многие из этих ограничений — следствия сознательно принятых решений команды create-react-app, и в них есть и свои плюсы. Например, коль скоро ваш проект не требует пользовательской настройки процесса сборки, её можно будет обновлять как зависимость. Прочитать больше об этом подходе [можно здесь](https://github.com/facebookincubator/create-react-app#philosophy).

#### Масштабирование вниз

React известен своей довольно крутой кривой изучения. До того момента, когда новичок сможет что-то написать, ему придётся узнать о JSX, а вероятно — и о ES2015+, поскольку многие примеры используют синтаксис ES2015-классов. Кроме того придётся разобраться с системами сборки, поскольку, хотя технически и существует возможность использовать Babel самостоятельно для live-компиляции кода, для production этот подход в любом случае не годится.

Vue масштабируется вверх ничуть не хуже, чем React, и в то же время его можно масштабировать и вниз — вплоть до варианта использования вместе с jQuery. Именно так — для старта в простейшем случае достаточно просто добавить тег скрипта на HTML-страницу.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Начиная с этого момента можно писать код на Vue, и даже использовать production-версию, не мучаясь угрызениями совести и волнениями насчёт производительности.

Поскольку знания JSX, ES2015 и систем сборки не требуется для начала работы с Vue, в среднем у новых разработчиков уходит не больше дня на чтение [руководства](./), позволяющего узнать достаточно для построения нетривиальных приложений.

### Нативная отрисовка

React Native позволяет писать нативные приложения для iOS и Android, используя ту же самую модель компонентов React'а. Это — прекрасно, так как позволяет разработчикам применить знание одного и того же фреймворка на различных платформах. В этой области, Vue официально поддерживает проект [Weex](https://weex.apache.org/) — кроссплатформенный UI-фреймворк, созданный Alibaba Group и инкубированный Apache Software Foundation (ASF). Weex позволяет использовать тот же синтаксис Vue для создания компонентов, которые не только могут отображаться в браузере, но и также нативными элементами в iOS и Android!

На данный момент Weex всё ещё находится в активной фазе разработки, и ещё не столь матёр и проверен опытом, как React Native. Однако, его разработка мотивируется реальными требованиями крупнейшего бизнеса электронной коммерции в мире. Команда разработки Vue также активно взаимодействует с разработчиками Weex, гарантируя отсутствие неожиданностей для Vue-разработчиков.

Ещё один вариант [NativeScript-Vue](https://nativescript-vue.org/) — плагин для [NativeScript](https://www.nativescript.org/) для создания по-настоящему нативных приложений с помощью Vue.js.

### Сравнение с MobX

MobX стал довольно популярным в сообществе React. Он использует почти идентичную Vue систему реактивности. В некотором смысле, связку React + MobX можно считать несколько более многословным вариантом Vue, так что если вы используете её, и она вам нравится, переход на Vue может оказаться следующим логичным шагом.

### Preact и другие React-подобные библиотеки

React-подобные библиотеки обычно пытаются использовать как можно больше своих API и экосистемы React насколько это осуществимо. По этой причине большинство сравнений, приведённых выше, также применимы и к ним. Главным отличием, как правило, будет уменьшение доступной экосистемы (часто значительно) в сравнении с React. Поскольку эти библиотеки не могут быть на 100% совместимы со всем в экосистеме React, некоторые библиотеки инструментов или сопутствующие библиотеки могут не использоваться. Или, даже если похоже что они работают, они могут сломаться в любое время, если ваша конкретная React-подобная библиотека не поддерживается наравне с React.

## AngularJS (Angular 1)

Некоторые части синтаксиса Vue выглядят очень похоже на синтаксис AngularJS (например, сравните `v-if` и `ng-if`). Это — не случайность: многие идеи, лежащие в основе AngularJS мы считаем верными, и вдохновлялись ими на ранних этапах разработки Vue. Впрочем, в AngularJS немало и болезненных проблем, и в этих областях мы постарались добиться значительных улучшений.

### Сложность

Vue значительно проще AngularJS, как в смысле API, так и в смысле архитектуры. Получение достаточных знаний для написания нетривиальных приложений обычно происходит менее чем за день, чего нельзя сказать об AngularJS.

### Гибкость и модульность

AngularJS имеет жёсткое мнение насчёт структуры вашего приложения, в то время как Vue проявляет гибкость и является более модульным решением. Хотя это и делает Vue пригодным для большего разнообразия проектов, мы понимаем и то, что когда решения уже приняты за тебя, можно сразу начать программировать, и в этом есть свои преимущества.

Поэтому мы предоставляем полную систему для быстрой разработки на Vue.js. [Vue CLI](https://github.com/vuejs/vue-cli) нацелен стать стандартным базовым инструментом для экосистемы Vue. Это гарантирует, что различные инструменты сборки будут работать вместе с оптимальными настройками по умолчанию, что позволит сосредоточиться на создании приложения, а не тратить часы на конфигурирование. В тоже время, он по-прежнему предоставляет гибкую настройку конфигурации каждого инструмента в соответствии с конкретными потребностями.

### Связывание данных

AngularJS использует двунаправленное связывание данных между областями видимости, в то время как Vue концентрируется на однонаправленном потоке данных между компонентами. Это позволяет облегчить понимание потока данных в нетривиальных приложениях.

### Директивы и компоненты

Vue чётче разделяет директивы и компоненты. Директивы предназначены только для инкапсуляции низкоуровневых манипуляций с DOM, в то время как компоненты являют собой полноценные автономные объекты, со своей собственной логикой данных и представления. В AngularJS директивы делают всю работу, а компоненты всего лишь определённый тип директив.

### Быстродействие выполнения

Vue производительнее AngularJS. Кроме того, из-за отсутствия dirty-checking, оптимизировать Vue-приложения намного-намного проще. AngularJS замедляется при увеличении количества наблюдателей, поскольку каждый раз при изменении чего-либо в области видимости все эти наблюдатели должны быть перезапущены. Кроме того, цикл может повториться несколько раз перед стабилизацией, поскольку реакция наблюдателей может спровоцировать следующее обновление. Пользователям AngularJS нередко приходится прибегать к весьма эзотерическим техникам для обхода этих трудностей, а в некоторых случаях оптимизация и вовсе становится невозможной.

Vue не подвержен таким проблемам по причине использования прозрачного механизма учёта зависимостей с асинхронной очередью — все изменения рассматриваются независимо, кроме случаев явного указания наличия связи между ними.

Любопытно, что есть немало общих черт, как Angular и Vue решают эти проблемы AngularJS.

## Angular (известный также как Angular 2)

Мы выделяем отдельную секцию для нового Angular, поскольку по сути он полностью отличен от AngularJS. Например, теперь он содержит полноценную компонентную систему; кроме того, многие детали реализации были полностью переписаны, а API очень существенно изменился.

### TypeScript

Для Angular требуется использовать TypeScript, поскольку почти вся его документация и учебные ресурсы основаны на TypeScript. TypeScript имеет свои очевидные преимущества — проверка статических типов может быть очень полезна для крупных приложений, и может добавить производительности разработчикам, работающим на Java и C#.

Однако не все хотят использовать TypeScript. Часто для небольших приложений введение системы типов может привести к большему увеличению накладных расходов нежели увеличению производительности разработки. В таких случаях вам лучше воспользоваться Vue, так как использовать Angular без TypeScript может быть сложным.

Наконец, хотя Vue и не так глубоко интегрирован с TypeScript как Angular, но предоставляет [официальные декларации типов](https://github.com/vuejs/vue/tree/dev/types) и [официальный декоратор](https://github.com/vuejs/vue-class-component) для тех, кто хочет использовать TypeScript с Vue. Мы также активно сотрудничаем с командами TypeScript и VSCode в Microsoft, чтобы улучшить опыт TS/IDE для пользователей Vue + TS.

### Быстродействие выполнения

В смысле производительности, оба фреймворка весьма быстры, и пока нет достаточных данных из реального мира чтобы вынести окончательный вердикт. Но если вы всё же хотите цифр, похоже что Vue 2.0 всё-таки обгоняет Angular, по крайней мере, если верить этому [стороннему исследованию производительности](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html).

Оба фреймворка исключительно быстрые, с очень похожими метриками в тестах. Вы можете [изучить конкретные цифры](https://stefankrause.net/js-frameworks-benchmark8/table.html) для более детального сравнения, но скорость вряд ли станет решающим фактором.

### Размер

Последние версии Angular, с [AOT-компиляцией](https://ru.wikipedia.org/wiki/AOT-компиляция) и [tree-shaking](https://en.wikipedia.org/wiki/Tree_shaking), смогли значительно уменьшить размер сборок. Однако полнофункциональный проект Vue 2 с включёнными Vuex + Vue Router (~30КБ gzip) по-прежнему значительно легче из коробки, чем AOT-скомпилированное приложение, созданное с помощью `angular-cli` (~65КБ gzip).

### Гибкость

Vue значительно менее упрям, чем Angular, и поддерживает множество различных систем сборок, не ограничивая разработчиков в том, какую структуру использовать для приложения. Многим программистам эта свобода нравится, хотя есть и те, кто предпочитают иметь единственно-правильный способ построения приложения.

### Кривая обучения

Всё что необходимо для начала работы с Vue — это знакомство с HTML и обыкновенным (ES5) JavaScript'ом. С этими базовыми навыками вы уже можете начать строить нетривиальные приложения после менее чем однодневного прочтения [руководства](./).

Кривая обучения у Angular гораздо круче. API фреймворка просто огромное и вам как пользователю нужно будет разобраться с большим количеством концепций, прежде чем стать продуктивным. Очевидно, что сложность Angular во многом обусловлена его направленностью только на большие, комплексные приложения — но это делает платформу намного более трудной для менее опытных разработчиков.

## Ember

Ember — это полнофункциональный фреймворк, изначально созданный «имеющим и отстаивающим своё мнение». Он требует соблюдения множества соглашений и конвенций, и когда вы с ними освоитесь — он может сделать вас весьма продуктивными. Однако, это означает ещё и высокую и крутую кривую обучения, не говоря о страдающей гибкости. Выбор между жёстко структурированным фреймворком и библиотекой со слабосвязанными совместно работающими инструментами — это всегда компромисс. Последняя даёт больше свободы, но и требует принятия большего количества самостоятельных архитектурных решений.

Учитывая вышесказанное, вероятно будет разумнее сравнивать ядро Vue и слои [шаблонизации](https://guides.emberjs.com/v2.7.0/templates/handlebars-basics/) и [объектной модели](https://guides.emberjs.com/v2.7.0/object-model/) Ember:

- Vue предлагает ненавязчивую реактивность для простых JavaScript-объектов и полностью автоматические вычисляемые свойства. В Ember от вас ожидается заворачивание всего в «Объекты Ember» и ручное указание зависимостей для вычисляемых свойств.

- Синтаксис шаблонов Vue позволяет использовать все возможности выражений JavaScript, в то время как возможности выражений Handlebars и синтаксис хелперов в Ember намеренно существенно ограничены.

- В вопросах производительности Vue [существенно выигрывает](https://stefankrause.net/js-frameworks-benchmark8/table.html) — даже с учётом последнего обновления Glimmer engine в Ember 3.x. Vue автоматически объединяет операции обновления, в то время как в Ember требуется ручное управление циклом выполнения в ситуациях, где производительность критична.

## Knockout

Knockout был пионером MVVM-подхода и отслеживания изменений в данных. Его система реактивности очень похожа на используемую Vue. Список [поддерживаемых браузеров](http://knockoutjs.com/documentation/browser-support.html) — впечатляет, особенно с учётом всех немалых возможностей фреймворка, доступных даже в IE6! Vue же поддерживает только IE9+.

Со временем, впрочем, разработка Knockout замедлилась и он понемногу начал показывать признаки своего возраста. Например, компонентной системе недостаёт полного набора хуков жизненного цикла, а интерфейс передачи дочерних компонентов, хоть и используется очень широко, выглядит по сравнению со [слотами Vue](components.html#Распределение-контента-слотами) не очень выигрышно.

Кроме того, похоже что существует разница в философских подходах к API, которая, если вам интересно, может быть продемонстрирована различиями при создании [простого списка todo](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). Конечно же, это — субъективный вопрос, но многим API Vue кажется проще и лучше структурированным.

## Polymer

Polymer — это проект, спонсируемый Google. В действительности, он тоже послужил источником вдохновения для Vue. Компоненты Vue можно приблизительно сравнивать с пользовательскими элементами Polymer. Стиль разработки с использованием обоих фреймворков довольно похож. Самая существенная разница состоит в том, что Polymer базируется на последних возможностях Web Components и требует для работы использования весьма нетривиальных полифилов (с потерей быстродействия в браузерах без нативной поддержки этих возможностей). Vue, напротив, без каких-либо зависимостей или полифилов работает во всех браузерах, начиная с IE9.

В Polymer, разработчики существенно ограничили систему связывания данных для улучшения производительности. Например, единственными поддерживаемыми в шаблонах Polymer выражениями являются булево отрицание и одиночные вызовы методов. Реализация вычисляемых свойств — тоже не очень гибкая.

## Riot

Riot 3.0 предлагает похожую модель разработки, основывающуюся на компонентах (в Riot называемых «тегами»), с минималистичным и прекрасно организованным API. Вероятно, Riot и Vue во многом основаны на схожих философских подходах. Однако, несмотря на немного больший размер по сравнению с Riot, Vue имеет некоторые существенные преимущества:

- Лучшая производительность. [Несмотря на заявленное](https://github.com/vuejs/vuejs.org/issues/346) использование Virtual DOM, в действительности Riot применяет dirty checking, и потому страдает от тех же проблем, что и AngularJS.
- Поддержка более зрелого инструментария. Vue официально поддерживает [Webpack](https://github.com/vuejs/vue-loader), [Browserify](https://github.com/vuejs/vueify) и [SystemJS](https://github.com/vuejs/systemjs-plugin-vue), в то время как Riot полагается в вопросах интеграции с системами сборки на поддержку сообщества.
