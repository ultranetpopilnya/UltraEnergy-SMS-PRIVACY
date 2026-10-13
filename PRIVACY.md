# Політика конфіденційності — UltraEnergy SMS Tool

**Дата набрання чинності:** 13 жовтня 2026 р.  
**Розробник:** Вадим Тригубець (незалежний розробник)  
**Контакт:** vadymtryhubets@gmail.com  

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

**Дані, які вводить або налаштовує користувач:**
- текст SMS-повідомлення (він може містити номер договору та пароль абонента, якщо їх включено в шаблон);
- шаблони повідомлень, тарифи SMS, налаштування інтерфейсу;
- ключ доступу, виданий компанією.

**Ключ доступу.** Розширення використовує ключ доступу для авторизації запитів до проксі-сервера Розширення. Ключ передається через протокол HTTPS (TLS) у заголовку авторизації для перевірки прав доступу перед кожною відправкою.

## 3. Як використовуються дані

Дані використовуються лише для основної мети Розширення: скласти текст SMS і відправити його вибраному абоненту. Інших цілей немає.

## 4. Куди передаються дані

Розширення передає дані через власний проксі-сервер (`https://sms-proxy.vadymtryhubets.workers.dev`) до шлюзу сервісу **TurboSMS** (`api.turbosms.ua`), і тільки під час перевірки авторизації або відправки SMS:
- номер телефону одержувача;
- текст повідомлення (може містити номер договору та пароль абонента, якщо їх включено в шаблон);
- ідентифікатор мережі (Ultranet або ISP Energy);
- ключ доступу (для автентифікації запиту).

**Проксі-сервер.** Проксі-сервер працює на інфраструктурі Cloudflare Workers. Він лише перевіряє ключ доступу, перевіряє коректність запиту та передає його до TurboSMS. Він не зберігає номери телефонів, паролі абонентів і тексти повідомлень та не веде історії відправлених SMS. Єдине, що він записує, — це загальний добовий лічильник відправок без персональних даних (захист від надмірного використання), який автоматично видаляється приблизно через дві доби. Cloudflare як постачальник інфраструктури може вести технічні журнали запитів (метадані, наприклад час і статус запиту), але не вміст повідомлень.

**TurboSMS.** Після передачі до TurboSMS повідомлення та номер одержувача обробляються і зберігаються в історії сервісу відповідно до його власних умов і політики конфіденційності. Розробник Розширення не контролює це зберігання.

**Сайти білінгу.** Розширення зчитує дані зі сторінок білінгу `bill.ultranetgroup.com.ua` та `bill.ispenergy.com.ua` і виконує запити до цих сайтів від імені користувача в межах його поточного сеансу, щоб отримати дані абонента. Ці сайти належать компаніям, для співробітників яких призначене Розширення.

Розширення **не передає** дані будь-яким іншим третім особам, не продає їх і не використовує в рекламі.

## 5. Де і як довго зберігаються дані

- **На вашому комп'ютері** (сховище браузера `chrome.storage.local`): налаштування інтерфейсу, шаблони, тарифи SMS, збережений ключ доступу. Ці дані зберігаються, доки ви не видалите Розширення або не очистите його дані.
- **Тимчасово** (сховище сеансу браузера `chrome.storage.session`): дані щойно відкритих абонентів (до 10 записів: номери телефонів, номер договору, пароль, сума кредиту) і чернетки повідомлень, щоб не втрачати введене при закритті вікна. Вони автоматично видаляються, коли ви закриваєте браузер.
- **На проксі-сервері:** лише добовий лічильник відправок без персональних даних (див. розділ 4).
- **У TurboSMS:** історія відправлених повідомлень за правилами цього сервісу (див. розділ 4).

## 6. Безпека

Всі з'єднання з проксі-сервером та сервісом TurboSMS здійснюються через зашифрований протокол HTTPS (TLS). Ключ доступу та локальні дані зберігаються в ізольованому сховищі браузера Розширення. Проте жоден спосіб зберігання та передачі не гарантує абсолютного захисту, тому ми рекомендуємо:
- використовувати Розширення лише на довірених робочих комп'ютерах;
- у разі підозри на компрометацію ключа доступу негайно звернутися до адміністратора для його деактивації.

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

Ви можете в будь-який момент видалити всі локальні дані Розширення, видаливши його з браузера. Щодо даних, які зберігаються в TurboSMS, звертайтеся до цього сервісу. З питань щодо цієї політики пишіть на адресу, вказану вище.

