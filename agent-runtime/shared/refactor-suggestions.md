# Отчёт по рефакторингу CSS — Aqua Odin

**Файл:** `style.css` (2273 строки)
**HTML:** `index.html`
**Дата анализа:** 2026-03-19

---

## Сводка

| Категория | Найдено проблем | Оценка экономии (строки) |
|---|---|---|
| Мёртвый код | 6 блоков | ~60 строк |
| Объединение селекторов | 5 групп | ~35 строк |
| Повторяющиеся стили (дублирование) | 4 блока | ~30 строк |
| Упрощение shorthand | 3 блока | ~10 строк |
| Жёстко прописанные значения вместо переменных | 8 мест | ~0 строк (качество) |
| **Итого** | | **~135 строк** |

---

## 1. Мёртвый код — стили без соответствующих элементов в HTML

### 1.1 Стили форм (`.form-input`, `.form-select`, `.form-label`, `.form-group`)

**Строки CSS:** 1886–1945 (60 строк)

В HTML-файле нет ни одного элемента с классами `form-input`, `form-select`, `form-label` или `form-group`. Эти стили определены, но нигде не используются.

**Текущий код (строки 1885–1945):**
```css
/* === Form Styles === */
.form-input {
  font-family: var(--font-body);
  font-size: var(--text-base);
  padding: 12px 16px;
  border: 2px solid var(--color-neutral-200);
  border-radius: var(--radius-md);
  background: var(--color-neutral-100);
  color: var(--color-neutral-900);
  width: 100%;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.form-input:focus { ... }
.form-input:focus-visible { ... }
.form-input::placeholder { ... }
.form-label { ... }
.form-select { ... }
.form-select:focus { ... }
.form-group { margin-bottom: var(--space-4); }
```

**Рекомендация:** Удалить полностью (строки 1885–1945) до момента, пока форма не будет добавлена в HTML. Либо вынести в отдельный файл `forms.css` и подключать только там, где нужно.

**Экономия: ~60 строк**

---

### 1.2 Класс `.wave-divider--flip`

**Строка CSS:** 550–552

```css
.wave-divider--flip {
  transform: scaleY(-1);
}
```

В HTML нет ни одного элемента с классом `wave-divider--flip`.

**Рекомендация:** Удалить.

**Экономия: 3 строки**

---

### 1.3 Класс `.text-center`

**Строки CSS:** 1839–1841

```css
.text-center {
  text-align: center;
}
```

В HTML нет элементов с классом `text-center`. Центрирование везде задаётся через компонентные классы.

**Рекомендация:** Удалить.

**Экономия: 3 строки**

---

### 1.4 Класс `.product-card__old-price`

**Строки CSS:** 1237–1241

```css
.product-card__old-price {
  font-size: var(--text-sm);
  color: var(--color-neutral-300);
  text-decoration: line-through;
}
```

В HTML нет ни одного элемента с классом `product-card__old-price` — зачёркнутые цены не используются ни в одной карточке товара.

**Рекомендация:** Удалить.

**Экономия: 5 строк**

---

## 2. Объединение селекторов через запятую

### 2.1 Блоки `.btn-primary` и `.btn-secondary` — общие свойства

**Строки CSS:** 196–257

`.btn-primary` (строки 196–226) и `.btn-secondary` (строки 228–257) имеют одинаковый набор базовых свойств:

**Текущий код:**
```css
.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  border-radius: var(--radius-sm);
  border: none;
  cursor: pointer;
  text-align: center;
  line-height: 1.4;
  /* ... уникальные свойства ... */
}

.btn-secondary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  text-align: center;
  line-height: 1.4;
  /* ... уникальные свойства ... */
}
```

**Предлагаемый код:**
```css
/* Общие стили для кнопок */
.btn-primary,
.btn-secondary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  border-radius: var(--radius-sm);
  cursor: pointer;
  text-align: center;
  line-height: 1.4;
  border: none;
}

/* Уникальные стили .btn-primary */
.btn-primary {
  background: var(--gradient-cta);
  color: var(--color-white);
  padding: 14px 32px;
  box-shadow: var(--shadow-cta);
  transition: transform 0.2s ease, box-shadow 0.3s ease;
}

/* Уникальные стили .btn-secondary */
.btn-secondary {
  background: transparent;
  color: var(--color-primary-700);
  padding: 12px 30px;
  border: 2px solid var(--color-primary-700);
  transition: background 0.2s ease, color 0.2s ease;
}
```

