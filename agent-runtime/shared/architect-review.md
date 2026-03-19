# Архитектурный обзор: Aqua Odin - сайт доставки воды

**Дата ревью:** 2026-03-19
**Файлы:** `index.html` (~1400 строк), `style.css` (~2200 строк)
**Тип проекта:** Одностраничный сайт (SPA-like landing) доставки воды

---

## 1. Структура секций

### Текущий порядок секций:
1. Header (фиксированный)
2. Hero (заглавный экран)
3. About (о воде, факты + статистика)
4. Benefits (преимущества)
5. Funnel (как это работает - 4 шага)
6. Products (каталог: вода / кулеры / аксессуары)
7. Reviews (отзывы)
8. FAQ (часто задаваемые вопросы)
9. CTA (финальный призыв к действию)
10. Footer

### Оценка

**Порядок логичен и соответствует классической воронке конверсии** - от знакомства с продуктом до целевого действия. Переходы между секциями реализованы SVG wave-dividers, что визуально связывает блоки.

#### Имеющиеся блоки - что хорошо:
- Промо-баннер внутри Products ("первый заказ - особенные условия")
- Sticky CTA для мобильных устройств
- WhatsApp-виджет (плавающая кнопка)
- JSON-LD микроразметка LocalBusiness
- Product modal с детальной информацией и сертификатами

#### Чего не хватает:

> **`[SEVERITY]` Отсутствующие блоки**

| Блок | Уровень |
|------|---------|
| Форма заказа (имя, телефон, адрес) | :red_circle: критично |
| Карта зоны доставки (Яндекс.Карты / интерактив) | :yellow_circle: важно |
| Секция "Контакты" (отдельная, а не только в footer) | :yellow_circle: важно |
| Калькулятор стоимости заказа | :yellow_circle: важно |
| Блок "Для бизнеса" / корпоративные клиенты | :green_circle: рекомендация |
| Блог / полезные статьи (для SEO) | :green_circle: рекомендация |

:red_circle: **Критично: отсутствие формы заказа.** Главная кнопка CTA (`Заказать доставку`) ведёт на `tel:+74952336471` - это звонок по телефону. Для сайта доставки воды обязательна веб-форма: не все пользователи готовы звонить, особенно в нерабочее время. Нужно минимум: имя, телефон, адрес, выбор воды, количество.

```html
<!-- Рекомендуемая структура формы в секции CTA -->
<form class="order-form" id="orderForm" action="#" method="POST">
  <div class="form-group">
    <label class="form-label" for="name">Ваше имя</label>
    <input class="form-input" type="text" id="name" name="name" required>
  </div>
  <div class="form-group">
    <label class="form-label" for="phone">Телефон</label>
    <input class="form-input" type="tel" id="phone" name="phone" required>
  </div>
  <div class="form-group">
    <label class="form-label" for="address">Адрес доставки</label>
    <input class="form-input" type="text" id="address" name="address">
  </div>
  <button type="submit" class="btn-primary">Отправить заявку</button>
</form>
```

:yellow_circle: **Важно:** Навигация в header содержит ссылку `href="#faq"` с текстом "Контакты", но ведёт она на FAQ. В мобильном меню набор ссылок отличается от десктопного (есть "Отзывы" и "FAQ", но нет "Контакты"). Нужно привести к единообразию.

---

## 2. Семантика HTML

### Что реализовано корректно:

- `<header>`, `<main>`, `<footer>` - базовая структура документа
- `<nav>` с `aria-label="Основная навигация"` - грамотная навигация
- `<section>` с уникальными `id` для каждого логического блока
- Skip-link (`<a href="#main" class="skip-link">`) для доступности
- ARIA-атрибуты: `aria-expanded`, `aria-hidden`, `aria-label`, `role="tablist"`, `role="tab"`, `role="tabpanel"`
- Структурированные данные JSON-LD (LocalBusiness)
- Open Graph мета-теги
- `lang="ru"` на `<html>`

### Проблемы:

:yellow_circle: **Важно: Отсутствие `<article>` для карточек отзывов.** Каждый отзыв - самостоятельная единица контента и семантически является `<article>`:

```html
<!-- Сейчас -->
<div class="review-card">...</div>

<!-- Рекомендация -->
<article class="review-card">...</article>
```

:yellow_circle: **Важно: Product cards имеют `role="button"` и `tabindex="0"` на `<div>`.** Это антипаттерн - если элемент должен быть интерактивным, лучше использовать `<button>` или `<a>`. Сейчас `<div>` с `role="button"` не обрабатывает нажатие Enter/Space без дополнительного JS:

```html
<!-- Сейчас -->
<div class="product-card" data-product="tverskaya" role="button" tabindex="0">

<!-- Рекомендация: обёртка в кликабельный элемент -->
<button class="product-card" data-product="tverskaya" type="button">
```

:yellow_circle: **Важно: Tab panels не связаны с кнопками через `aria-labelledby` / `aria-controls`.** Для корректной работы скринридеров:

```html
<button role="tab" id="tab-water" aria-controls="panel-water">Вода 19л</button>
<div role="tabpanel" id="panel-water" aria-labelledby="tab-water">...</div>
```

:green_circle: **Рекомендация:** FAQ-аккордеон использует `<button>` с `aria-expanded` - это правильно. Но `<div class="faq-item__answer" role="region">` не имеет `aria-labelledby`, указывающего на вопрос. Стоит добавить связку.

:green_circle: **Рекомендация:** SVG-иконки повторяются десятки раз (телефон, щит, капля). Стоит вынести в `<svg><defs>` / sprite и использовать `<use xlink:href="#icon-phone">` для уменьшения размера HTML.

---

## 3. Адаптивность

### Брейкпоинты:
- **480px** - малые экраны (кнопки hero в ряд, логотип с текстом, grid продуктов 2 колонки)
- **768px** - планшеты (hero в row, facts 3 колонки, footer 2 колонки, header с телефоном)
- **1024px** - десктоп (навигация, burger скрыт, benefits 3 колонки, funnel 4 колонки, footer 4 колонки)
- **1280px** - упоминается в CSS-переменных, но не используется в media queries
- **1440px** - широкие экраны (контейнер расширяется до 1320px)

### Что реализовано хорошо:

- **Mobile-first подход** - базовые стили для мобильных, `min-width` media queries для расширения
- **Гибкие grid-лейауты** - `grid-template-columns` адаптируется: 1fr -> repeat(2) -> repeat(3/4)
- **Мобильное меню** - полноэкранный overlay с `100dvh` (учтена проблема Safari)
- **Sticky CTA для мобильных** - появляется после 30% скролла, скрывается на десктопе
- **Модалка продуктов** - на мобильных (<480px) превращается в bottom sheet
- **Touch swipe** для слайдера отзывов с `{ passive: true }`
- **`prefers-reduced-motion`** - отключение анимаций для пользователей с соответствующей настройкой

### Проблемы:

:yellow_circle: **Важно: Нет `max-width: 480px` media query для очень маленьких экранов.** Модалка продуктов использует `max-width: 480px`, но основные стили полагаются только на `min-width`. На экранах 320px некоторые элементы могут выглядеть тесно (особенно hero__buttons и products__tabs с горизонтальным скроллом).

:yellow_circle: **Важно: Hero секция `min-height: 100vh`** - на мобильных устройствах с динамическими toolbars это может привести к обрезанию контента. Лучше использовать `min-height: 100dvh` (как сделано для mobile-menu, но не для hero):

```css
.hero {
  min-height: 100vh;
  min-height: 100dvh; /* progressive enhancement */
}
```

:green_circle: **Рекомендация:** Слайдер отзывов - единственная карточка видна целиком, нет peek-эффекта для соседних карточек. На планшетах (768px+) можно показывать 2 карточки одновременно для лучшего использования пространства.

:green_circle: **Рекомендация:** `products__tabs` имеет `overflow-x: auto` для горизонтального скролла, но нет визуальной индикации (fade/gradient по краям), что есть ещё табы за пределами экрана.

---

## 4. 3D-заглушка

### Текущая реализация:

В hero-секции присутствует заглушка под 3D-объект:

```html
<div id="3d-hero-placeholder" class="hero__3d">
  <!-- Three.js / Spline mount point -->
  <div class="hero__visual">
    <div class="hero__bottle"></div>
  </div>
</div>
```

CSS-реализация заглушки:
- **Форма бутылки** создана чистым CSS через `border-radius: 40% 40% 45% 45% / 20% 20% 50% 50%` с полупрозрачным фоном и бордером
- **Анимация покачивания** (`@keyframes float`) - вертикальное движение на 12px с easing
- **Имитация воды** через `::before` с анимацией `waterWave` (масштабирование по X)
- **Текст "19L"** через `::after` псевдоэлемент
- Адаптивные размеры: 200x280px (мобильный) -> 280x380px (планшет+)

