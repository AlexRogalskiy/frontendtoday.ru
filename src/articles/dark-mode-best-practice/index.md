---
title: 'Темный режим и как его лучше делать'
date: 2020-10-15
author:
  name: Usman Yunusov
  url: https://twitter.com/usmanyunusov
layout: article.njk
tags:
  - article
  - css
  - js
preview: 'Темный режим сегодня является одним из самых главных тенденций в современном дизайне пользовательского интерфейса. Операционные системы, браузеры и другие продукты, которыми мы пользуемся сегодня, уже во всю используют эту фичу.'
hero:
  src: images/1.png
  alt: 'Dark Mode cover'
---

Вы, наверное, уже заметили, что в последнее время все начали предлагать своим пользователям темный режим (Dark Mode), в котором в основном используются темные цвета, вместо традиционных белых и ярких тонов. Темный режим сегодня является одним из самых главных тенденций в современном дизайне пользовательского интерфейса. Операционные системы, браузеры и другие продукты, которыми мы пользуемся сегодня, уже во всю используют эту фичу.

Темный режим (он же “ночной режим”) не является новшеством, так как изначально первые компьютеры использовали монохромные ЭЛТ-мониторы, которые отображали зеленоватый текст на черном экране. Многие первые [текстовые процессоры](https://ru.wikipedia.org/wiki/%D0%A2%D0%B5%D0%BA%D1%81%D1%82%D0%BE%D0%B2%D1%8B%D0%B9_%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%81%D0%BE%D1%80) также позволяли печатать на черном фоне белым текстом. С появлением сложных [настольных издательских систем](https://ru.wikipedia.org/wiki/%D0%9D%D0%B0%D1%81%D1%82%D0%BE%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D0%B8%D0%B7%D0%B4%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D1%81%D0%BA%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0) с технологией [WYSIWYG](https://ru.wikipedia.org/wiki/WYSIWYG.) начался тренд светлого режима. Сделать виртуальный документ похожим на лист бумаги стал популярным. Браузеры так же приняли эту тенденцию, прописав в таблице стилей [агента пользователя](https://developer.mozilla.org/ru/docs/%D0%A1%D0%BB%D0%BE%D0%B2%D0%B0%D1%80%D1%8C/User_agent) дефолтные базовые цвета ([Chrome](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css), [Firefox](https://dxr.mozilla.org/mozilla-central/source/layout/style/res/html.css), [Safari](https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css)). Вот почему когда открываем HTML-страницу, мы видим черный текст на белом фоне.

Так почему же темный режим вновь обрел популярность сегодня? Возможно, это просто очередной тренд, который предлагает ряд преимуществ:

- Уменьшение нагрузки на глаза;
- Увеличивание видимости при слабом освещение;
- Экономия заряда батареи;
- Добавление акцента;

А как на счет веба? Он так же начал потихоньку переходит на темную сторону. У большенство сайтов появилась кнопка переключения ночного и дневного режима. Для реализации такого функционала обычно используются:

- Отдельные классы;
- Отдельные таблицы стилей;
- Пользовательские свойства;
- Серверные скрипты;

В зависимости от требований вашего проекта, вы можете использовать конкретные вышеперечисленные способы, так же их совмещать между собой. На примерах мы будем использовать [пользовательские свойства](https://www.w3.org/TR/css-variables-1/). В начале объявляем основные глобальные цвета и переопределяем их в определенном селекторе.

```css
:root {
  --color-text: #000;
  --color-back: #fff;
}

:root[data-theme='dark'] {
  --color-text: #fff;
  --color-back: #000;
}

body {
  background-color: var(--color-back);
  color: var(--color-text);
}
```

Остается только написать небольшой js-скрипт, который будет отвечать за изменения значения data-атрибута (в данном случае `data-theme`).

```js
const button = document.querySelector('.button')

button.addEventListener('click', function () {
  let theme = document.documentElement.getAttribute('data-theme')
  document.documentElement.setAttribute(
    'data-theme',
    theme === 'dark' ? 'light' : 'dark',
  )
})
```

Однако, при каждом обновление страницы, надо будет заново переключаться в нужный режим. Уместно было бы сохранять настройки в браузере, чтобы при следующем посещение нашего сайта показать пользователю тот режим, который он предпочел. Для этого будем использовать [LocalStorage](https://developer.mozilla.org/ru/docs/Web/API/Window/localStorage). Также для этого хорошо подходит [сookies](https://ru.wikipedia.org/wiki/Cookie).

```js
const button = document.querySelector('.button')
const currentTheme = localStorage.getItem('theme') || 'light'

button.addEventListener('click', function () {
  const theme = localStorage.getItem('theme')
  document.documentElement.setAttribute(
    'data-theme',
    theme === 'light' ? 'dark' : 'light',
  )
  localStorage.setItem('theme', theme === 'light' ? 'dark' : 'light')
})

document.documentElement.setAttribute('data-theme', currentTheme)
```

Теперь все работает как надо. Настройки сохраняются – пользователи радуются.

<iframe src="https://codepen.io/usmanyunusov/embed/preview/Exyagjv" title="Пример работы на CodePen."></iframe>

Но как быть, если пользователь предпочел темный режим в своем устройстве и ожидает увидеть в темном браузере темную страницу (что-то много стало темного 🙂 )? Многие упускают этот момент. Некоторые предпочитают, чтобы сайт отображался только в одном выбранном режиме, в то время как другие хотят чтобы он реагировал также на изменение темы их операционной системы. Как это сделать?

Не задолго до выпуска macOS Mojave, в 2018 году W3C добавила [черновой вариант спецификации](https://github.com/w3c/csswg-drafts/commit/bc456b739e20ad55b4cfa2684277fc646c4a0afc#diff-991dd8d5d2f1acaf819b5c26d8b3f99eR660) “Media Queries Level 5”, куда был включен новый медиа-запрос [prefers-color-scheme](https://www.w3.org/TR/mediaqueries-5/#prefers-color-scheme). Он используется для определения того, светлую или темную тему использует пользователь в своем устройстве. На сегодняшний день данная функция поддерживается [большинством браузеров](https://caniuse.com/?search=prefers-color-scheme).

Давайте воспользуемся новой фичей и немного поменяем наш функционал. До этого наш переключатель темы имел два состояния, то сейчас нам нужно три (dark, light, device). Добавим новое значение `data-theme="device"`.

```css
:root {
  --color-text: #000;
  --color-back: #fff;
}

:root[data-theme='dark'] {
  --color-text: #fff;
  --color-back: #000;
}

@media (prefers-color-scheme: dark) {
  :root[data-theme='device'] {
    --color-text: #fff;
    --color-back: #000;
  }
}

body {
  background-color: var(--color-back);
  color: var(--color-text);
}
```

Изменим js-скрипт

```js
const inputs = document.querySelectorAll('input[name="theme"]')
const currentTheme = localStorage.getItem('theme') || 'device'
const input = document.querySelector(`input[id="${currentTheme}"]`)

inputs.forEach((input) => {
  input.addEventListener('change', (e) => {
    document.documentElement.setAttribute('data-theme', e.target.id)
    localStorage.setItem('theme', e.target.id)
  })
})

document.documentElement.setAttribute('data-theme', currentTheme)
input.setAttribute('checked', true)
```

Вуаля, все работает как задумалось. Даже оставили пользователю возможность выбирать предпочитаемую ему тему, при любом режиме в его устройстве.

<iframe src="https://codepen.io/usmanyunusov/embed/preview/JjKoRWv" title="Пример работы на CodePen."></iframe>

Иногда бывает, что HTML-страница загружается и ждет окончание загрузки CSS файла. Вопрос, что будет, если мы изначально выбрали темный режим и сохранили настройки в браузере? Верно, как только загрузятся стили и сработает скрипт, то произойдет резкий переход с белого на черный, так как изначально наша страница имеет белый фон. Для избежания такого эффекта, есть специальный мета-тег [color-scheme](https://www.w3.org/TR/css-color-adjust-1/#color-scheme-prop), который сообщает таблице стилей агента пользователя, какие цветовые схемы ему использовать по умолчанию.

```html
<meta name="color-scheme" content="dark" />
```

Данную запись можно установить и в CSS:

```css
:root {
  color-scheme: light dark;
}
```

Для поддержки только темной темы нужно указать `dark`. Если поддерживаете обе темы, то `dark light`. Данный функционал работать почти во [всех современных браузерах](https://caniuse.com/?search=name%3A%20color-scheme), кроме Firefox.

На этом пока все!

От себя хочу добавить, что темный режим делает пользовательский интерфейс разнообразнее и интереснее. Для вас это отличная возможность показать, что вы идете в ногу со временем и заботитесь обо всех своих пользователях.

А ты уже перешел на “темную сторону”?