**Экономия: ~10 строк**

---

### 2.2 Одинаковые `outline` у интерактивных элементов

**Строки CSS:** 254–257, 275–278, 1139–1142, 1376–1379, 1403–1406, 1441–1444

Все интерактивные элементы используют одинаковый стиль фокуса:

**Текущий код (повторяется 6+ раз):**
```css
.btn-secondary:focus {
  outline: 3px solid var(--color-primary-400);
  outline-offset: 2px;
}
.btn-ghost:focus {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
}
.tab-btn:focus {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
}
.slider-btn:focus {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
}
.reviews__dot:focus {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
}
```

**Предлагаемый код:**
```css
/* Глобальный focus-visible для интерактивных элементов */
.btn-ghost:focus-visible,
.tab-btn:focus-visible,
.slider-btn:focus-visible,
.reviews__dot:focus-visible,
.faq-item__question:focus-visible {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
}

/* Усиленный фокус для основной кнопки */
.btn-primary:focus-visible,
.btn-secondary:focus-visible {
  outline: 3px solid var(--color-primary-400);
  outline-offset: 2px;
}
```

**Экономия: ~12 строк**

---

### 2.3 Одинаковые hover-эффекты для карточек

**Строки CSS:** 870–873 и 1173–1176

`.fact-card:hover` и `.product-card:hover` используют одинаковый сдвиг на `-4px`:

**Текущий код:**
```css
/* строки 870-873 */
.fact-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}

/* строки 1173-1176 */
.product-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}
```

Примечание: в секции glow-эффектов (строка 2233–2235) `.product-card:hover` переопределяет `box-shadow`. Поэтому объединять можно только `transform`:

**Предлагаемый код:**
```css
.fact-card:hover,
.product-card:hover {
  transform: translateY(-4px);
}

.fact-card:hover {
  box-shadow: var(--shadow-lg);
}
```

**Экономия: ~3 строки**

---

### 2.4 Иконки с одинаковыми размерами `.fact-card__icon` и `.benefit-card__icon`

**Строки CSS:** 874–883 и 985–995

Оба блока определяют иконку `56×56px` с `border-radius: 50%`, `background: var(--color-accent-100)`, `display: flex`, `align-items: center`, `justify-content: center`.

**Текущий код:**
```css
/* строки 874-883 */
.fact-card__icon {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--color-accent-100);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto var(--space-4);
}

/* строки 985-995 */
.benefit-card__icon {
  width: 56px;
  height: 56px;
  min-width: 56px;
  border-radius: 50%;
  background: var(--color-accent-100);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform var(--transition-spring);
}
```

**Предлагаемый код:**
```css
/* Общие стили иконок карточек */
.fact-card__icon,
.benefit-card__icon {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--color-accent-100);
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Уникальные свойства */
.fact-card__icon {
  margin: 0 auto var(--space-4);
}

.benefit-card__icon {
  min-width: 56px;
  transition: transform var(--transition-spring);
}
```

**Экономия: ~7 строк**

---

### 2.5 Одинаковые `font-size` и `color` у текста карточек

**Строки CSS:** 896–900, 1011–1015, 1078–1082

`.fact-card__text`, `.benefit-card__text` и `.step__text` имеют идентичные свойства:

**Текущий код:**
```css
.fact-card__text {
  font-size: var(--text-sm);
  color: var(--color-neutral-500);
  line-height: 1.6;
}

.benefit-card__text {
  font-size: var(--text-sm);
  color: var(--color-neutral-500);
  line-height: 1.5;
}

.step__text {
  font-size: var(--text-sm);
  color: var(--color-neutral-500);
  line-height: 1.6;
}
```

**Предлагаемый код:**
```css
.fact-card__text,
.benefit-card__text,
.step__text {
  font-size: var(--text-sm);
  color: var(--color-neutral-500);
  line-height: 1.6;
}
/* Если нужно переопределить line-height для .benefit-card__text: */
.benefit-card__text {
  line-height: 1.5;
}
```

**Экономия: ~6 строк**

---

## 3. Повторяющиеся стили / дублирование правил

### 3.1 Дублирование `box-shadow` у `.btn-primary`

**Строки CSS:** 209 и 2196–2201

