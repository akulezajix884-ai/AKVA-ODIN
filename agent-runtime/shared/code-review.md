# Code Review — Aqua Odin Water Delivery Website

**Проверено:** `outputs/index.html`, `outputs/style.css`
**Дата проверки:** 2026-03-19
**Ревьюер:** Claude Code (claude-sonnet-4-6)

---

## 1. ВАЛИДНОСТЬ HTML

---

**🟡 index.html:7 — Устаревший мета-тег keywords**
  Проблема: `<meta name="keywords">` игнорируется Google и другими поисковиками с 2009 года. Не несёт пользы, добавляет лишний HTML.
  Решение: Удалить тег полностью — он не влияет на SEO и загромождает `<head>`.

---

**🟡 index.html:12 — Отсутствует og:image в Open Graph разметке**
  Проблема: Есть `og:title`, `og:description`, `og:type`, `og:locale`, но нет `og:image`. При шаринге в соцсетях карточка будет без изображения.
  Решение: Добавить `<meta property="og:image" content="https://aquaodin.ru/og-image.jpg">` и `og:url`.

---

**🟡 index.html:18-20 — Нет `rel="preload"` для критического CSS**
  Проблема: Google Fonts загружается через стандартный `<link>`, что блокирует рендер. Есть `preconnect`, но нет `preload` для самого файла шрифтов.
  Решение: Добавить `<link rel="preload" as="style" href="style.css">` и рассмотреть `font-display: swap` (уже применяется через параметр `&display=swap` в Google Fonts URL — это плюс).

---

**🟡 index.html:46-50 — Несоответствие ссылок в навигации**
  Проблема: Десктопная навигация ведёт `#faq` с текстом "Контакты", но секции "Контакты" нет — есть только `#faq`. Мобильное меню ведёт правильно на `#reviews` и `#faq`, но в десктопной навигации `#reviews` отсутствует.
  Решение: Синхронизировать десктопную и мобильную навигацию. Переименовать ссылку "Контакты" в "FAQ" или создать отдельный якорь для контактов в футере.

---

**🟡 index.html:133 — Некорректный id начинается с цифры**
  Проблема: `id="3d-hero-placeholder"` начинается с цифры. Хотя HTML5 это разрешает, `document.getElementById('3d-hero-placeholder')` работает корректно, но CSS-селектор `#3d-hero-placeholder` без escaping в некоторых контекстах может вызвать проблемы (например, при использовании в CSS или querySelector).
  Решение: Переименовать в `id="hero-3d-placeholder"`.

---

**🟡 index.html:142,204,284,344,670,761,831 — Инлайновые стили в волновых разделителях**
  Проблема: Повторяющийся паттерн `style="position: absolute; bottom: 0; left: 0; right: 0;"` и `style="margin-top: var(--space-10);"` задан инлайново у 7+ элементов `.wave-divider`.
  Решение: Добавить классы-модификаторы (`wave-divider--hero`, `wave-divider--section`) и вынести стили в CSS.

---

**🟢 index.html:360-363 — Таб-список без `aria-label`**
  Проблема: `<div class="products__tabs" role="tablist">` не имеет `aria-label`, что затрудняет навигацию для screen reader — непонятно, что это за группа табов.
  Решение: Добавить `aria-label="Категории продуктов"` на элемент `role="tablist"`.

---

**🟡 index.html:367,572,611 — `role="tabpanel"` без `aria-labelledby`**
  Проблема: Панели табов не связаны с кнопками-переключателями через `aria-labelledby`. Скринридер не сможет объявить, какая вкладка активна.
  Решение: Добавить `id` к каждому `tab-btn` и `aria-labelledby="tab-water-btn"` к соответствующим `tab-panel`.

---

