# Tamizdat — дизайн-система для микроигры

Всё, что нужно, чтобы игра выглядела частью сайта: шрифты, цвета, типографика,
радиусы, брейкпоинты, ритм отступов и визуальные паттерны.

Источник истины — `FrontSources/Tamizdat/src/styles/abstracts/` в репозитории
tamizdat. Значения ниже выгружены из скомпилированного CSS, не переписаны руками.

**Как пользоваться:** скопируй блок `:root` из [Приложения](#приложение-полный-root-копировать-целиком)
в свой CSS и дальше пиши только `var(--color-text-primary)`, `var(--radius-l)` и т.д.
Хардкод хексов — нет; это единственное правило, которое реально важно.

---

## 1. Шрифты

| Роль | Стек | Где берём |
|------|------|-----------|
| Основной (UI, заголовки, интерфейс) | `"Inter Variable", "Inter", sans-serif` | вариативный Inter, вес 100–900, `font-display: swap` |
| Акцидентный (крупные заголовки, «журнальные» врезки) | `"Excelsior LT Std", "Excelsior", serif` | лицензионный файл, лежит в репо — попроси у Александра |
| Дисплейный (одно огромное слово) | `"Retiler", "Excelsior LT Std", serif` | **trial-лицензия**, в игре лучше не использовать |

```css
--font-sans:  "Inter Variable", "Inter", sans-serif;
--font-serif: "Excelsior LT Std", "Excelsior", serif;
```

Веса: только два — **400** (regular) и **600** (semibold). Bold 700 в системе нет.

Inter — вариативный, поэтому во всех текстовых стилях задаётся оптический размер:

```css
font-variation-settings: "opsz" 32;
```

Базовые настройки `body`:

```css
body {
  font-optical-sizing: auto;
  -webkit-font-smoothing: antialiased;
  color: var(--color-text-primary);
  background: var(--color-surface-primary);
}
```

Если Excelsior недоступен — бери только Inter, серифные токены не подменяй
системным serif: пропорции поедут.

---

## 2. Цвета

Три уровня. **В коде игры используй только второй (семантический).**
Первый — сырая палитра, из неё нельзя брать напрямую (исключения: `--white` /
`--black` для чистых заливок и тени).

### Tier 1 — примитивы (не использовать напрямую)

| Токен | Значение | Что это |
|-------|----------|---------|
| `--ink-950` | `#1f2a2e` | самый тёмный сине-серый |
| `--ink-900` | `#263338` | основной текст |
| `--ink-700` | `#576266` | вторичный текст |
| `--ink-500` | `#8a9599` | третичный текст |
| `--ink-300` | `#c2c9cc` | слабый текст, разделители |
| `--paper` | `#fcfaf7` | тёплый фон страницы |
| `--warm-100` | `#f5f3f0` | тёплая подложка |
| `--warm-150` | `#f0eeeb` | тёплая подложка (active) |
| `--warm-200` | `#edebe8` | тёплая подложка потемнее |
| `--indigo` | `#6155f5` | единственный акцент |
| `--white` / `--black` | `#fff` / `#000` | чистые заливки |

Альфа-оверлеи: `--alpha-black-10/12/15/20/40/75`, `--alpha-white-80`.

### Tier 2 — семантические (это и используем)

| Токен | Указывает на | Применение |
|-------|--------------|------------|
| `--color-text-primary` | `--ink-900` | основной текст |
| `--color-text-secondary` | `--ink-700` | подзаголовки, описания |
| `--color-text-tertiary` | `--ink-500` | даты, метаданные, подписи |
| `--color-text-weak` | `--ink-300` | самое тихое: подписи к картинкам, подчёркивания |
| `--color-text-inverse` | `--paper` | текст на тёмном |
| `--color-surface-primary` | `--paper` | фон страницы |
| `--color-surface-secondary` | `--warm-100` | карточки, врезки |
| `--color-surface-secondary-hover` | `--warm-200` | ховер по такой карточке |
| `--color-surface-secondary-active` | `--warm-150` | нажатое состояние |
| `--color-surface-tertiary` | `--warm-200` | плейсхолдеры, футер, тогглы |
| `--color-surface-inverse` | `--ink-950` | тёмные кнопки, инверсные блоки |
| `--color-border` | `rgba(0,0,0,.1)` | все рамки и разделители по умолчанию |
| `--color-border-secondary` | `rgba(0,0,0,.12)` | рамка чуть плотнее |
| `--color-accent` | `--indigo` | акцент (использовать скупо) |
| `--color-overlay` | `rgba(0,0,0,.4)` | затемняющий скрим, тени ховера |
| `--color-overlay-inverse` | `rgba(255,255,255,.8)` | осветляющий скрим |

Шкала текста — порядковая: `primary → secondary → tertiary → weak`, от громкого
к тихому. `inverse`, `accent`, `overlay` — не ранги, а «виды».

Палитра тёплая и приглушённая: бумага вместо белого, сине-серый вместо чёрного.
Единственный яркий цвет в системе — индиго. Если игре нужны сигнальные цвета
(успех/ошибка), их в системе нет — заведи локально и скажи нам.

### Tier 3 — компонентные

`--footer-bg-color`, `--switch-bg-color`, `--bs-btn-*` и т.п. Это оверрайды
Bootstrap и конкретных компонентов сайта — **в игру не тащи**, они там не нужны.

### Тёмная тема

Не сделана. Структурно готова: достаточно переопределить Tier 2 под
`[data-theme="dark"]`. Если делаешь тёмную тему в игре — переопределяй только
`--color-*`, не трогая примитивы.

---

## 3. Типографика

Имя токена: `{семейство}-{брейкпоинт}-{размер}-{вес}-{интерлиньяж}`

- семейство: `sans` | `serif`
- брейкпоинт: `d` (десктоп ≥1200) | `t` (планшет 768–1199) | `m` (мобайл <768)
- размер: `3xl | 2xl | xl | l | m | s | xs`
- вес: `bold` (600) | `reg` (400); у serif опускается — там всегда 400
- интерлиньяж: `loose | normal | tight`

### Sans / Десктоп (Inter)

| Токен | Размер | Интерлиньяж | Трекинг | Вес |
|-------|--------|-------------|---------|-----|
| `sans-d-3xl-bold-normal` | 64px | 64px | -0.96px | 600 |
| `sans-d-2xl-bold-normal` | 46px | 52px | -0.46px | 600 |
| `sans-d-2xl-bold-tight` | 46px | 48px | -0.46px | 600 |
| `sans-d-2xl-reg-normal` | 46px | 52px | -0.46px | 400 |
| `sans-d-xl-bold-normal` | 28px | 36px | -0.28px | 600 |
| `sans-d-xl-bold-tight` | 28px | 28px | -0.28px | 600 |
| `sans-d-xl-reg-loose` | 28px | 38px | 0 | 400 |
| `sans-d-xl-reg-normal` | 28px | 34px | 0 | 400 |
| `sans-d-l-bold-loose` | 18px | 26px | 0 | 600 |
| `sans-d-l-bold-normal` | 18px | 23px | 0 | 600 |
| `sans-d-l-bold-tight` | 18px | 20px | 0 | 600 |
| `sans-d-l-reg-loose` | 18px | 26px | 0 | 400 |
| `sans-d-l-reg-normal` | 18px | 23px | 0 | 400 |
| `sans-d-l-reg-tight` | 18px | 20px | 0 | 400 |
| `sans-d-m-bold-loose` | 16px | 24px | 0 | 600 |
| `sans-d-m-bold-tight` | 16px | 20px | 0.1px | 600 |
| `sans-d-m-reg-loose` | 16px | 24px | 0.1px | 400 |
| `sans-d-m-reg-tight` | 16px | 20px | 0.1px | 400 |
| `sans-d-s-bold-tight` | 14px | 18px | 0.112px | 600 |
| `sans-d-s-reg-loose` | 14px | 20px | 0.11px | 400 |
| `sans-d-s-reg-tight` | 14px | 18px | 0.11px | 400 |
| `sans-d-xs-bold-normal` | 12px | 16px | 0 | 600 |
| `sans-d-xs-bold-tight` | 12px | 14px | 0.11px | 600 |
| `sans-d-xs-reg-normal` | 12px | 16px | 0.11px | 400 |
| `sans-d-xs-reg-tight` | 12px | 14px | 0.11px | 400 |

### Sans / Мобайл (<768px)

| Токен | Размер | Интерлиньяж | Трекинг | Вес |
|-------|--------|-------------|---------|-----|
| `sans-m-xl-bold-normal` | 24px | 26px | 0.048px | 600 |
| `sans-m-l-bold-normal` | 20px | 23px | 0 | 600 |
| `sans-m-m-bold-tight` | 15px | 18px | 0.15px | 600 |
| `sans-m-m-reg-loose` | 15px | 22px | 0.15px | 400 |
| `sans-m-m-reg-tight` | 15px | 18px | 0.15px | 400 |
| `sans-m-s-bold-normal` | 12px | 18px | 0.12px | 600 |
| `sans-m-s-reg-normal` | 12px | 18px | 0.12px | 400 |

### Serif (Excelsior) — для «журнальных» врезок и лидов

| Токен | Размер | Интерлиньяж | Трекинг |
|-------|--------|-------------|---------|
| `serif-d-3xl` | 64px | 64px | -0.64px |
| `serif-d-2xl` | 45px | 48px | -0.9px |
| `serif-d-xl` | 32px | 36px | -0.32px |
| `serif-d-l` | 21px | 32px | -0.21px |
| `serif-t-2xl` | 32px | 34px | -0.64px |
| `serif-t-xl` | 24px | 28px | -0.24px |
| `serif-m-2xl` | 20px | 22px | -0.2px |
| `serif-m-l` | 16px | 24px | -0.16px |

Одиночный токен вне лестницы: `sans-d-review-lead` — 21px / 28px / 0 / 600.

### Как применять

На сайте это SCSS-миксин `@include apply("sans-d-xl-bold-normal")`. В игре
достаточно готовых CSS-переменных — каждый токен разложен на четыре:

```css
.title {
  font-family: var(--font-sans);
  font-size:      var(--type-sans-d-xl-bold-normal-fs);
  line-height:    var(--type-sans-d-xl-bold-normal-lh);
  letter-spacing: var(--type-sans-d-xl-bold-normal-ls);
  font-variation-settings: "opsz" var(--type-sans-d-xl-bold-normal-opsz);
  font-weight: 600; /* вес в переменные не вынесен — бери из таблицы */
}
```

### Адаптивная типографика — правило

Типографика **desktop-first**. Пишем десктопный токен, затем перекрываем
через `max-width`-медиазапрос:

```css
.author {
  /* ...sans-d-s-reg-tight... */
}
@media (max-width: 768px) {
  .author { /* ...sans-m-s-reg-normal... */ }
}
```

Перекрывай **только те брейкпоинты, для которых стиль реально задан в макете**.
Не придумывай промежуточные ступени.

---

## 4. Радиусы

| Токен | Значение | Где |
|-------|----------|-----|
| `--radius-s` | `4px` | картинки, превью |
| `--radius-m` | `6px` | инпуты, мелкие контролы |
| `--radius-l` | `8px` | карточки, врезки, панели |
| `--radius-round` | `999px` | пилюли, кнопки-таблетки, аватары, точки-сепараторы |

Других радиусов в системе нет.

---

## 5. Брейкпоинты

| Имя | Значение |
|-----|----------|
| `sm` | 576px |
| `md` | **768px** — граница «мобайл / не мобайл» |
| `lg` | 992px |
| `xl` | **1200px** — граница «планшет / десктоп» |
| `xxl` | 1400px |

Медиазапросы **desktop-first**: `@media screen and (max-width: 768px)`.
Рабочих границ на практике две: `md` (768) и `xl` (1200).

---

## 6. Ритм отступов

Формальной шкалы `--space-*` в системе нет — отступы пишутся значениями.
Но де-факто ритм такой (по частоте употребления в коде):

| Значение | Употребление |
|----------|--------------|
| `0.25rem` (4px) | микро-зазор внутри строки |
| `0.5rem` (8px) | подпись под элементом |
| `0.75rem` (12px) | плотный внутренний отступ |
| **`1rem` (16px)** | базовый шаг — самый частый |
| **`1.5rem` (24px)** | **gap между карточками, padding карточки** — второй по частоте |
| `2rem` (32px) | padding контейнера по горизонтали, блок текста |
| `40px` | отступ сверху у секции |
| `80px` / `120px` | вертикальные интервалы между крупными секциями |

Практическое правило: **сетки и карточки — `gap: 1.5rem`**, всё внутреннее —
кратно `0.5rem`, между секциями — 40 / 80 / 120px.

---

## 7. Размерности макета

```css
--container-max: 1712px;       /* макс. ширина контентного контейнера */
--container-pad: 2rem;         /* горизонтальные поля; 1rem на <768px */
--review-content-width: 970px; /* широкая колонка статьи */
--review-text-width: 810px;    /* текстовая мера — на неё садится текст */
```

Ниже 1200px все три ширины схлопываются в 100%.

---

## 8. Визуальные паттерны

### Ссылки

Подчёркивание появляется на ховере и всегда тихое:

```css
a { color: inherit; text-decoration: none; }
a:hover {
  text-decoration: underline;
  text-decoration-color: var(--color-text-weak);
  text-decoration-thickness: 1px;
  text-underline-offset: 4px;
}
```

Это фирменный приём: тонкая светлая линия с отбивкой 4px, не жирное синее
подчёркивание. У карточки-ссылки подчёркивается **только заголовок**, а не весь
блок (`a:hover { text-decoration: none }` на карточке + правило на заголовке).

### Кнопки

Два вида.

**Основная** — тёмная заливка, без рамки:

```css
.btn-primary {
  background: var(--color-surface-inverse);
  border: 1px solid var(--color-surface-inverse);
  color: var(--color-text-inverse);
  border-radius: var(--radius-round);
}
```

**Вторичная («ещё»)** — прозрачная пилюля с тонкой рамкой:

```css
.btn-more {
  background: transparent;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-round);
  padding: 0 1.5rem;
  /* типографика: sans-d-s-bold-tight (14px/600) */
  line-height: 1.75rem;  /* 28px задаёт высоту кнопки */
}
```

Кнопки — **всегда пилюли** (`--radius-round`), горизонтальный padding `1.5rem`.

### Карточки и врезки

```css
.card {
  background: var(--color-surface-secondary);
  border-radius: var(--radius-l);
  padding: 1rem;          /* или 1.5rem для крупных */
}
```

Разделители между элементами списка — не рамка вокруг, а линия сверху:

```css
.list-item {
  border-top: 1px solid var(--color-border);
  padding: 1rem 0;
}
```

### Тени

Тень — декоративная, не токенизирована, привязана к компоненту.
Работающие рецепты:

```css
/* картинка / превью */
box-shadow: 0 5px 10px -5px var(--alpha-black-20);
/* та же картинка на ховере — мягче и шире */
box-shadow: 0 5px 15px 0 var(--color-overlay);
/* плавающий элемент (стрелка карусели) */
box-shadow: 0 4px 7px var(--alpha-black-10);
/* крупная «физическая» карточка (обложка) — многослойная */
box-shadow:
  0 88px 17.5px rgba(0, 0, 0, 0.01),
  0 50px 15px   rgba(0, 0, 0, 0.03),
  0 22px 11px   rgba(0, 0, 0, 0.05),
  0 6px  6px    rgba(0, 0, 0, 0.06);
```

### Картинки

Пропорции задаются через `aspect-ratio`, кроп — `object-fit: cover`,
скругление `--radius-s`. Обложки книг — `object-fit: contain` с фиксированной
высотой, чтобы ничего не резать. Плейсхолдер на месте несъехавшей картинки:
заливка `--color-surface-tertiary`, а не белая дыра.

```css
.picture { border-radius: var(--radius-s); overflow: hidden; }
.picture .image { width: 100%; height: 100%; object-fit: cover; }
.placeholder { background: var(--color-surface-tertiary); }
```

### Ховер-зум

Главная микроинтеракция сайта — картинка наезжает внутри неподвижной рамки:

```css
.hover-zoom .picture { overflow: hidden; }
.hover-zoom .image   { transition: transform 0.2s ease-in-out; }
.hover-zoom:hover .image { transform: scale(1.05); }

@media (prefers-reduced-motion: reduce) {
  .hover-zoom .image,
  .hover-zoom:hover .image { transition: none; transform: none; }
}
```

**`prefers-reduced-motion` обязателен** для любой анимации в игре.

### Анимации

Движение короткое и сдержанное:

| Что | Значение |
|-----|----------|
| Трансформации (зум, сдвиг) | `0.2s ease-in-out` |
| Появление/исчезание | `0.2s ease` |
| Раскрытие панели, аккордеон | `0.25s cubic-bezier(0.17, 0.67, 0.09, 0.97)` |
| Крупный слайд (карусель, меню) | `0.4s cubic-bezier(0.03, 0.96, 0.52, 0.98)` |

Ничего дольше 0.5s. Никаких bounce и spring — тон архивный, не игривый
(при том, что это игра — движение всё равно держим тихим).

### Точка-сепаратор в строке метаданных

Даты, рубрики и прочая метаинформация разделяются крошечной точкой:

```css
.meta .item::after {
  content: '';
  display: inline-block;
  width: 2px; height: 2px;
  background: var(--color-text-weak);
  border-radius: var(--radius-round);
  margin: auto 0.5rem;
  vertical-align: middle;
}
.meta .item:last-child::after { content: none; }
```

Сама строка — `sans-d-s-reg-tight` цветом `--color-text-tertiary`.

### Аватары внахлёст

Круглые, перекрывают друг друга и обведены цветом подложки — так группа
читается как стопка:

```css
.avatar {
  border-radius: var(--radius-round);
  overflow: hidden;
  margin-left: -1rem;
  outline: 4px solid var(--color-surface-secondary);
}
.avatar:first-child { margin-left: 0; }
```

### Подпись под изображением

```css
.image-caption {
  max-width: 42rem;
  margin-top: 0.5rem;
  color: var(--color-text-weak);
  /* типографика: sans-d-s-reg-loose */
}
```

---

## 9. Общий тон

- Тёплая бумага вместо белого, сине-серый вместо чёрного.
- Один акцентный цвет, применяется скупо.
- Иерархия строится **цветом текста** (primary → tertiary), а не размером и не рамками.
- Рамок мало; разделение — линией сверху и сменой подложки.
- Пилюли для кнопок, мягкое скругление 8px для карточек.
- Движение тихое и короткое.

---

## 10. Чего в системе нет

Если игре нужно — заводи у себя локально и предупреди нас, чтобы мы это
потом подняли в систему:

- сигнальные цвета (success / warning / error);
- тёмная тема;
- формальная шкала отступов `--space-*`;
- токены теней;
- состояния focus-visible (кроме `--form-shadow` у полей ввода);
- иконки: на сайте иконочный шрифт Fontello — **в игру не тащи**, бери SVG.

---

## Приложение: полный `:root` (копировать целиком)

Выгружено из скомпилированного `base/_root.scss`. Вставь в свой CSS и дальше
работай только через `var()`.

```css
:root {
  --ink-950: #1f2a2e;
  --ink-900: #263338;
  --ink-700: #576266;
  --ink-500: #8a9599;
  --ink-300: #c2c9cc;
  --paper: #fcfaf7;
  --warm-100: #f5f3f0;
  --warm-150: #f0eeeb;
  --warm-200: #edebe8;
  --indigo: #6155f5;
  --alpha-black-10: rgba(0, 0, 0, 0.1);
  --alpha-black-12: rgba(0, 0, 0, 0.12);
  --alpha-black-15: rgba(0, 0, 0, 0.15);
  --alpha-black-20: rgba(0, 0, 0, 0.2);
  --alpha-black-40: rgba(0, 0, 0, 0.4);
  --alpha-black-75: rgba(0, 0, 0, 0.75);
  --alpha-white-80: rgba(255, 255, 255, 0.8);
  --white: #fff;
  --black: #000;
  --color-text-primary: var(--ink-900);
  --color-text-secondary: var(--ink-700);
  --color-text-tertiary: var(--ink-500);
  --color-text-weak: var(--ink-300);
  --color-text-inverse: var(--paper);
  --color-surface-primary: var(--paper);
  --color-surface-secondary: var(--warm-100);
  --color-surface-secondary-hover: var(--warm-200);
  --color-surface-secondary-active: var(--warm-150);
  --color-surface-tertiary: var(--warm-200);
  --color-surface-inverse: var(--ink-950);
  --color-border: var(--alpha-black-10);
  --color-border-secondary: var(--alpha-black-12);
  --color-accent: var(--indigo);
  --color-overlay: var(--alpha-black-40);
  --color-overlay-inverse: var(--alpha-white-80);
  --bs-body-font-family: "Inter Variable", sans-serif;
  --bs-body-bg: var(--color-surface-primary);
  --bg-form: var(--color-text-tertiary);
  --form-shadow: 0 0 0 0.25rem var(--alpha-black-12);
  --bs-btn-bg: var(--color-surface-inverse);
  --bs-btn-border-color: var(--color-surface-inverse);
  --bs-btn-more-border-color: #0000;
  --bs-btn-more-color-hover: var(--white);
  --footer-bg-color: var(--color-surface-tertiary);
  --footer-border-color: var(--color-border);
  --switches-bg-color: var(--color-surface-tertiary);
  --switches-label-color: var(--color-text-primary);
  --switch-bg-color: var(--white);
  --switch-text-color: var(--color-text-secondary);
  --bs-breakpoint-xs: 0;
  --bs-breakpoint-sm: 576px;
  --bs-breakpoint-md: 768px;
  --bs-breakpoint-lg: 992px;
  --bs-breakpoint-xl: 1200px;
  --bs-breakpoint-xxl: 1400px;
  --radius-s: 4px;
  --radius-m: 6px;
  --radius-l: 8px;
  --radius-round: 999px;
}

:root {
  --type-sans-d-3xl-bold-normal-fs: 64px;
  --type-sans-d-3xl-bold-normal-lh: 64px;
  --type-sans-d-3xl-bold-normal-ls: -0.96px;
  --type-sans-d-3xl-bold-normal-opsz: 32;
  --type-sans-d-2xl-bold-normal-fs: 46px;
  --type-sans-d-2xl-bold-normal-lh: 52px;
  --type-sans-d-2xl-bold-normal-ls: -0.46px;
  --type-sans-d-2xl-bold-normal-opsz: 32;
  --type-sans-d-2xl-bold-tight-fs: 46px;
  --type-sans-d-2xl-bold-tight-lh: 48px;
  --type-sans-d-2xl-bold-tight-ls: -0.46px;
  --type-sans-d-2xl-bold-tight-opsz: 32;
  --type-sans-d-2xl-reg-normal-fs: 46px;
  --type-sans-d-2xl-reg-normal-lh: 52px;
  --type-sans-d-2xl-reg-normal-ls: -0.46px;
  --type-sans-d-2xl-reg-normal-opsz: 32;
  --type-sans-d-xl-bold-normal-fs: 28px;
  --type-sans-d-xl-bold-normal-lh: 36px;
  --type-sans-d-xl-bold-normal-ls: -0.28px;
  --type-sans-d-xl-bold-normal-opsz: 32;
  --type-sans-d-xl-bold-tight-fs: 28px;
  --type-sans-d-xl-bold-tight-lh: 28px;
  --type-sans-d-xl-bold-tight-ls: -0.28px;
  --type-sans-d-xl-bold-tight-opsz: 32;
  --type-sans-d-xl-reg-loose-fs: 28px;
  --type-sans-d-xl-reg-loose-lh: 38px;
  --type-sans-d-xl-reg-loose-ls: 0px;
  --type-sans-d-xl-reg-loose-opsz: 32;
  --type-sans-d-xl-reg-normal-fs: 28px;
  --type-sans-d-xl-reg-normal-lh: 34px;
  --type-sans-d-xl-reg-normal-ls: 0px;
  --type-sans-d-xl-reg-normal-opsz: 32;
  --type-sans-d-l-bold-loose-fs: 18px;
  --type-sans-d-l-bold-loose-lh: 26px;
  --type-sans-d-l-bold-loose-ls: 0px;
  --type-sans-d-l-bold-loose-opsz: 32;
  --type-sans-d-l-bold-normal-fs: 18px;
  --type-sans-d-l-bold-normal-lh: 23px;
  --type-sans-d-l-bold-normal-ls: 0px;
  --type-sans-d-l-bold-normal-opsz: 32;
  --type-sans-d-l-bold-tight-fs: 18px;
  --type-sans-d-l-bold-tight-lh: 20px;
  --type-sans-d-l-bold-tight-ls: 0px;
  --type-sans-d-l-bold-tight-opsz: 32;
  --type-sans-d-l-reg-loose-fs: 18px;
  --type-sans-d-l-reg-loose-lh: 26px;
  --type-sans-d-l-reg-loose-ls: 0px;
  --type-sans-d-l-reg-loose-opsz: 32;
  --type-sans-d-l-reg-normal-fs: 18px;
  --type-sans-d-l-reg-normal-lh: 23px;
  --type-sans-d-l-reg-normal-ls: 0px;
  --type-sans-d-l-reg-normal-opsz: 32;
  --type-sans-d-l-reg-tight-fs: 18px;
  --type-sans-d-l-reg-tight-lh: 20px;
  --type-sans-d-l-reg-tight-ls: 0px;
  --type-sans-d-l-reg-tight-opsz: 32;
  --type-sans-d-m-bold-loose-fs: 16px;
  --type-sans-d-m-bold-loose-lh: 24px;
  --type-sans-d-m-bold-loose-ls: 0px;
  --type-sans-d-m-bold-loose-opsz: 32;
  --type-sans-d-m-bold-tight-fs: 16px;
  --type-sans-d-m-bold-tight-lh: 20px;
  --type-sans-d-m-bold-tight-ls: 0.1px;
  --type-sans-d-m-bold-tight-opsz: 32;
  --type-sans-d-m-reg-loose-fs: 16px;
  --type-sans-d-m-reg-loose-lh: 24px;
  --type-sans-d-m-reg-loose-ls: 0.1px;
  --type-sans-d-m-reg-loose-opsz: 32;
  --type-sans-d-m-reg-tight-fs: 16px;
  --type-sans-d-m-reg-tight-lh: 20px;
  --type-sans-d-m-reg-tight-ls: 0.1px;
  --type-sans-d-m-reg-tight-opsz: 32;
  --type-sans-d-s-bold-tight-fs: 14px;
  --type-sans-d-s-bold-tight-lh: 18px;
  --type-sans-d-s-bold-tight-ls: 0.112px;
  --type-sans-d-s-bold-tight-opsz: 32;
  --type-sans-d-s-reg-loose-fs: 14px;
  --type-sans-d-s-reg-loose-lh: 20px;
  --type-sans-d-s-reg-loose-ls: 0.11px;
  --type-sans-d-s-reg-loose-opsz: 32;
  --type-sans-d-s-reg-tight-fs: 14px;
  --type-sans-d-s-reg-tight-lh: 18px;
  --type-sans-d-s-reg-tight-ls: 0.11px;
  --type-sans-d-s-reg-tight-opsz: 32;
  --type-sans-d-xs-bold-normal-fs: 12px;
  --type-sans-d-xs-bold-normal-lh: 16px;
  --type-sans-d-xs-bold-normal-ls: 0px;
  --type-sans-d-xs-bold-normal-opsz: 32;
  --type-sans-d-xs-bold-tight-fs: 12px;
  --type-sans-d-xs-bold-tight-lh: 14px;
  --type-sans-d-xs-bold-tight-ls: 0.11px;
  --type-sans-d-xs-bold-tight-opsz: 32;
  --type-sans-d-xs-reg-normal-fs: 12px;
  --type-sans-d-xs-reg-normal-lh: 16px;
  --type-sans-d-xs-reg-normal-ls: 0.11px;
  --type-sans-d-xs-reg-normal-opsz: 32;
  --type-sans-d-xs-reg-tight-fs: 12px;
  --type-sans-d-xs-reg-tight-lh: 14px;
  --type-sans-d-xs-reg-tight-ls: 0.11px;
  --type-sans-d-xs-reg-tight-opsz: 32;
  --type-sans-m-xl-bold-normal-fs: 24px;
  --type-sans-m-xl-bold-normal-lh: 26px;
  --type-sans-m-xl-bold-normal-ls: 0.048px;
  --type-sans-m-xl-bold-normal-opsz: 32;
  --type-sans-m-l-bold-normal-fs: 20px;
  --type-sans-m-l-bold-normal-lh: 23px;
  --type-sans-m-l-bold-normal-ls: 0px;
  --type-sans-m-l-bold-normal-opsz: 32;
  --type-sans-m-m-bold-tight-fs: 15px;
  --type-sans-m-m-bold-tight-lh: 18px;
  --type-sans-m-m-bold-tight-ls: 0.15px;
  --type-sans-m-m-bold-tight-opsz: 32;
  --type-sans-m-m-reg-loose-fs: 15px;
  --type-sans-m-m-reg-loose-lh: 22px;
  --type-sans-m-m-reg-loose-ls: 0.15px;
  --type-sans-m-m-reg-loose-opsz: 32;
  --type-sans-m-m-reg-tight-fs: 15px;
  --type-sans-m-m-reg-tight-lh: 18px;
  --type-sans-m-m-reg-tight-ls: 0.15px;
  --type-sans-m-m-reg-tight-opsz: 32;
  --type-sans-m-s-bold-normal-fs: 12px;
  --type-sans-m-s-bold-normal-lh: 18px;
  --type-sans-m-s-bold-normal-ls: 0.12px;
  --type-sans-m-s-bold-normal-opsz: 32;
  --type-sans-m-s-reg-normal-fs: 12px;
  --type-sans-m-s-reg-normal-lh: 18px;
  --type-sans-m-s-reg-normal-ls: 0.12px;
  --type-sans-m-s-reg-normal-opsz: 32;
  --type-serif-d-3xl-fs: 64px;
  --type-serif-d-3xl-lh: 64px;
  --type-serif-d-3xl-ls: -0.64px;
  --type-serif-d-3xl-opsz: 32;
  --type-serif-d-2xl-fs: 45px;
  --type-serif-d-2xl-lh: 48px;
  --type-serif-d-2xl-ls: -0.9px;
  --type-serif-d-2xl-opsz: 32;
  --type-serif-d-xl-fs: 32px;
  --type-serif-d-xl-lh: 36px;
  --type-serif-d-xl-ls: -0.32px;
  --type-serif-d-xl-opsz: 32;
  --type-serif-d-l-fs: 21px;
  --type-serif-d-l-lh: 32px;
  --type-serif-d-l-ls: -0.21px;
  --type-serif-d-l-opsz: 32;
  --type-serif-t-2xl-fs: 32px;
  --type-serif-t-2xl-lh: 34px;
  --type-serif-t-2xl-ls: -0.64px;
  --type-serif-t-2xl-opsz: 32;
  --type-serif-t-xl-fs: 24px;
  --type-serif-t-xl-lh: 28px;
  --type-serif-t-xl-ls: -0.24px;
  --type-serif-t-xl-opsz: 32;
  --type-serif-m-2xl-fs: 20px;
  --type-serif-m-2xl-lh: 22px;
  --type-serif-m-2xl-ls: -0.2px;
  --type-serif-m-2xl-opsz: 32;
  --type-serif-m-l-fs: 16px;
  --type-serif-m-l-lh: 24px;
  --type-serif-m-l-ls: -0.16px;
  --type-serif-m-l-opsz: 32;
  --type-sans-d-review-lead-fs: 21px;
  --type-sans-d-review-lead-lh: 28px;
  --type-sans-d-review-lead-ls: 0px;
  --type-sans-d-review-lead-opsz: 32;
}

/* ── Добавка для игры ──────────────────────────────────────────────────
   Этих переменных в CSS сайта нет: там семейства шрифтов и ширины живут
   в SCSS. Для игры удобнее иметь их как обычные var(). */
:root {
  --font-sans:  "Inter Variable", "Inter", sans-serif;
  --font-serif: "Excelsior LT Std", "Excelsior", serif;

  --container-max: 1712px;
  --container-pad: 2rem;
  --review-content-width: 60.625rem; /* 970px */
  --review-text-width:    50.625rem; /* 810px */
}

@media screen and (max-width: 768px) {
  :root { --container-pad: 1rem; }
}
```