`.btn-primary` получает `box-shadow` в двух местах — сначала в основном блоке кнопки (строка 209), затем переопределяется в секции Glow Effects (строка 2197). В итоге первая декларация полностью перекрыта.

**Текущий код:**
```css
/* Строка 209 — основной блок */
.btn-primary {
  ...
  box-shadow: var(--shadow-cta);   /* ← ПЕРЕКРЫТО */
  ...
}

/* Строки 2196-2198 — секция Glow Effects */
.btn-primary {
  box-shadow: 0 4px 16px rgba(242, 140, 40, 0.35), 0 0 20px rgba(242, 140, 40, 0.15);
}
.btn-primary:hover {
  box-shadow: 0 6px 24px rgba(242, 140, 40, 0.45), 0 0 30px rgba(242, 140, 40, 0.2);
}
```

**Предлагаемый код:**
```css
/* Строка 209 — убрать box-shadow отсюда, оставить только в Glow Effects */
.btn-primary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background: var(--gradient-cta);
  color: var(--color-white);
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  padding: 14px 32px;
  border-radius: var(--radius-sm);
  border: none;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.3s ease;
  text-align: center;
  line-height: 1.4;
  /* box-shadow задан в секции Glow Effects */
}
```

Также аналогично: `.btn-primary:hover` определяет `box-shadow` на строке 217, который затем переопределяется на строке 2200. Строки 215–218 можно упростить, убрав `box-shadow`.

**Экономия: ~4 строки**

---

### 3.2 Дублирование `box-shadow` у `.whatsapp-widget__btn`

**Строки CSS:** 1762 и 2259–2263

Аналогично кнопке: `.whatsapp-widget__btn` получает `box-shadow` в основном блоке (строка 1762), а затем он полностью переопределяется в секции Glow Effects (строка 2259).

**Текущий код:**
```css
/* Строка 1762 */
.whatsapp-widget__btn {
  ...
  box-shadow: 0 4px 16px rgba(37, 211, 102, 0.35);  /* ← ПЕРЕКРЫТО */
  ...
}

/* Строки 2258-2263 — секция Glow Effects */
.whatsapp-widget__btn {
  box-shadow: 0 4px 16px rgba(37, 211, 102, 0.35), 0 0 24px rgba(37, 211, 102, 0.15);
}
.whatsapp-widget__btn:hover {
  box-shadow: 0 6px 24px rgba(37, 211, 102, 0.45), 0 0 36px rgba(37, 211, 102, 0.2);
}
```

**Рекомендация:** Убрать `box-shadow` из строки 1762 — он полностью перекрывается секцией Glow Effects.

**Экономия: 1 строка**

---

### 3.3 Дублирование `font-size: 2.75rem` для `.section__title` и `.hero__title`

**Строки CSS:** 617–619 и 671–675 и 1873–1876

`.section__title` принимает `font-size: 2.75rem` в медиа-запросе `@media (min-width: 768px)` (строки 616–620). Затем в блоке общих медиа-запросов на строке 1873–1876 повторно задаётся то же самое значение:

**Текущий код:**
```css
/* Строки 616-620 */
@media (min-width: 768px) {
  .section__title {
    font-size: 2.75rem;
  }
}

/* Строки 1868-1876 */
@media (min-width: 1024px) {
  .hero__title {
    font-size: 3.5rem;
  }
  .section__title {        /* ← ДУБЛЬ */
    font-size: 2.75rem;
  }
}
```

`.section__title` получает `2.75rem` уже с `768px`, и переопределение того же значения с `1024px` — избыточно.

**Рекомендация:** Удалить строки 1873–1875 (`.section__title { font-size: 2.75rem; }` внутри `@media (min-width: 1024px)`).

**Экономия: 3 строки**

---

### 3.4 Дублирование правил `.faq-item__question:focus` и `:focus-visible`

**Строки CSS:** 1441–1453

```css
.faq-item__question:focus {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
  color: var(--color-primary-700);
}
.faq-item__question:focus:not(:focus-visible) {
  outline: none;
}
.faq-item__question:focus-visible {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
  color: var(--color-primary-700);
}
```

Паттерн `:focus` + `:focus:not(:focus-visible)` + `:focus-visible` — это устаревший способ поддержки браузеров без `:focus-visible`. Сегодня все актуальные браузеры поддерживают `:focus-visible` нативно.