**🔴 index.html:779,789,799,809,819 — FAQ-кнопки без `aria-controls`**
  Проблема: Каждая кнопка аккордеона `.faq-item__question` имеет `aria-expanded`, но не связана с раскрывающейся областью через `aria-controls`. Элемент `.faq-item__answer` не имеет `id`. Screen reader не понимает, каким контентом управляет кнопка.
  Решение:
  ```html
  <button class="faq-item__question" aria-expanded="false" aria-controls="faq-answer-1">
  <div class="faq-item__answer" id="faq-answer-1" role="region" aria-labelledby="faq-btn-1">
  ```

---

**🟡 index.html:965 — Модальное окно без `role="dialog"` и `aria-modal`**
  Проблема: `<div class="product-modal" id="productModal" aria-hidden="true">` не имеет `role="dialog"` и `aria-modal="true"`. Screen reader не понимает, что это модальное окно, и не ограничивает фокус внутри него.
  Решение: Добавить `role="dialog"` и `aria-modal="true"`. Реализовать focus trap при открытии модалки.

---

**🔴 index.html:965 — Отсутствует focus trap в модальном окне**
  Проблема: При открытии модалки фокус не перемещается внутрь и не ограничивается — пользователь клавиатуры может перейти на фоновый контент.
  Решение: При открытии модалки сохранять предыдущий активный элемент, перемещать фокус на первый интерактивный элемент внутри модалки, блокировать Tab за её пределами. При закрытии — возвращать фокус.

---

**🟡 index.html:370,390,409 — `role="button"` на `<div>` вместо нативного `<button>` или `<a>`**
  Проблема: Карточки товаров реализованы как `<div role="button" tabindex="0">`. Это работает, но нативный `<button>` лучше: он автоматически активируется по Enter/Space, доступен из коробки, не требует `tabindex`.
  Решение: Обернуть карточку в `<article>` для семантики или использовать нативный `<button>` для интерактивной части.

---

## 2. ДОСТУПНОСТЬ (A11Y)

---

**🔴 style.css:143-146 — Глобальный сброс `text-decoration: none` для ссылок**
  Проблема: `a { text-decoration: none; }` убирает подчёркивание со ВСЕХ ссылок. Подчёркивание — ключевой визуальный сигнал доступности для ссылок в тексте (WCAG 1.4.1).
  Решение: Оставить глобальный сброс только для навигационных ссылок через более специфичный селектор:
  ```css
  .header__nav a, .footer__links a, .mobile-menu__nav a { text-decoration: none; }
  ```
  Инлайновые ссылки (например, в `#faq .section__subtitle`) должны быть подчёркнуты.

---

**🟡 index.html:89-96 — Ссылки в мобильном меню без `aria-label`**
  Проблема: Телефонные ссылки `<a href="tel:...">` в мобильном меню содержат только SVG-иконку и текст. Это нормально, но SVG-иконки не имеют `aria-hidden="true"`, и скринридер может дважды читать иконку и текст.
  Решение: Добавить `aria-hidden="true"` ко всем декоративным SVG внутри ссылок/кнопок.

---

