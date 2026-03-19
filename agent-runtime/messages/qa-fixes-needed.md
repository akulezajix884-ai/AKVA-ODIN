# QA Fixes Needed — Итерация 2

Приоритет: CRITICAL > HIGH > MEDIUM > LOW

---

## Исправления для index.html

### CRITICAL

1. **Строки 736-768 (секция #cta):** Добавить форму заказа. Между `<p class="cta-block__subtitle">` и кнопкой "Заказать доставку" вставить:
   ```html
   <form class="cta-form" id="orderForm" novalidate>
     <div class="cta-form__fields">
       <input type="text" class="form-input" name="name" placeholder="Ваше имя" required aria-label="Ваше имя">
       <input type="tel" class="form-input" name="phone" placeholder="Телефон" required aria-label="Телефон">
       <input type="text" class="form-input" name="address" placeholder="Адрес доставки" aria-label="Адрес доставки">
     </div>
     <button type="submit" class="btn-primary cta-block__btn">Заказать доставку</button>
     <p class="cta-form__disclaimer">Без обязательств. Отменить можно в любой момент. Оплата при получении.</p>
   </form>
   ```
   Удалить текущую ссылку `<a href="tel:..." class="btn-primary cta-block__btn">` (строка 748) и inline-стиль `<p>` (строка 749) — они заменяются формой.

2. **Строка 20:** Подключить JetBrains Mono. Заменить URL Google Fonts на:
   ```
   https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@500;700&family=Montserrat:wght@500;600;700;800&display=swap
   ```

### HIGH

3. **Строка 119:** Синхронизировать H1 с copy.md. Заменить "Чистая вода для вашей семьи — с доставкой до двери" на "Вода из горных источников — к вашей двери" (из copy.md).

4. **Строка 120:** Заменить подзаголовок на версию из copy.md: "Доставляем бутилированную воду 19 л по Одинцово и району. Четыре бренда на выбор, привозим в день заказа — ежедневно с 8:00 до 22:00."

5. **Строки 584-637:** Заменить все три отзыва на версии из copy.md:
   - Отзыв 1: Ольга М., мама двоих детей — текст из copy.md строки 185
   - Отзыв 2: Дмитрий К., офис-менеджер — текст из copy.md строки 194
   - Отзыв 3: Валентина и Геннадий С., пенсионеры — текст из copy.md строки 203

6. **Строка 50:** Изменить `<a href="#faq">Контакты</a>` на `<a href="#footer">Контакты</a>`.

7. **Строка 49:** Изменить `<a href="#benefits">Доставка</a>` на `<a href="#benefits">Преимущества</a>` или оставить "Доставка" но с href на соответствующий контент.

8. **После строки 13 (в `<head>`):** Добавить OG-теги:
   ```html
   <meta property="og:image" content="https://akvavita.ru/assets/og-image.jpg">
   <meta property="og:url" content="https://akvavita.ru">
   ```

9. **После строки 13 (в `<head>`):** Добавить favicon:
   ```html
   <link rel="icon" type="image/svg+xml" href="favicon.svg">
   ```

### MEDIUM

10. **Строка 125:** Вынести inline style. Заменить `<p class="hero__subtitle animate-on-scroll stagger-2" style="font-size: 0.85rem; opacity: 0.7; margin-bottom: var(--space-4);">` на `<p class="hero__microtext animate-on-scroll stagger-2">`.

11. **Строка 749:** Вынести inline style. Заменить `<p class="animate-on-scroll stagger-2" style="color: var(--color-primary-300); font-size: 0.85rem; margin-top: 12px;">` на `<p class="cta-block__microtext animate-on-scroll stagger-2">`.

12. **Строки 142, 204, 284, 344, 567, 658, 728:** Убрать inline styles с wave-dividers. Добавить CSS-класс `.wave-divider--hero-bottom` и т.д. для позиционирования.

13. **Строка 410:** Заменить `style="background: #F28C28;"` на класс `.product-card__badge--kids`.

14. **Строка 430:** Заменить `style="background: #1A75C4;"` на класс `.product-card__badge--nature`.

15. **Строка 1164 (JS):** Изменить порог sticky CTA с `scrollPercent > 0.3` на `scrollPercent > 0.5` (по спецификации funnel.md и cta-texts.md).

---

## Исправления для style.css

### CRITICAL

1. **Строка 66 (после `--font-body`):** Добавить:
   ```css
   --font-mono: 'JetBrains Mono', monospace;
   ```

2. **Строка 1222:** Заменить `font-family: var(--font-heading);` на `font-family: var(--font-mono);` в `.product-card__price`.

3. **Строки 1431-1433:** Заменить:
   ```css
   .faq-item__question:focus {
     outline: none;
     color: var(--color-primary-700);
   }
   ```
   На:
   ```css
   .faq-item__question:focus-visible {
     outline: 2px solid var(--color-primary-400);
     outline-offset: 2px;
     color: var(--color-primary-700);
   }
   ```

4. **После строки 1559 (или в конце секции CTA):** Добавить стили формы:
   ```css
   /* CTA Form */
   .cta-form {
     max-width: 480px;
     margin: 0 auto;
   }
   .cta-form__fields {
     display: flex;
     flex-direction: column;
     gap: var(--space-3);
     margin-bottom: var(--space-4);
   }
   .form-input {
     font-family: var(--font-body);
     font-size: var(--text-base);
     padding: 12px 16px;
     border: 2px solid rgba(255, 255, 255, 0.2);
     border-radius: var(--radius-md);
     background: rgba(255, 255, 255, 0.1);
     color: var(--color-white);
     width: 100%;
     transition: border-color 0.2s ease, box-shadow 0.2s ease, background 0.2s ease;
   }
   .form-input::placeholder {
     color: var(--color-primary-300);
   }
   .form-input:focus {
     outline: none;
     border-color: var(--color-cta-500);
     box-shadow: 0 0 0 3px rgba(242, 140, 40, 0.2);
     background: rgba(255, 255, 255, 0.15);
   }
   .cta-form__disclaimer {
     color: var(--color-primary-300);
     font-size: var(--text-xs);
     margin-top: var(--space-3);
   }
   @media (min-width: 768px) {
     .cta-form__fields {
       flex-direction: row;
     }
   }
   ```

### HIGH

5. **Строка 1452:** Заменить `max-height: 300px;` на `max-height: 500px;`.

6. **После строки 87:** Добавить переменные сетки:
   ```css
   --grid-columns: 12;
   --grid-gutter: 24px;
   --grid-gutter-mobile: 16px;
   ```

### MEDIUM

7. **Строки 1860-1864:** Удалить или закомментировать переопределение контейнера на 1320px:
   ```css
   /* Удалить:
   @media (min-width: 1440px) {
     .container {
       max-width: 1320px;
     }
   }
   */
   ```

8. **После строки 1833 (утилиты):** Добавить классы для inline-стилей:
   ```css
   .hero__microtext {
     font-size: 0.85rem;
     opacity: 0.7;
     margin-bottom: var(--space-4);
     color: var(--color-primary-200);
   }
   .cta-block__microtext {
     color: var(--color-primary-300);
     font-size: 0.85rem;
     margin-top: var(--space-3);
   }
   ```

9. **После строки 1183:** Добавить модификаторы бейджей:
   ```css
   .product-card__badge--kids {
     background: var(--color-cta-500);
   }
   .product-card__badge--nature {
     background: var(--color-primary-500);
   }
   ```

10. **После строки 555:** Добавить класс для абсолютного позиционирования волны hero:
    ```css
    .wave-divider--abs-bottom {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
    }
    .wave-divider--mt {
      margin-top: var(--space-10);
    }
    ```

---

## Исправления для copy.md

### HIGH

1. **Секция 1 (Hero), строка 8:** Синхронизировать H1. Если принято решение использовать "Чистая вода для вашей семьи — с доставкой до двери" из HTML, обновить copy.md. Или наоборот — обновить HTML. Выбрать один вариант и зафиксировать.

2. **Секция 2 (О воде), строка 27:** Заголовок "Вода, в которой не нужно сомневаться" — содержит двойное отрицание. В HTML использован лучший вариант "Вода, в которой вы можете быть уверены". Обновить copy.md.

3. **Секция 4 (Воронка, Шаг 1), строка 66:** Смягчить заголовок "Знаете ли вы, что течёт из вашего крана?" до "Задумывались ли вы о воде, которую пьёте каждый день?"

### MEDIUM

4. **Секция 6 (Отзывы):** Рассмотреть добавление одного отзыва с рейтингом 4/5 (мелкое замечание + общая похвала) для повышения достоверности.

5. **Секция 8 (CTA):** Синхронизировать заголовок с HTML. В HTML: "Закажите воду — привезём сегодня". В copy.md: "Попробуйте воду, к которой не захочется добавлять фильтр". Принять одно решение.

---

## Заметки для следующей итерации (backlog)

- Калькулятор стоимости (funnel.md шаг 4)
- Сравнительная таблица "водопровод vs АкваВита" (funnel.md шаг 2)
- Exit-intent popup (funnel.md шаг 5)
- Блок "Знаете ли вы..." с фактами о водопроводной воде (funnel.md шаг 1)
- B2B логотипы клиентов (funnel.md шаг 3)
- Принцип дефицита Чалдини ("осталось N слотов доставки")