**Предлагаемый код:**
```css
.faq-item__question:focus-visible {
  outline: 2px solid var(--color-primary-400);
  outline-offset: 2px;
  color: var(--color-primary-700);
}
```

**Экономия: 8 строк**

---

## 4. Упрощение развёрнутых свойств (shorthand)

### 4.1 Свойства `margin` в `.cta-block__subtitle`

**Строки CSS:** 1521–1528**

```css
.cta-block__subtitle {
  font-size: var(--text-lg);
  color: var(--color-primary-200);
  margin-bottom: var(--space-8);
  max-width: 560px;
  margin-left: auto;
  margin-right: auto;
}
```

Три отдельных свойства `margin` можно заменить одним shorthand:

**Предлагаемый код:**
```css
.cta-block__subtitle {
  font-size: var(--text-lg);
  color: var(--color-primary-200);
  max-width: 560px;
  margin: 0 auto var(--space-8);
}
```

**Экономия: 2 строки**

---

### 4.2 Свойства `padding` в `.mobile-menu`

**Строка CSS:** 481**

```css
.mobile-menu {
  ...
  padding: 96px 24px 40px;   /* уже shorthand — ОК */
  ...
}
```

Это уже shorthand — замечаний нет.

---

### 4.3 Свойства `top/left/right/bottom` → `inset`

**Строки CSS:** 282–285 (`.header`), 471–474 (`.mobile-menu`), 1586–1588 (`.cta-block__bubbles`), 1969–1972 (`.product-modal`)

В нескольких местах используется комбинация `top: 0; left: 0; right: 0` или `inset: 0` (уже применяется в некоторых блоках).

**Текущий код:**
```css
/* Строки 282-285 */
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  ...
}
```

**Предлагаемый код:**
```css
.header {
  position: fixed;
  inset: 0 0 auto 0;  /* top:0, right:0, bottom:auto, left:0 */
  ...
}
/* Или более читаемо: */
.header {
  position: fixed;
  top: 0;
  inset-inline: 0;    /* left: 0; right: 0 — логические свойства CSS */
  ...
}
```

Примечание: `inset: 0` уже используется в `.hero__bubbles` (строка 809) и `.product-modal__overlay` (строка 1987) — хорошая практика, стоит применить везде последовательно.

**Экономия: ~4 строки (при применении ко всем 4 блокам)**

---

## 5. Жёстко прописанные значения вместо переменных

Ниже перечислены места, где значения продублированы напрямую вместо использования уже имеющихся CSS-переменных.

### 5.1 Захардкоженные размеры шрифта

| Строка | Текущее значение | Рекомендуемая переменная |
|---|---|---|
| 205 | `font-size: 16px` | `var(--text-base)` |
| 237 | `font-size: 16px` | `var(--text-base)` |
| 313 | `font-size: 1.35rem` | (нет переменной, добавить `--text-logo: 1.35rem`) |
| 497 | `font-size: 1.35rem` | аналогично |
| 2067 | `font-size: 0.75rem` | `var(--text-xs)` |
| 2117 | `font-size: 0.9rem` | (нет переменной, добавить `--text-sm-alt`) |

**Текущий код (пример):**
```css
/* Строка 205 */
.btn-primary {
  font-size: 16px;  /* жёсткое значение */
}

/* Строка 237 */
.btn-secondary {
  font-size: 16px;  /* жёсткое значение */
}
```

**Предлагаемый код:**
```css
.btn-primary {
  font-size: var(--text-base);
}

.btn-secondary {
  font-size: var(--text-base);
}
```

---

### 5.2 Захардкоженный цвет `#083C6E` вместо переменной

**Строка CSS:** 566

```css
.wave-divider--footer svg path {
  fill: #083C6E;   /* равно var(--color-primary-800) */
}
```

**Предлагаемый код:**
```css
.wave-divider--footer svg path {
  fill: var(--color-primary-800);
}
```

---

### 5.3 Значение `border-radius: 20px` вместо переменной

**Строки CSS:** 2046–2047, 2159, 2180

```css
/* Строка 2046 */
.product-modal__image {
  border-radius: 20px 20px 0 0;
}

/* Строка 2159 */
.product-modal__cert-content {
  border-radius: var(--radius-lg, 12px);  /* fallback 12px некорректен: --radius-lg = 20px */
}
```

В `:root` определены: `--radius-lg: 20px`, но на строке 2115 и 2159 в fallback передаётся `12px`:

```css
border-radius: var(--radius-lg, 12px);  /* неверный fallback */
```

**Предлагаемый код:**
```css
border-radius: var(--radius-lg);  /* fallback не нужен, переменная определена в :root */
```

**Строка 2046:**
```css
.product-modal__image {
  border-radius: var(--radius-lg) var(--radius-lg) 0 0;
}
```

---

### 5.4 Значение `gap: 8px` вместо `var(--space-2)`

Встречается в нескольких блоках:

| Строка | Элемент | Текущее | Рекомендуемое |
|---|---|---|---|
| 200 | `.btn-primary` | `gap: 8px` | `gap: var(--space-2)` |
| 232 | `.btn-secondary` | `gap: 8px` | `gap: var(--space-2)` |
| 304 | `.header__inner` | `gap: 16px` | `gap: var(--space-4)` |
| 727–728 | `.hero__trust` | `gap: 8px` | `gap: var(--space-2)` |
| 1305 | `.review-card__stars` | `gap: 4px` | `gap: var(--space-1)` |
| 1387 | `.reviews__dots` | `gap: 8px` | `gap: var(--space-2)` |
| 1565 | `.cta-badge` | `gap: 6px` | (нет переменной — добавить `--space-1-5: 6px`) |
| 1629–1630 | `.footer__logo` | `gap: 8px` | `gap: var(--space-2)` |
| 1671 | `.footer__contact-item` | `gap: 8px` | `gap: var(--space-2)` |

---

## 6. Дополнительные рекомендации

### 6.1 Разбить секцию Glow Effects на компонентные блоки

**Строки CSS:** 2191–2273

Секция с glow-эффектами (82 строки) добавляет `box-shadow` и `text-shadow` к элементам, которые уже определены выше. Это создаёт скрытые переопределения (см. п. 3.1 и 3.2). Рекомендуется объединить glow-свойства с основными блоками компонентов:

- Убрать блок `.btn-primary { box-shadow: ... }` из Glow Effects, перенести в основной блок `.btn-primary` (строка 196)
- Аналогично для `.whatsapp-widget__btn`

### 6.2 Объединить дублирующиеся `@media (min-width: 768px)` блоки

В файле есть несколько разрозненных медиа-запросов для одного и того же брейкпоинта `768px`:
- Строки 189–193 (`.container`)
- Строки 569–573 (`.wave-divider svg`)
- Строки 585–589 (`.section`)
- Строки 616–620 (`.section__title`)
- Строки 643–649 (`.hero__inner`)
- ... и ещё ~12 блоков

Их объединение в единый `@media (min-width: 768px) { ... }` блок улучшит читаемость, хотя и не сократит количество строк кода.

---

## Итоговый план рефакторинга (по приоритету)

| Приоритет | Действие | Экономия строк | Риск |
|---|---|---|---|
| 1 (высокий) | Удалить мёртвый код форм (строки 1885–1945) | ~60 | Нулевой |
| 2 (высокий) | Удалить `.wave-divider--flip`, `.text-center`, `.product-card__old-price` | ~11 | Нулевой |
| 3 (высокий) | Убрать дубль `box-shadow` из строк 209 и 1762 (уже переопределён в Glow Effects) | ~2 | Нулевой |
| 4 (высокий) | Удалить дубль `.section__title` в `@media 1024px` (строки 1873–1875) | ~3 | Нулевой |
| 5 (средний) | Объединить `:focus` в единый блок (строки 1441–1453 → 4 строки) | ~8 | Низкий |
| 6 (средний) | Объединить общие свойства `.btn-primary` / `.btn-secondary` | ~10 | Низкий |
| 7 (средний) | Объединить `.fact-card__icon` / `.benefit-card__icon` | ~7 | Низкий |
| 8 (средний) | Объединить `*__text` классов карточек | ~6 | Низкий |
| 9 (низкий) | Заменить `gap: 8px` → `var(--space-2)` везде | 0 (качество) | Нулевой |
| 10 (низкий) | Заменить `#083C6E` → `var(--color-primary-800)` | 0 (качество) | Нулевой |
| 11 (низкий) | Shorthand `margin` в `.cta-block__subtitle` | ~2 | Нулевой |
| 12 (низкий) | Убрать некорректный fallback в `var(--radius-lg, 12px)` | 0 (качество) | Нулевой |
| **Итого** | | **~109 строк** | |