**🟡 Контрастность — `--color-neutral-500` (#6B6B80) на белом фоне**
  Проблема: Цвет `#6B6B80` на `#FFFFFF` даёт соотношение контраста ~4.3:1. Это проходит WCAG AA для обычного текста, но находится на грани. Используется для `.fact-card__text`, `.benefit-card__text`, `.step__text` — мелкий текст (0.875rem / 14px), для которого WCAG AA требует 4.5:1 для текста меньше 18px/14px bold.
  Решение: Использовать `--color-neutral-700` (#3D3D56) для body-текста на белом фоне, оставив `--color-neutral-500` только для второстепенных подписей.

---

**🟡 index.html:125,852 — Инлайновые стили изменяют `opacity` текста**
  Проблема: `style="opacity: 0.7;"` применён к параграфу с текстом "Доставка в день заказа. Без обязательств." Исходный цвет `--color-primary-200` (#A3D0F2) на тёмном фоне при opacity 0.7 может не пройти WCAG AA.
  Решение: Убрать инлайновый `opacity`, заменить на более тёмный цвет переменной или создать CSS-класс `.hero__fine-print`.

---

**🟢 index.html:108-115 — Декоративные пузырьки без `aria-hidden`**
  Проблема: `<div class="hero__bubbles">` с дочерними `<div class="bubble">` — чисто декоративные элементы, но не скрыты от скринридеров.
  Решение: Добавить `aria-hidden="true"` на `.hero__bubbles` и `.cta-block__bubbles`.

---

**🟡 index.html:127 — SVG-иконка в `.hero__trust` без `aria-hidden`**
  Проблема: Декоративная иконка щита внутри блока доверия не помечена `aria-hidden="true"`. Скринридер попытается её озвучить.
  Решение: Добавить `aria-hidden="true"` ко всем декоративным SVG-иконкам (в `fact-card__icon`, `benefit-card__icon`, `step__icon`, `cta-badge` и т.д.).

---

**🟢 index.html:686 — Слайдер отзывов без `aria-live`**
  Проблема: При автоматическом переключении слайдов пользователь скринридера не получает уведомление об изменении контента. `aria-live` отсутствует.
  Решение: Добавить `aria-live="polite"` на `.reviews__slider` и убедиться, что автоплей останавливается при `prefers-reduced-motion`.

---

**🔴 style.css:1948-1959 — `prefers-reduced-motion` выключает анимации, но не autoplay слайдера**
  Проблема: CSS-правило `prefers-reduced-motion` корректно отключает CSS-анимации и переходы. Однако JS-автоплей слайдера (`setInterval(nextSlide, 5000)`) не проверяет `prefers-reduced-motion` и продолжает работать — для пользователей с вестибулярными расстройствами это проблема.
  Решение: В JS добавить проверку:
  ```js
  if (!window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    startAutoplay();
  }
  ```

---

## 3. ПРОИЗВОДИТЕЛЬНОСТЬ CSS

---

**🟡 style.css:2196-2234 — Дублирование `box-shadow` для `.btn-primary`**
  Проблема: `box-shadow` для `.btn-primary` объявлен дважды: в базовых стилях кнопки (строка ~209) и в секции "Glow Effects" (строки 2196-2201). Второе объявление полностью перезаписывает первое. Это создаёт путаницу и лишнюю специфичность.
  Решение: Оставить одно объявление `box-shadow` в базовых стилях `.btn-primary`. Секцию "Glow Effects" объединить с основными стилями компонента.

---

**🟡 style.css:780-804 — Анимация `floatUp` вызывает layout thrashing**
  Проблема: Анимация пузырьков:
  ```css
  @keyframes floatUp {
    0% { transform: translateY(100vh) scale(0); opacity: 0; }
    100% { transform: translateY(-100px) scale(1); opacity: 0; }
  }
  ```
  Использует только `transform` и `opacity` — это правильно. Однако `will-change` не задан для `.bubble`, что означает браузер не создаёт отдельный compositing layer заранее.
  Решение: Добавить `will-change: transform, opacity` к `.bubble`. Убрать его после окончания анимации через JS если нужно оптимизировать память.

---

**🟡 style.css:211 — Разные `transition-duration` для `transform` и `box-shadow` в `.btn-primary`**
  Проблема: `transition: transform 0.2s ease, box-shadow 0.3s ease;` — неодинаковые длительности создают визуально нескоординированный эффект.
  Решение: Унифицировать: `transition: transform var(--transition-base), box-shadow var(--transition-base);`

---

**🟢 style.css:1127 — `transition: all` на `.tab-btn`**
  Проблема: `transition: all var(--transition-base)` перехватывает ALL CSS-свойства, включая те, что браузер не умеет анимировать. Это дорого для производительности при наличии многих элементов.
  Решение: Заменить на конкретные свойства: `transition: background var(--transition-base), border-color var(--transition-base), color var(--transition-base);`

---

**🟢 style.css:1121 — `-webkit-overflow-scrolling: touch` устарел**
  Проблема: `products__tabs` использует `-webkit-overflow-scrolling: touch`. Это свойство устарело и удалено из Safari 13+. Все современные iOS-браузеры инерционный скролл включают по умолчанию.
  Решение: Удалить `webkit-overflow-scrolling: touch`. Вместо него можно добавить `overscroll-behavior-x: contain` для лучшего UX.

---

**🟡 style.css:1465-1473 — `max-height` анимация для аккордеона FAQ неоптимальна**
  Проблема: Анимация `max-height: 0` → `max-height: 600px` создаёт layout recalculation на каждый кадр, так как браузер не знает финального размера. Это один из самых дорогих CSS-переходов.
  Решение: Использовать современный подход с CSS Grid:
  ```css
  .faq-item__answer { display: grid; grid-template-rows: 0fr; transition: grid-template-rows 0.35s ease; }
  .faq-item__answer > * { overflow: hidden; }
  .faq-item.open .faq-item__answer { grid-template-rows: 1fr; }
  ```
  Или использовать Web Animations API в JS для анимации реального `height`.

---

**🟡 style.css:1579-1582 — `pulse` анимация на CTA-кнопке постоянно активна**
  Проблема: `animation: pulse 3s ease-in-out infinite` на `.cta-block__btn` постоянно запускается. `scale()` анимация хорошо для GPU, но бесконечный `pulse` мешает фокусному восприятию и может отвлекать пользователей с когнитивными нарушениями (WCAG 2.2.2).
  Решение: Добавить паузу между итерациями или вовсе убрать бесконечный пульс, заменив на однократную анимацию при появлении.

---

**🟢 style.css:1808-1831 — `will-change` не задан для `.animate-on-scroll`**
  Проблема: Элементы `.animate-on-scroll` анимируются по `opacity` и `transform`, но `will-change` не задан. При большом количестве одновременно анимирующихся элементов браузер может не успеть создать compositing layers.
  Решение: Добавить `will-change: opacity, transform` к `.animate-on-scroll` и убирать его после срабатывания анимации (`observer.unobserve`):
  ```css
  .animate-on-scroll { will-change: opacity, transform; }
  .animate-on-scroll.visible { will-change: auto; }
  ```

---

## 4. ДУБЛИРОВАНИЕ КОДА

---

**🟡 style.css:196-278 — Дублирование базовых стилей `.btn-primary` и `.btn-secondary`**
  Проблема: Оба класса кнопок повторяют: `display: inline-flex`, `align-items: center`, `justify-content: center`, `font-family: var(--font-heading)`, `font-weight: 600`, `font-size: 16px`, `border-radius: var(--radius-sm)`, `cursor: pointer`, `text-align: center`, `line-height: 1.4`.
  Решение: Выделить общий базовый класс:
  ```css
  .btn-base {
    display: inline-flex; align-items: center; justify-content: center;
    font-family: var(--font-heading); font-weight: 600; font-size: 16px;
    border-radius: var(--radius-sm); cursor: pointer; text-align: center; line-height: 1.4;
  }
  ```
  `.btn-primary` и `.btn-secondary` наследуют от него.

---

**🟡 style.css — SVG телефонного пути повторяется 5 раз в HTML**
  Проблема: Идентичный SVG-путь для иконки телефона (`<path d="M22 16.92v3 ..."`) продублирован в: хедере (десктоп), хедере (мобильный), мобильном меню (дважды), футере. Это ~500 байт лишнего HTML.
  Решение: Использовать `<symbol>` в SVG-спрайте в начале `<body>` и подключать иконки через `<use href="#icon-phone">`.

---

**🟡 style.css:875-900, 985-1015 — Дублирование стилей иконок карточек**
  Проблема: `.fact-card__icon` и `.benefit-card__icon` имеют идентичные стили:
  - `width: 56px; height: 56px; border-radius: 50%; background: var(--color-accent-100); display: flex; align-items: center; justify-content: center;`
  Аналогично `.fact-card__title` и `.benefit-card__title`, `.fact-card__text` и `.benefit-card__text`.
  Решение: Создать утилитарный класс `.card-icon` или общий компонентный класс `.feature-card` с вариантами.

---

**🟡 style.css — Magic numbers вместо переменных**
  Проблема: В коде встречаются магические числа, не привязанные к design system:
  - `1.35rem` (font-size в `.header__logo` и `.mobile-menu__nav a`) — нет переменной
  - `1.1rem`, `1.15rem`, `1.125rem` — похожие размеры без системы
  - `72px` (высота хедера) — продублирована в CSS (`.header { height: 72px }`, `.hero { padding-top: 72px }`) и в JS (`headerHeight = header.offsetHeight`)
  Решение: Добавить переменную `--header-height: 72px` в `:root` и использовать её везде.

---

**🟢 style.css:2067 — Хардкод `#fff` вместо переменной**
  Проблема: `.product-modal__badge { color: #fff; }` использует хардкод вместо `var(--color-white)`. Таких мест в файле несколько (строки 2069, 2137).
  Решение: Заменить все `#fff` на `var(--color-white)` для консистентности.

---

**🟡 style.css:1997, 2115, 2159 — Дублирование fallback значений для CSS-переменных**
  Проблема: Встречается `var(--radius-xl, 20px)` и `var(--radius-lg, 12px)` — fallback не нужен, так как переменные определены в `:root`. Это лишний код.
  Решение: Использовать просто `var(--radius-xl)` и `var(--radius-lg)`.

---

**🟡 index.html — Дублирование волновых SVG-паттернов**
  Проблема: Одинаковый path `"M0,40 C360,100 720,0 1080,60 C1260,80 1380,20 1440,40 L1440,100 L0,100 Z"` встречается дважды (hero wave и faq→cta wave). Семь волновых блоков имеют практически идентичную структуру.
  Решение: Вынести SVG-волны в шаблонные блоки или использовать CSS-маски/clip-path.

---

## 5. КРОССБРАУЗЕРНОСТЬ

---

**🟡 style.css:294-296 — `backdrop-filter` без полного fallback**
  Проблема: `.header.scrolled` использует `backdrop-filter: blur(12px)` с корректным `-webkit-backdrop-filter`. Однако Firefox до версии 103 не поддерживал `backdrop-filter`. Без fallback хедер будет выглядеть прозрачным в старых Firefox.
  Решение: Добавить fallback-фон с непрозрачностью:
  ```css
  background: rgba(255, 255, 255, 0.97); /* fallback */
  @supports (backdrop-filter: blur(12px)) {
    background: rgba(255, 255, 255, 0.85);
    backdrop-filter: blur(12px);
  }
  ```

---

**🟡 style.css:477 — `height: 100dvh` без fallback**
  Проблема: `.mobile-menu { height: 100dvh; }` — `dvh` единицы не поддерживаются в Safari до 15.4 и Chrome до 108. Объявление `height: 100vh` стоит перед ним (строка 476), но при cascading некоторые парсеры могут игнорировать оба значения.
  Решение: Оставить оба объявления — это правильный прогрессивный подход. Убедиться, что `height: 100vh` действительно идёт первым:
  ```css
  height: 100vh; /* fallback */
  height: 100dvh; /* modern */
  ```
  Текущая реализация это делает корректно — это скорее информационная заметка.

---

**🟡 style.css:1196-1205 — `aspect-ratio` без fallback**
  Проблема: `.product-card__image { aspect-ratio: 1 / 1; }` не поддерживается в Safari до 15.0 и Chrome до 88. В старых браузерах блок схлопнется до нулевой высоты.
  Решение: Добавить fallback через padding trick:
  ```css
  .product-card__image {
    /* fallback для старых браузеров */
    padding-top: 100%;
    position: relative;
    height: 0;
  }
  @supports (aspect-ratio: 1) {
    .product-card__image {
      padding-top: 0;
      height: auto;
      aspect-ratio: 1 / 1;
    }
  }
  ```

---

**🟢 style.css:1931 — `appearance: none` без `-webkit-` префикса**
  Проблема: `.form-select { appearance: none; }` — без `-webkit-appearance: none` не будет работать в старых WebKit-браузерах (Safari < 15, старые Chrome/Edge).
  Решение: Добавить:
  ```css
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  ```

---

**🟢 style.css:1990 — `inset: 0` без fallback**
  Проблема: `inset: 0` — сокращение для `top/right/bottom/left: 0`. Не поддерживается в Safari до 14.1, Firefox до 87. Встречается в `.product-modal` и `.hero__bubbles`.
  Решение: Добавить явный fallback:
  ```css
  top: 0; right: 0; bottom: 0; left: 0; /* fallback */
  inset: 0; /* modern */
  ```
  Или заменить на `top: 0; right: 0; bottom: 0; left: 0;` для лучшей совместимости.

---

**🟢 style.css — Отсутствует `-webkit-` префикс для `transition` в некоторых местах**
  Проблема: В 2026 году `-webkit-transition` не нужен — все современные браузеры поддерживают стандартный `transition`. Это не проблема, просто подтверждение корректности.
  Решение: Ничего менять не нужно. CSS Custom Properties (переменные) не требуют префиксов.

---

## ИТОГОВАЯ ТАБЛИЦА

| Тяжесть | Кол-во | Категория |
|---------|--------|-----------|
| 🔴 critical | 3 | Доступность (focus trap, aria-controls для FAQ, prefers-reduced-motion в JS) |
| 🟡 warning | 20 | HTML-валидность, A11Y, производительность CSS, дублирование, кроссбраузерность |
| 🟢 suggestion | 8 | Улучшения кода, оптимизации, косметические правки |
| **ИТОГО** | **31** | |

### Распределение по категориям

| Категория | 🔴 | 🟡 | 🟢 |
|-----------|----|----|-----|
| HTML-валидность | 0 | 5 | 1 |
| Доступность (A11Y) | 3 | 4 | 3 |
| Производительность CSS | 0 | 4 | 3 |
| Дублирование кода | 0 | 5 | 1 |
| Кроссбраузерность | 0 | 2 | 2 |

---

### Приоритеты для немедленного исправления

1. **🔴 Focus trap в модальном окне** — без него клавиатурные и AT-пользователи не могут нормально работать с продуктовым модалом.
2. **🔴 `aria-controls` для FAQ-аккордеона** — screen readers не понимают связь между кнопкой и раскрывающимся контентом.
3. **🔴 `prefers-reduced-motion` в JS для автоплея** — нарушение WCAG 2.2.2 для пользователей с вестибулярными расстройствами.
4. **🟡 Контрастность `--color-neutral-500`** — мелкий серый текст на белом фоне может не проходить WCAG AA.
5. **🟡 Глобальный сброс `text-decoration: none`** — нарушение WCAG 1.4.1 для инлайновых ссылок.

### Что сделано хорошо

- Skip-link реализован корректно (строка 28).
- `aria-label` есть на logo, бургер-меню, кнопках слайдера, соцсетях, WhatsApp-виджете.
- `prefers-reduced-motion` реализован в CSS.
- JSON-LD разметка (`LocalBusiness`) присутствует и корректна.
- `preconnect` для Google Fonts настроен правильно.
- Passive event listeners на scroll-хендлерах.
- `rel="noopener noreferrer"` на внешних ссылках.
- Фокус-стили (`:focus`, `:focus-visible`) реализованы для всех интерактивных элементов.
- Мобильное меню управляет `aria-hidden` и `aria-expanded` корректно.
- CSS Custom Properties используются системно и охватывают цвета, типографику, отступы.
