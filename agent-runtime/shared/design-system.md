# Дизайн-система АкваВита

Версия: 1.0
Дата: 2026-03-19

---

## 1. Цветовая палитра

### Primary (глубокий синий)

| Токен                  | HEX       | Применение                              |
|------------------------|-----------|-----------------------------------------|
| --color-primary-900    | #062D54   | Фон footer, тёмные акценты             |
| --color-primary-800    | #083C6E   | Hover-состояния на тёмном фоне          |
| --color-primary-700    | #0A4D8C   | **Основной бренд-цвет**, header, кнопки |
| --color-primary-600    | #1060A6   | Ссылки, иконки                          |
| --color-primary-500    | #1A75C4   | Акцентные элементы                      |
| --color-primary-400    | #3D93DB   | Бордеры, лёгкие акценты                |
| --color-primary-300    | #6BB0E8   | Фон карточек при hover                  |
| --color-primary-200    | #A3D0F2   | Подложки, бейджи                        |
| --color-primary-100    | #D1E8F9   | Фон секций, светлые блоки              |
| --color-primary-50     | #EBF4FC   | Фон страницы, подложки форм            |

### Secondary (ледяной голубой)

| Токен                    | HEX       | Применение                       |
|--------------------------|-----------|----------------------------------|
| --color-secondary-500    | #7DD3E8   | Декоративные элементы            |
| --color-secondary-300    | #B0E6F2   | Волнистые разделители, SVG-фоны  |
| --color-secondary-100    | #E0F5FA   | Фон чередующихся секций          |
| --color-secondary-50     | #F0FAFE   | Лёгкие подложки                  |

### Accent (бирюзовый / аквамарин)

| Токен                | HEX       | Применение                            |
|----------------------|-----------|---------------------------------------|
| --color-accent-600   | #0D8F7F   | Hover для бирюзовых кнопок            |
| --color-accent-500   | #11B8A4   | Бейджи "Хит", индикаторы, чек-марки  |
| --color-accent-400   | #3DD4C0   | Иконки преимуществ                    |
| --color-accent-200   | #A1EDDF   | Фон тегов                             |
| --color-accent-100   | #D4F7F0   | Подложки                              |

### CTA (оранжевый)

| Токен             | HEX       | Применение                          |
|-------------------|-----------|-------------------------------------|
| --color-cta-700   | #C45A08   | Active-состояние кнопок             |
| --color-cta-600   | #E06A0A   | Hover-состояние CTA                 |
| --color-cta-500   | #F28C28   | **Главные CTA-кнопки**, акценты    |
| --color-cta-400   | #F5A855   | Лёгкие акценты, бордеры            |
| --color-cta-100   | #FDE8CC   | Фон уведомлений, бейджи акций      |

### Neutral

| Токен                 | HEX       | Применение                    |
|-----------------------|-----------|-------------------------------|
| --color-neutral-900   | #1A1A2E   | Основной текст                |
| --color-neutral-700   | #3D3D56   | Подзаголовки                  |
| --color-neutral-500   | #6B6B80   | Вторичный текст, подписи      |
| --color-neutral-300   | #B0B0C0   | Плейсхолдеры, disabled-текст  |
| --color-neutral-200   | #D4D4DE   | Бордеры, разделители          |
| --color-neutral-100   | #EDEDF2   | Фон input-полей               |
| --color-neutral-50    | #F7F7FA   | Фон страницы (альтернативный) |
| --color-white         | #FFFFFF   | Белый фон, текст на тёмном   |

### Gradient (водные градиенты)

