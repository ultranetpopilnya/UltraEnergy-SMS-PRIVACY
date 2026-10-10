# Політика конфіденційності — UltraEnergy SMS Tool

**Дата набрання чинності:** 10 жовтня 2026 р.
**Розробник:** Вадим Тригубець (незалежний розробник)
**Контакт:** mynameisfreeman@gmail.com

*English version is below.*

---

## 1. Про розширення

UltraEnergy SMS Tool (далі — «Розширення») — робочий інструмент для співробітників Ultranet Group та ISP Energy. Воно дозволяє відправляти SMS абонентам зі сторінки білінгу через сервіс TurboSMS.

Розширення не є офіційним продуктом Ultranet Group, ISP Energy чи TurboSMS. Воно створене незалежним розробником з дозволу компаній Ultranet Group та ISP Energy для використання їхніми співробітниками.

Розширення призначене виключно для співробітників цих компаній і не призначене для загального користування. Для роботи потрібні ключ доступу, виданий компанією, та доступ до її білінгу.

## 2. Які дані обробляє Розширення

**Дані абонента** (зчитуються зі сторінки білінгу, яку відкрив користувач):
- номери телефонів;
- номер договору та пароль абонента;
- сума кредиту.

**Дані, які вводить користувач:**
- текст SMS-повідомлення;
- ключі API TurboSMS та назви відправників;
- шаблони повідомлень, тарифи SMS, налаштування інтерфейсу.

**Код доступу.** Розширення просить код доступу, виданий компанією. Код перевіряється локально, порівнянням із контрольним значенням у коді Розширення. Він нікуди не передається.

## 3. Як використовуються дані

Дані використовуються лише для основної мети Розширення: скласти текст SMS і відправити його вибраному абоненту. Інших цілей немає.

## 4. Куди передаються дані

Розширення передає дані лише в сервіс **TurboSMS** (`api.turbosms.ua`), і тільки під час відправки SMS:
- номер телефону одержувача;
- текст повідомлення;
- назву відправника;
- ключ API (для автентифікації запиту).

Це необхідно для відправки SMS. Правила обробки даних у TurboSMS описані на сайті сервісу.

Також Розширення зчитує дані зі сторінок білінгу `bill.ultranetgroup.com.ua` та `bill.ispenergy.com.ua` і виконує запити до цих сайтів від імені користувача в межах його поточного сеансу, щоб отримати дані абонента. Ці сайти належать компаніям, для співробітників яких призначене Розширення.

Розширення **не передає** дані будь-яким іншим третім особам, не продає їх і не використовує в рекламі.

## 5. Де і як довго зберігаються дані

- **На вашому комп'ютері** (сховище браузера `chrome.storage.local`): налаштування, шаблони, тарифи SMS, зашифровані ключі API. Ці дані зберігаються, доки ви не видалите Розширення або не очистите його дані.
- **Тимчасово** (сховище сеансу браузера `chrome.storage.session`): дані щойно відкритих абонентів (до 10 записів) і чернетки повідомлень, щоб не втрачати введене при закритті вікна. Вони автоматично видаляються, коли ви закриваєте браузер.

Розширення не має власного сервера й не зберігає ваші дані ніде, крім вашого браузера.

## 6. Безпека

Ключі API зберігаються на вашому пристрої в зашифрованому вигляді. Проте жоден спосіб зберігання не гарантує абсолютного захисту, тому ми рекомендуємо:
- використовувати розширення лише на довірених комп'ютерах;
- у разі підозри на витік негайно перегенерувати ключ API в особистому кабінеті TurboSMS.

## 7. Чого Розширення не робить

- не збирає аналітику, статистику використання та дані про відвідані сторінки;
- не відстежує дії користувача й не записує натискання клавіш;
- не використовує файли cookie для відстеження;
- не завантажує та не виконує віддалений код;
- не продає та не передає дані користувачів для цілей, не пов'язаних із відправкою SMS;
- не використовує дані для визначення кредитоспроможності.

## 8. Відповідність політикам Chrome Web Store

Використання даних, отриманих Розширенням, відповідає [Політиці щодо даних користувачів Chrome Web Store](https://developer.chrome.com/docs/webstore/program_policies/), зокрема вимогам обмеженого використання (Limited Use).

## 9. Права користувача

Ви можете в будь-який момент видалити всі дані Розширення, видаливши його з браузера. Якщо у вас є питання щодо цієї політики, напишіть на адресу, вказану вище.

## 10. Зміни

У разі змін у політиці оновлена версія буде опублікована на цій сторінці з новою датою набрання чинності.

---

# Privacy Policy — UltraEnergy SMS Tool

**Effective date:** October 10, 2026
**Developer:** Vadym Tryhubets (independent developer)
**Contact:** mynameisfreeman@gmail.com

## 1. About

UltraEnergy SMS Tool (the "Extension") is a work tool for employees of Ultranet Group and ISP Energy. It lets them send SMS messages to subscribers directly from the billing page through the TurboSMS service.

The Extension is not an official product of Ultranet Group, ISP Energy or TurboSMS. It was created by an independent developer with the permission of Ultranet Group and ISP Energy for use by their employees. It is intended solely for those employees and not for general public use. It requires an access code issued by the company and access to the company's billing system.

## 2. Data the Extension handles

- **Subscriber data** read from the billing page opened by the user: phone numbers, contract number, subscriber password, credit amount.
- **User-provided data:** SMS text, TurboSMS API keys and sender names, message templates, SMS prices, interface settings.
- **Access code:** checked locally against a reference value in the Extension's code; it is never transmitted.

## 3. How the data is used

Only for the Extension's single purpose: composing and sending an SMS to the selected subscriber.

## 4. Data sharing

The Extension sends data only to **TurboSMS** (`api.turbosms.ua`), and only when an SMS is sent: the recipient phone number, the message text, the sender name and the API key (to authenticate the request). It also reads pages of, and makes requests to, `bill.ultranetgroup.com.ua` and `bill.ispenergy.com.ua` within the user's own session to obtain subscriber data. The Extension does not share data with any other third party, does not sell data and does not use it for advertising.

## 5. Storage and retention

- **Locally** (`chrome.storage.local`): settings, templates, prices and encrypted API keys, until you remove the Extension or clear its data.
- **Temporarily** (`chrome.storage.session`): recently opened subscribers (up to 10) and message drafts; removed automatically when the browser is closed.

The Extension has no server of its own and stores no data outside your browser.

## 6. Security

API keys are stored on your device in encrypted form. No storage method is absolutely secure, so use the Extension only on trusted computers and regenerate your API key in your TurboSMS account if you suspect a leak.

## 7. What the Extension does not do

It collects no analytics or browsing history, does not track user actions or keystrokes, does not load or run remote code, does not sell or transfer user data for purposes unrelated to sending SMS, and does not use data to determine creditworthiness.

## 8. Compliance

The use of data received by the Extension complies with the [Chrome Web Store User Data Policy](https://developer.chrome.com/docs/webstore/program_policies/), including the Limited Use requirements.

## 9. User rights and changes

You can delete all Extension data at any time by removing the Extension from your browser. Questions can be sent to the contact address above. Updates to this policy will be published on this page with a new effective date.
