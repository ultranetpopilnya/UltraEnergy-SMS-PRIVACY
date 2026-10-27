# Політика конфіденційності — UltraEnergy SMS Tool

**Дата набрання чинності:** 27 жовтня 2026 р.  
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

**Баланс сервісів.** Розширення отримує від проксі-сервера поточні числові значення балансів для Ultranet та ISP Energy, щоб показувати їх у своєму інтерфейсі. Отримані значення кешуються локально в `chrome.storage.local`.

**Статус доставки SMS.** Після відправки SMS Розширення періодично запитує в проксі-сервера статус доставки цього повідомлення (доставлено, не доставлено або ще невідомо) і показує його на кнопці відправки та на іконці Розширення. Для цього використовується ідентифікатор повідомлення, який повернув сервіс TurboSMS при відправці.

**Сповіщення.** Якщо користувач увімкнув у налаштуваннях сповіщення про доставку (за замовчуванням вони вимкнені), Розширення показує локальне системне сповіщення про результат доставки з останніми чотирма цифрами номера одержувача. Сповіщення формується на комп'ютері користувача й нікуди не передається. Для цього Розширення використовує дозвіл браузера `notifications`.

**Дані, які вводить або налаштовує користувач:**
- текст SMS-повідомлення (він може містити номер договору та пароль абонента, якщо їх включено в шаблон);
- шаблони повідомлень, тарифи SMS, налаштування інтерфейсу та сповіщень;
- ключ доступу, виданий компанією.

**Ключ доступу.** Розширення використовує ключ доступу для авторизації запитів до проксі-сервера: перевірки ключа, отримання балансів, відправки SMS і отримання статусу доставки. Ключ передається через HTTPS (TLS) у заголовку `Authorization`.

## 3. Як використовуються дані

Дані використовуються для основної мети Розширення: скласти текст SMS і відправити його вибраному абоненту, показати користувачу результат доставки цього повідомлення, а також отримати й показати поточні баланси сервісів SMS для Ultranet та ISP Energy. Баланси використовуються для відображення в інтерфейсі й кешуються локально, щоб їх можна було показати до завершення наступного оновлення.

## 4. Куди передаються дані

Розширення звертається до власного проксі-сервера (`https://sms-proxy.vadymtryhubets.workers.dev`) для перевірки ключа доступу, отримання балансів, відправки SMS і отримання статусу доставки. Дані залежать від типу запиту:
- **Перевірка ключа (`/check`) і отримання балансів (`/balance`):** ключ доступу в заголовку `Authorization`.
- **Відправка SMS (`/send`):** номер телефону одержувача, текст повідомлення (може містити номер договору та пароль абонента, якщо їх включено в шаблон), ідентифікатор мережі (Ultranet або ISP Energy) та ключ доступу для автентифікації.
- **Статус доставки (`/status`):** ідентифікатор повідомлення, ідентифікатор мережі (Ultranet або ISP Energy) та ключ доступу для автентифікації. Номер телефону та текст повідомлення в цьому запиті не передаються.

Запит `/balance` не містить номера телефону, номера договору, пароля абонента чи тексту SMS. У відповідь проксі-сервер повертає числові значення балансів для відображення в розширенні; вони кешуються локально в браузері (див. розділ 5).

**Проксі-сервер.** Проксі-сервер працює на інфраструктурі Cloudflare Workers. Він перевіряє ключ доступу та коректність запитів, передає запити на відправку SMS і запити статусу доставки до TurboSMS та повертає розширенню значення балансів і статус доставки. Сервер не зберігає номери телефонів, паролі абонентів і тексти повідомлень та не веде історії відправлених SMS. Він записує загальний добовий лічильник відправок без персональних даних для захисту від надмірного використання; лічильник автоматично видаляється приблизно через дві доби. Cloudflare як постачальник інфраструктури може вести технічні журнали запитів (метадані, наприклад час і статус запиту).

**TurboSMS.** Після передачі до TurboSMS повідомлення та номер одержувача обробляються і зберігаються в історії сервісу відповідно до його власних умов і політики конфіденційності. Статус доставки також надається цим сервісом. Розробник Розширення не контролює це зберігання.

**Сайти білінгу.** Розширення зчитує дані зі сторінок білінгу `bill.ultranetgroup.com.ua` та `bill.ispenergy.com.ua` і виконує запити до цих сайтів від імені користувача в межах його поточного сеансу, щоб отримати дані абонента. Ці сайти належать компаніям, для співробітників яких призначене Розширення.

Розширення **не передає** дані будь-яким іншим третім особам, не продає їх і не використовує в рекламі.

## 5. Де і як довго зберігаються дані

- **На вашому комп'ютері** (сховище браузера `chrome.storage.local`): налаштування інтерфейсу та сповіщень, шаблони, тарифи SMS, збережений ключ доступу та кеш останніх отриманих балансів Ultranet й ISP Energy. Кеш балансів оновлюється після успішного запиту до проксі-сервера. Локальні дані зберігаються, доки ви не видалите Розширення або не очистите його дані.
- **Тимчасово** (сховище сеансу браузера `chrome.storage.session`): дані щойно відкритих абонентів (до 10 записів: номери телефонів, номер договору, пароль, сума кредиту) і чернетки повідомлень, щоб не втрачати введене при закритті вікна. Вони автоматично видаляються, коли ви закриваєте браузер.
- **Статус доставки:** під час відстеження (до 90 секунд після відправки) ідентифікатор повідомлення та останні цифри номера лише тимчасово зберігаються в пам'яті фонового процесу Розширення й не записуються у сховище.
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