### Оценка:

:green_circle: **Хорошо:** Элемент подготовлен как mount point (`id="3d-hero-placeholder"`) с комментарием "Three.js / Spline mount point". Это позволяет легко подключить 3D без перестройки HTML.

:yellow_circle: **Важно: Отсутствует `<canvas>` элемент.** Для Three.js / WebGL необходим canvas. Рекомендуется добавить:

```html
<div id="3d-hero-placeholder" class="hero__3d">
  <canvas id="hero-canvas" class="hero__canvas"></canvas>
  <!-- CSS fallback показывается когда canvas не инициализирован -->
  <div class="hero__visual hero__fallback">
    <div class="hero__bottle"></div>
  </div>
</div>
```

```css
.hero__canvas {
  width: 100%;
  height: 100%;
  position: absolute;
  inset: 0;
}
/* Скрываем fallback когда 3D загружен */
.hero__3d.loaded .hero__fallback {
  display: none;
}
```

:yellow_circle: **Важно:** Нет нигде подключения three.js, @splinetool/runtime или других 3D-библиотек. Нет `<script>` с подключением, нет lazy-loading стратегии. Для продакшена нужно:

```html
<!-- Lazy-load 3D после основного контента -->
<script type="module">
  if (window.matchMedia('(min-width: 768px)').matches) {
    import('/js/hero-3d.js').then(m => m.init());
  }
</script>
```

:green_circle: **Рекомендация:** CSS fallback-бутылка хорошо работает как заглушка. Декоративные пузыри (`hero__bubbles`, `cta-block__bubbles`) с `@keyframes floatUp` создают тематическую атмосферу. При подключении 3D имеет смысл заменить CSS-пузыри на particle system в canvas для единообразия.

---

## 5. Общая архитектура

### CSS Design System

:green_circle: **Сильная сторона - дизайн-токены.** В `:root` определена полная система переменных:
- **Цвета:** 5 палитр (primary, secondary, accent, cta, neutral) с градациями 50-900
- **Типографика:** 3 шрифта (Montserrat/Inter/JetBrains Mono) + масштаб размеров
- **Spacing:** от 4px до 96px (space-1 до space-24)
- **Тени:** 4 уровня (sm, md, lg, cta)
- **Радиусы:** 5 вариантов (sm, md, lg, xl, full)
- **Transitions:** 4 варианта с разными easing-функциями
- **Градиенты:** 5 готовых градиентов

Это обеспечивает единообразие и масштабируемость.

### Организация CSS

Файл организован по секциям с чёткими комментариями:
1. CSS Custom Properties
2. Reset & Base
3. Utility (skip-link, container)
4. Buttons (primary, secondary, ghost)
5. Header + Mobile Menu
6. Wave Dividers
7. Section Common
8. Per-section styles (Hero, About, Benefits, Funnel, Products, Reviews, FAQ, CTA)
9. Footer
10. Widgets (WhatsApp, Sticky CTA)
11. Animations
12. Media queries (дополнительные)
13. Form styles
14. Product Modal
15. Glow Effects

### Проблемы архитектуры:

:red_circle: **Критично: весь JS инлайнится в HTML.** ~250 строк JavaScript находятся внутри `<script>` в конце `index.html`. Включает: header scroll, burger menu, smooth scroll, IntersectionObserver анимации, countUp, tabs, slider с autoplay/touch, FAQ accordion, sticky CTA, active nav, product modal с данными всех 10 продуктов. Это:
- Невозможно кешировать отдельно от HTML
- Невозможно тестировать юнит-тестами
- Затрудняет поддержку при масштабировании

**Рекомендация:** Вынести в отдельные модули:
```
/js/
  app.js           (init, event delegation)
  header.js        (scroll, burger)
  slider.js        (reviews carousel)
  tabs.js          (product tabs)
  accordion.js     (FAQ)
  modal.js         (product modal)
  animations.js    (IntersectionObserver, countUp)
  data/products.js (product data)
```

:yellow_circle: **Важно: BEM-нейминг непоследователен.** В целом используется BEM (`header__logo`, `product-card__title`), но есть нарушения:
- `.bubble` - нет блока-родителя в имени (должен быть `.hero__bubble` или `.decoration-bubble`)
- `.step` вместо `.funnel__step`
- `.stat` вместо `.about__stat`
- `.tab-btn` вместо `.products__tab-btn`