## 10. Зміни

У разі змін у політиці оновлена версія буде опублікована на цій сторінці з новою датою набрання чинності.

---

# Privacy Policy — UltraEnergy SMS Tool

**Effective date:** October 13, 2026  
**Developer:** Vadym Tryhubets (independent developer)  
**Contact:** vadymtryhubets@gmail.com  

## 1. About

UltraEnergy SMS Tool (the "Extension") is a work tool for employees of Ultranet Group and ISP Energy. It lets them send SMS messages to subscribers directly from the billing page through the TurboSMS service.

The Extension is not an official product of Ultranet Group, ISP Energy or TurboSMS. It was created by an independent developer with the permission of Ultranet Group and ISP Energy for use by their employees. It is intended solely for those employees and not for general public use. It requires an access key issued by the company and access to the company's billing system.

## 2. Data the Extension handles

**Subscriber data** read from the billing page opened by the user:
- phone numbers;
- contract number and subscriber password;
- credit amount.

**Data provided or configured by the user:**
- SMS text (it may include the subscriber's contract number and password if the template contains them);
- message templates, SMS prices, interface settings;
- the access key issued by the company.

**Access key.** The Extension uses the access key to authorize requests to the Extension's proxy server. The key is sent over HTTPS (TLS) in the authorization header so that access rights can be verified before each send.

## 3. How the data is used

Only for the Extension's single purpose: composing an SMS and sending it to the selected subscriber. There are no other purposes.

## 4. Data sharing

The Extension transmits data via its own proxy server (`https://sms-proxy.vadymtryhubets.workers.dev`) to the **TurboSMS** gateway (`api.turbosms.ua`), and only when verifying access or sending an SMS:
- recipient phone number;
- message text (may include the subscriber's contract number and password if the template contains them);
- target network identifier (Ultranet or ISP Energy);
- the access key (to authenticate the request).

**Proxy server.** The proxy runs on Cloudflare Workers infrastructure. It only verifies the access key, validates the request and forwards it to TurboSMS. It does not store phone numbers, subscriber passwords or message text, and keeps no message history. The only thing it records is an overall daily send counter containing no personal data (abuse protection), which is deleted automatically after about two days. Cloudflare, as the infrastructure provider, may keep technical request logs (metadata such as time and status) but not message content.

**TurboSMS.** Once passed to TurboSMS, the message and the recipient number are processed and kept in that service's history under its own terms and privacy policy. The developer of the Extension does not control that storage.

**Billing sites.** The Extension reads pages of, and makes requests to, `bill.ultranetgroup.com.ua` and `bill.ispenergy.com.ua` within the user's own session to obtain subscriber data. These sites belong to the companies whose employees the Extension is intended for.

The Extension does not share data with any other third party, does not sell data and does not use it for advertising.

## 5. Storage and retention

- **Locally** (`chrome.storage.local`): interface settings, templates, prices and the saved access key, until you remove the Extension or clear its data.
- **Temporarily** (`chrome.storage.session`): recently opened subscribers (up to 10 records: phone numbers, contract number, password, credit amount) and message drafts; removed automatically when the browser is closed.
- **On the proxy server:** only a daily send counter containing no personal data (see section 4).
- **At TurboSMS:** history of sent messages under that service's rules (see section 4).

## 6. Security

All connections to the proxy server and the TurboSMS gateway are encrypted via HTTPS (TLS). The access key and local preferences are stored within the Extension's isolated browser storage. No method of transmission or storage is absolutely secure, so use the Extension only on trusted work computers and contact your administrator immediately if a compromise of the access key is suspected.

## 7. What the Extension does not do

It collects no analytics or browsing history, does not track user actions or keystrokes, does not use tracking cookies, does not load or run remote code, does not sell or transfer user data for purposes unrelated to sending SMS, and does not use data to determine creditworthiness.

## 8. Compliance

The use of data received by the Extension complies with the [Chrome Web Store User Data Policy](https://developer.chrome.com/docs/webstore/program_policies/), including the Limited Use requirements.

## 9. User rights

You can delete all local Extension data at any time by removing the Extension from your browser. For data kept at TurboSMS, please contact that service. Questions about this policy can be sent to the contact address above.

## 10. Changes

Updates to this policy will be published on this page with a new effective date.