Ви можете в будь-який момент видалити всі локальні дані Розширення, видаливши його з браузера. Сповіщення можна вимкнути в налаштуваннях Розширення. Щодо даних, які зберігаються в TurboSMS, звертайтеся до цього сервісу. З питань щодо цієї політики пишіть на адресу, вказану вище.

## 10. Зміни

У разі змін у політиці оновлена версія буде опублікована на цій сторінці з новою датою набрання чинності.

---

# Privacy Policy — UltraEnergy SMS Tool

**Effective date:** October 27, 2026  
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

**Service balances.** The Extension receives current numeric balance values for Ultranet and ISP Energy from its proxy server and displays them in the interface. The returned values are cached locally in `chrome.storage.local`.

**SMS delivery status.** After an SMS is sent, the Extension periodically asks the proxy server for the delivery status of that message (delivered, not delivered, or not yet known) and shows it on the send button and on the Extension icon. This uses the message identifier returned by TurboSMS when the message was sent.

**Notifications.** If the user turns on delivery notifications in the settings (they are off by default), the Extension shows a local system notification with the delivery result and the last four digits of the recipient's number. The notification is generated on the user's computer and is not transmitted anywhere. The Extension uses the browser `notifications` permission for this.

**Data provided or configured by the user:**
- SMS text (it may include the subscriber's contract number and password if the template contains them);
- message templates, SMS prices, interface and notification settings;
- the access key issued by the company.

**Access key.** The Extension uses the access key to authorize requests to the proxy server for key verification, balance retrieval, SMS sending, and delivery status retrieval. The key is sent over HTTPS (TLS) in the `Authorization` header.

## 3. How the data is used

The data is used for the Extension's purpose: composing and sending an SMS to the selected subscriber, showing the user the delivery result of that message, and retrieving and displaying current SMS service balances for Ultranet and ISP Energy. Balance values are displayed in the interface and cached locally so they can be shown before the next refresh completes.

## 4. Data sharing

The Extension contacts its own proxy server (`https://sms-proxy.vadymtryhubets.workers.dev`) to verify the access key, retrieve balances, send SMS messages, and retrieve delivery status. The data depends on the request:
- **Key verification (`/check`) and balance retrieval (`/balance`):** the access key in the `Authorization` header.
- **SMS sending (`/send`):** the recipient's phone number, message text (which may include the subscriber's contract number and password if the template contains them), the target network identifier (Ultranet or ISP Energy), and the access key for authentication.
- **Delivery status (`/status`):** the message identifier, the target network identifier (Ultranet or ISP Energy), and the access key for authentication. The phone number and message text are not included in this request.

The `/balance` request does not include a subscriber's phone number, contract number, password, or SMS text. The proxy returns numeric balance values for display in the Extension; these values are cached locally in the browser (see section 5).

**Proxy server.** The proxy runs on Cloudflare Workers infrastructure. It validates the access key and requests, forwards SMS-sending requests and delivery status requests to TurboSMS, and returns balance values and delivery status to the Extension. It does not store phone numbers, subscriber passwords or message text and keeps no SMS history. It records an overall daily send counter containing no personal data for abuse prevention; this counter is automatically deleted after about two days. Cloudflare, as the infrastructure provider, may keep technical request logs (metadata such as request time and status).

**TurboSMS.** Once passed to TurboSMS, the message and the recipient number are processed and kept in that service's history under its own terms and privacy policy. The delivery status is also provided by that service. The developer of the Extension does not control that storage.

**Billing sites.** The Extension reads pages of, and makes requests to, `bill.ultranetgroup.com.ua` and `bill.ispenergy.com.ua` within the user's own session to obtain subscriber data. These sites belong to the companies whose employees the Extension is intended for.

The Extension does not share data with any other third party, does not sell data and does not use it for advertising.

## 5. Storage and retention

- **Locally** (`chrome.storage.local`): interface and notification settings, templates, SMS prices, the saved access key, and a cache of the latest retrieved Ultranet and ISP Energy balance values. The balance cache is updated after a successful request to the proxy server. Local data remains until you remove the Extension or clear its data.
- **Temporarily** (`chrome.storage.session`): recently opened subscribers (up to 10 records: phone numbers, contract number, password, credit amount) and message drafts; removed automatically when the browser is closed.
- **Delivery status tracking:** while a message is being tracked (up to 90 seconds after sending), the message identifier and the last digits of the number are held only in the memory of the Extension's background process and are not written to storage.
- **On the proxy server:** only a daily send counter containing no personal data (see section 4).
- **At TurboSMS:** history of sent messages under that service's rules (see section 4).

## 6. Security

All connections to the proxy server and the TurboSMS gateway are encrypted via HTTPS (TLS). The access key and local preferences are stored within the Extension's isolated browser storage. No method of transmission or storage is absolutely secure, so use the Extension only on trusted work computers and contact your administrator immediately if a compromise of the access key is suspected.

## 7. What the Extension does not do

It collects no analytics or browsing history, does not track user actions or keystrokes, does not use tracking cookies, does not load or run remote code, does not sell or transfer user data for purposes unrelated to sending SMS, and does not use data to determine creditworthiness.

## 8. Compliance

The use of data received by the Extension complies with the [Chrome Web Store User Data Policy](https://developer.chrome.com/docs/webstore/program_policies/), including the Limited Use requirements.

## 9. User rights

You can delete all local Extension data at any time by removing the Extension from your browser. Notifications can be turned off in the Extension settings. For data kept at TurboSMS, please contact that service. Questions about this policy can be sent to the contact address above.

## 10. Changes

Updates to this policy will be published on this page with a new effective date.