:yellow_circle: **Важно: Дублирование media queries.** Одни и те же брейкпоинты описаны в нескольких местах. Например, `@media (min-width: 768px)` встречается ~15 раз. Для лучшей поддерживаемости стоит группировать media queries рядом с компонентом (что частично сделано), но также есть отдельный блок media queries в конце файла (строки 1854-1883), который дублирует уже объявленные правила (`.hero__title` 3.5rem уже задан в строке 678-681).

:yellow_circle: **Важно: Inline styles в HTML.** Несколько элементов используют `style=""` атрибуты:
- Wave dividers: `style="position: absolute; bottom: 0; ..."`, `style="margin-top: var(--space-10);"`
- Badge цвета: `style="background: #F28C28;"`, `style="background: #0D8F7F;"` и др.
- Мелкие стили: `style="font-size: 0.85rem; opacity: 0.7; ..."` на hero subtitle

Это нарушает принцип разделения и затрудняет поддержку. Все стили должны быть в CSS.

```css
/* Вместо inline style="background: #F28C28;" */
.product-card__badge--family { background: var(--color-cta-500); }
.product-card__badge--category { background: var(--color-accent-600); }
.product-card__badge--source { background: var(--color-primary-500); }
```

:green_circle: **Рекомендация: нет CSS-препроцессора или модулей.** Для проекта такого размера (~2200 строк CSS) стоит рассмотреть:
- CSS-модули или PostCSS с `@import` для разбивки по компонентам
- Или хотя бы конкатенацию нескольких файлов при сборке

:green_circle: **Рекомендация: SVG-изображения продуктов инлайнятся в HTML.** Каждая карточка продукта содержит SVG-заглушку бутылки (~6-8 строк SVG). При 10 карточках + modal + productData в JS это значительный объём повторяющегося кода. Стоит:
- Вынести SVG в отдельные файлы и подгружать через `<img>` или `<use>`
- Или генерировать программно из параметров (цвет, текст)

---

## 6. Производительность и SEO

:green_circle: **Хорошо:**
- `<link rel="preconnect">` для Google Fonts
- `font-display=swap` в URL шрифтов
- `{ passive: true }` для scroll/touch listeners
- `IntersectionObserver` вместо scroll-based проверок
- JSON-LD микроразметка
- Meta description, keywords, Open Graph
- `scroll-behavior: smooth` в CSS (а не только JS)

:yellow_circle: **Важно:** Три шрифта загружаются одним запросом (Montserrat 500-800, Inter 400-600, JetBrains Mono 500-700) - это 10 начертаний. JetBrains Mono используется только для цен. Стоит загружать его асинхронно или заменить на системный `monospace`.

:yellow_circle: **Важно:** Нет `<link rel="icon">` (favicon).

:green_circle: **Рекомендация:** Нет `<meta name="robots">`, `<link rel="canonical">`, `sitemap.xml` - стоит добавить для SEO.

---

## Итоговая таблица

| Категория | Оценка | Комментарий |
|-----------|--------|-------------|
| Структура секций | 8/10 | Логичная воронка, но нет формы заказа и карты |
| Семантика HTML | 7/10 | Хорошая база (header/main/footer/nav/section, ARIA), но есть проблемы с article, div[role=button], tab accessibility |
| Адаптивность | 8.5/10 | Грамотный mobile-first, 4 брейкпоинта, touch support, dvh для меню, prefers-reduced-motion |
| 3D-заглушка | 6/10 | Mount point подготовлен, CSS fallback работает, но нет canvas, нет библиотек, нет lazy-load стратегии |
| Общая архитектура | 6.5/10 | Сильная дизайн-система, но монолитный CSS/JS, inline styles, непоследовательный BEM, дублирование |

### Приоритетный план действий:

1. :red_circle: Добавить форму заказа (имя, телефон, адрес, выбор воды)
2. :red_circle: Вынести JS в отдельные модули
3. :yellow_circle: Исправить семантику: `article` для отзывов, `button` вместо `div[role=button]`, связать tabs/panels через aria-controls
4. :yellow_circle: Убрать inline styles, создать CSS-классы для badge-вариантов
5. :yellow_circle: Добавить `<canvas>` и стратегию lazy-load для 3D
6. :yellow_circle: Исправить несоответствие навигации desktop/mobile
7. :yellow_circle: Добавить `100dvh` для hero секции
8. :green_circle: Вынести SVG в sprite, оптимизировать загрузку шрифтов
9. :green_circle: Консолидировать BEM-нейминг
10. :green_circle: Добавить карту зоны доставки, favicon, canonical URL