| Токен                       | Значение                                          | Применение              |
|-----------------------------|---------------------------------------------------|-------------------------|
| --gradient-hero             | linear-gradient(135deg, #062D54 0%, #0A4D8C 50%, #1A75C4 100%) | Hero-секция         |
| --gradient-water            | linear-gradient(180deg, #E0F5FA 0%, #D1E8F9 100%) | Фон водных секций       |
| --gradient-cta              | linear-gradient(135deg, #F28C28 0%, #E06A0A 100%) | CTA-кнопки              |
| --gradient-card-hover       | linear-gradient(180deg, #EBF4FC 0%, #E0F5FA 100%) | Карточки при hover       |
| --gradient-footer           | linear-gradient(180deg, #083C6E 0%, #062D54 100%) | Footer                  |

### Тени

| Токен              | Значение                                      | Применение              |
|--------------------|-----------------------------------------------|-------------------------|
| --shadow-sm        | 0 1px 3px rgba(10, 77, 140, 0.08)             | Кнопки, малые элементы  |
| --shadow-md        | 0 4px 12px rgba(10, 77, 140, 0.12)            | Карточки                |
| --shadow-lg        | 0 8px 30px rgba(10, 77, 140, 0.16)            | Модальные окна, hover   |
| --shadow-cta       | 0 4px 16px rgba(242, 140, 40, 0.35)           | CTA-кнопки              |

---

## 2. Типографика

### Шрифты

| Токен              | Шрифт                       | Применение                 |
|--------------------|------------------------------|----------------------------|
| --font-heading     | 'Montserrat', sans-serif     | Заголовки H1-H6, логотип  |
| --font-body        | 'Inter', sans-serif          | Основной текст, UI         |
| --font-mono        | 'JetBrains Mono', monospace  | Цены, технические данные   |

Google Fonts подключение:

```
https://fonts.googleapis.com/css2?family=Montserrat:wght@500;600;700;800&family=Inter:wght@400;500;600&display=swap
```

### Масштаб заголовков

| Элемент | Desktop          | Mobile           | Weight | Line-height | Letter-spacing |
|---------|------------------|------------------|--------|-------------|----------------|
| H1      | 56px / 3.5rem    | 36px / 2.25rem   | 800    | 1.1         | -0.02em        |
| H2      | 44px / 2.75rem   | 28px / 1.75rem   | 700    | 1.15        | -0.015em       |
| H3      | 32px / 2rem      | 24px / 1.5rem    | 700    | 1.2         | -0.01em        |
| H4      | 24px / 1.5rem    | 20px / 1.25rem   | 600    | 1.3         | 0              |
| H5      | 20px / 1.25rem   | 18px / 1.125rem  | 600    | 1.3         | 0              |
| H6      | 16px / 1rem      | 16px / 1rem      | 600    | 1.4         | 0.01em         |

### Масштаб текста

| Токен           | Размер         | Weight | Line-height | Применение                 |
|-----------------|----------------|--------|-------------|----------------------------|
| --text-xl       | 20px / 1.25rem | 400    | 1.6         | Лид-абзацы, hero-подтекст  |
| --text-lg       | 18px / 1.125rem| 400    | 1.6         | Увеличенный текст           |
| --text-base     | 16px / 1rem    | 400    | 1.6         | Основной текст              |
| --text-sm       | 14px / 0.875rem| 400    | 1.5         | Подписи, мета-информация   |
| --text-xs       | 12px / 0.75rem | 500    | 1.4         | Бейджи, лейблы             |

---

## 3. Сетка и отступы

### Сетка

| Параметр            | Значение     |
|---------------------|--------------|
| Колонок             | 12           |
| Контейнер max-width | 1200px       |
| Gutter              | 24px         |
| Gutter mobile       | 16px         |
| Padding контейнера  | 0 24px       |
| Padding mobile      | 0 16px       |

### Breakpoints

| Токен         | Значение | Описание            |
|---------------|----------|---------------------|
| --bp-mobile   | 480px    | Малые телефоны      |
| --bp-tablet   | 768px    | Планшеты            |
| --bp-desktop  | 1024px   | Десктоп             |
| --bp-wide     | 1280px   | Широкие экраны      |

### Spacing scale

| Токен            | Значение | Применение                     |
|------------------|----------|--------------------------------|
| --space-1        | 4px      | Минимальный зазор              |
| --space-2        | 8px      | Между иконкой и текстом        |
| --space-3        | 12px     | Внутренние отступы мелких блоков|
| --space-4        | 16px     | Padding кнопок, карточек       |
| --space-6        | 24px     | Gutter сетки, межстрочные      |
| --space-8        | 32px     | Отступы между элементами       |
| --space-10       | 40px     | Разделение групп элементов     |
| --space-12       | 48px     | Padding секций (mobile)        |
| --space-16       | 64px     | Padding секций (tablet)        |
| --space-20       | 80px     | Padding секций (desktop)       |
| --space-24       | 96px     | Большие вертикальные отступы   |

### Border radius

| Токен               | Значение | Применение            |
|---------------------|----------|-----------------------|
| --radius-sm         | 6px      | Кнопки, бейджи        |
| --radius-md         | 12px     | Карточки, input        |
| --radius-lg         | 20px     | Крупные карточки       |
| --radius-xl         | 32px     | Декоративные блоки     |
| --radius-full       | 9999px   | Пилюли, аватары        |

---

## 4. Компоненты

### 4.1 Кнопки

**Primary (CTA)**

```css
.btn-primary {
  background: var(--gradient-cta);
  color: var(--color-white);
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  padding: 14px 32px;
  border-radius: var(--radius-sm);
  border: none;
  box-shadow: var(--shadow-cta);
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.3s ease;
}
.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(242, 140, 40, 0.45);
}
.btn-primary:active {
  transform: translateY(0);
}
```

**Secondary**

```css
.btn-secondary {
  background: transparent;
  color: var(--color-primary-700);
  font-family: var(--font-heading);
  font-weight: 600;
  font-size: 16px;
  padding: 12px 30px;
  border-radius: var(--radius-sm);
  border: 2px solid var(--color-primary-700);
  cursor: pointer;
  transition: background 0.2s ease, color 0.2s ease;
}
.btn-secondary:hover {
  background: var(--color-primary-700);
  color: var(--color-white);
}
```

**Ghost**

```css
.btn-ghost {
  background: transparent;
  color: var(--color-primary-600);
  font-family: var(--font-body);
  font-weight: 500;
  font-size: 14px;
  padding: 8px 16px;
  border: none;
  cursor: pointer;
  text-decoration: underline;
  text-underline-offset: 3px;
  transition: color 0.2s ease;
}
.btn-ghost:hover {
  color: var(--color-primary-800);
}
```

### 4.2 Карточки товаров

```css
.product-card {
  background: var(--color-white);
  border-radius: var(--radius-md);
  padding: var(--space-6);
  box-shadow: var(--shadow-md);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  position: relative;
  overflow: hidden;
}
.product-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
}
.product-card__badge {
  position: absolute;
  top: 12px;
  left: 12px;
  background: var(--color-accent-500);
  color: var(--color-white);
  font-size: var(--text-xs);
  font-weight: 600;
  padding: 4px 10px;
  border-radius: var(--radius-full);
}
.product-card__image {
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: contain;
  margin-bottom: var(--space-4);
}
.product-card__title {
  font-family: var(--font-heading);
  font-size: 18px;
  font-weight: 600;
  color: var(--color-neutral-900);
  margin-bottom: var(--space-2);
}
.product-card__price {
  font-family: var(--font-mono);
  font-size: 24px;
  font-weight: 700;
  color: var(--color-primary-700);
  margin-bottom: var(--space-4);
}
.product-card__action {
  /* Использует .btn-primary */
  width: 100%;
}
```

### 4.3 Волнистые разделители (SVG waves)

Между секциями используются SVG-волны для создания плавных переходов.

```html
<div class="wave-divider">
  <svg viewBox="0 0 1440 100" preserveAspectRatio="none"
       xmlns="http://www.w3.org/2000/svg">
    <path d="M0,40 C360,100 720,0 1080,60 C1260,80 1380,20 1440,40 L1440,100 L0,100 Z"
          fill="currentColor"/>
  </svg>
</div>
```

```css
.wave-divider {
  width: 100%;
  line-height: 0;
  overflow: hidden;
  color: var(--color-secondary-100); /* цвет следующей секции */
}
.wave-divider svg {
  width: 100%;
  height: 60px;
}
@media (min-width: 768px) {
  .wave-divider svg { height: 100px; }
}
```

Варианты волн:
- `.wave-divider--flip` - перевёрнутая (scaleY(-1))
- `.wave-divider--primary` - цвет primary-100
- `.wave-divider--white` - белая

### 4.4 Иконки

Источник: Lucide Icons (open-source, легковесные).

Ключевые иконки:
- `droplet` — вода, основная тема
- `truck` — доставка
- `clock` — режим работы
- `phone` — телефон
- `shield-check` — качество / сертификация
- `award` — преимущество
- `thermometer` — температура воды
- `refresh-cw` — возврат / обмен тары
- `map-pin` — адрес
- `star` — рейтинг / отзывы
- `chevron-down` — аккордеон FAQ
- `menu` / `x` — бургер-меню

Размеры: 20px (inline), 24px (навигация), 32px (карточки преимуществ), 48px (герой-иконки).

### 4.5 Формы

```css
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
.form-input:focus {
  outline: none;
  border-color: var(--color-primary-500);
  box-shadow: 0 0 0 3px rgba(26, 117, 196, 0.15);
  background: var(--color-white);
}
.form-input::placeholder {
  color: var(--color-neutral-300);
}
.form-label {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  color: var(--color-neutral-700);
  margin-bottom: var(--space-2);
  display: block;
}
.form-select {
  /* наследует .form-input + кастомная стрелка */
  appearance: none;
  background-image: url("data:image/svg+xml,..."); /* chevron-down */
  background-repeat: no-repeat;
  background-position: right 12px center;
  padding-right: 40px;
}
```

---

## 5. Анимации и переходы

| Токен                   | Значение                      | Применение                 |
|-------------------------|-------------------------------|----------------------------|
| --transition-fast       | 150ms ease                    | Hover цвета, opacity       |
| --transition-base       | 250ms ease                    | Общие переходы             |
| --transition-smooth     | 350ms cubic-bezier(.4,0,.2,1) | Трансформации, slide       |
| --transition-spring     | 500ms cubic-bezier(.34,1.56,.64,1) | Появление элементов  |

### Scroll-анимации (Intersection Observer)

- **fade-up**: opacity 0 -> 1, translateY(24px) -> 0. Stagger: 100ms между элементами.
- **fade-in**: opacity 0 -> 1. Для текстовых блоков.
- **scale-in**: scale(0.95) -> scale(1) + opacity. Для карточек.

---

## 6. CSS-переменные (полный блок)

```css
:root {
  /* === Colors: Primary === */
  --color-primary-900: #062D54;
  --color-primary-800: #083C6E;
  --color-primary-700: #0A4D8C;
  --color-primary-600: #1060A6;
  --color-primary-500: #1A75C4;
  --color-primary-400: #3D93DB;
  --color-primary-300: #6BB0E8;
  --color-primary-200: #A3D0F2;
  --color-primary-100: #D1E8F9;
  --color-primary-50: #EBF4FC;

  /* === Colors: Secondary === */
  --color-secondary-500: #7DD3E8;
  --color-secondary-300: #B0E6F2;
  --color-secondary-100: #E0F5FA;
  --color-secondary-50: #F0FAFE;

  /* === Colors: Accent === */
  --color-accent-600: #0D8F7F;
  --color-accent-500: #11B8A4;
  --color-accent-400: #3DD4C0;
  --color-accent-200: #A1EDDF;
  --color-accent-100: #D4F7F0;

  /* === Colors: CTA === */
  --color-cta-700: #C45A08;
  --color-cta-600: #E06A0A;
  --color-cta-500: #F28C28;
  --color-cta-400: #F5A855;
  --color-cta-100: #FDE8CC;

  /* === Colors: Neutral === */
  --color-neutral-900: #1A1A2E;
  --color-neutral-700: #3D3D56;
  --color-neutral-500: #6B6B80;
  --color-neutral-300: #B0B0C0;
  --color-neutral-200: #D4D4DE;
  --color-neutral-100: #EDEDF2;
  --color-neutral-50: #F7F7FA;
  --color-white: #FFFFFF;

  /* === Gradients === */
  --gradient-hero: linear-gradient(135deg, #062D54 0%, #0A4D8C 50%, #1A75C4 100%);
  --gradient-water: linear-gradient(180deg, #E0F5FA 0%, #D1E8F9 100%);
  --gradient-cta: linear-gradient(135deg, #F28C28 0%, #E06A0A 100%);
  --gradient-card-hover: linear-gradient(180deg, #EBF4FC 0%, #E0F5FA 100%);
  --gradient-footer: linear-gradient(180deg, #083C6E 0%, #062D54 100%);

  /* === Shadows === */
  --shadow-sm: 0 1px 3px rgba(10, 77, 140, 0.08);
  --shadow-md: 0 4px 12px rgba(10, 77, 140, 0.12);
  --shadow-lg: 0 8px 30px rgba(10, 77, 140, 0.16);
  --shadow-cta: 0 4px 16px rgba(242, 140, 40, 0.35);

  /* === Typography === */
  --font-heading: 'Montserrat', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --text-xl: 1.25rem;
  --text-lg: 1.125rem;
  --text-base: 1rem;
  --text-sm: 0.875rem;
  --text-xs: 0.75rem;

  /* === Spacing === */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;
  --space-20: 80px;
  --space-24: 96px;

  /* === Layout === */
  --container-max: 1200px;
  --grid-columns: 12;
  --grid-gutter: 24px;
  --grid-gutter-mobile: 16px;

  /* === Breakpoints (for reference, use in @media) === */
  --bp-mobile: 480px;
  --bp-tablet: 768px;
  --bp-desktop: 1024px;
  --bp-wide: 1280px;

  /* === Border Radius === */
  --radius-sm: 6px;
  --radius-md: 12px;
  --radius-lg: 20px;
  --radius-xl: 32px;
  --radius-full: 9999px;

  /* === Transitions === */
  --transition-fast: 150ms ease;
  --transition-base: 250ms ease;
  --transition-smooth: 350ms cubic-bezier(.4, 0, .2, 1);
  --transition-spring: 500ms cubic-bezier(.34, 1.56, .64, 1);
}
```
