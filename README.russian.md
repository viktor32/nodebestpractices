[✔]: assets/images/checkbox-small-blue.png

# Node.js Лучшие практики

<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Node.js Лучшие практики">
</h1>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/⚙%20Item%20count%20-%2079%20Best%20practices-blue.svg" alt="79 items"> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%20Jan%201%202019-green.svg" alt="Last update: January 1st, 2019"> <img src="https://img.shields.io/badge/%E2%9C%94%20Updated%20For%20Version%20-%20Node%2010.15.0%20LTS-brightgreen.svg" alt="Updated for Node 10.15.0 LTS">
</div>

<br/>

[![nodepractices](/assets/images/twitter-s.png)](https://twitter.com/nodepractices/) **Следуйте за нами на Twitter!** [**@nodepractices**](https://twitter.com/nodepractices/)

<br/>

Читать на других языках: [![CN](/assets/flags/CN.png)**CN**](/README.chinese.md) [(![BR](/assets/flags/BR.png)**BR**, ![ES](/assets/flags/ES.png)**ES**, ![FR](/assets/flags/FR.png)**FR**, ![HE](/assets/flags/HE.png)**HE**, ![KR](/assets/flags/KR.png)**KR**, ![RU](/assets/flags/RU.png)**RU** and ![TR](/assets/flags/TR.png)**TR** in progress!)](#translations)

<br/>

# Добро пожаловать! 3 вещи, которые вы должны знать в первую очередь:

**1. Читая это, на самом деле вы читаете лучшие статьи о Node.js -** это краткое изложение и список наиболее популярных материалов по Node.js.

**2. Это самый большой сборник, и он растет каждую неделю -** здесь представлено более 50 лучших практик, руководств по стилю и советов по архитектуре. Каждый день создаются новые задачи и PR, обновляя данные материалы. Мы бы хотели, чтобы вы также внесли свой вклад, будь то исправление ошибок в коде или предложение новой блестящей идеи. Смотри наш [milestones здесь](https://github.com/i0natan/nodebestpractices/milestones?direction=asc&sort=due_date&state=open)

**3. Большинство маркеров имеют дополнительную информацию -** рядом с большинством маркеров вы найдете ссылку **🔗 Подробнее**, перейдя по который вы сможете найти примеры кода, цитаты из блогов и другую дополнительную информацию.

<br/><br/><br/>

## Оглавление

1.  [Структура Проекта (5)](#1-project-structure-practices)
2.  [Обработка ошибок (11) ](#2-error-handling-practices)
3.  [Стиль Кода (12) ](#3-code-style-practices)
4.  [Практики Тестирования И Поддержания Качества (9) ](#4-testing-and-overall-quality-practices)
5.  [Практики Релиза (18) ](#5-going-to-production-practices)
6.  [Лучшие Практики По Безопасности (24)](#6-security-best-practices)
7.  [Лучшие Практики По Быстродействию (в работе)](#7-performance-best-practices)

<br/><br/><br/>

# `1. Структура Проекта`

## ![✔] 1.1 Структурируйте свое решение по компонентам

**TL;DR:** Самая большая проблема больших приложений - поддержка кодбазы с сотнями зависимостей - это очень сильно замедняет разработку и внедрение новых функций. Вместо этого разделите ваш код на компоненты, каждый из которых получает свою собственную папку или выделенную кодбазу, и убедитесь, что каждый модуль остается маленьким и простым. По ссылке «Подробнее» ниже вы найдете примеры правильной структуры проекта.

**В противном случае:** При разработке нового функционала разработчикам постоянно приходится осознавать влияние своего кода на все приложение в целом, так как появляется большая вероятность сломать другие зависимые компоненты, развертывание  приложения становится медленным, увеличиваются риски допустить ошибку. Также приложение с разделенной логикой легче поддается маштабированию.

🔗 [**Подробнее: компонентная структура**](/sections/projectstructre/breakintcomponents.russian.md)

<br/><br/>

## ![✔] 1.2 Разделяйте логику компонентов, отделяйте Express от бизнес-логики приложения

**TL;DR:** Каждый компонент должен содержать «слои» - выделенные объекты для web, логики и кода для работы с данными. Это не только четко разделяет задачи, но и значительно облегчает проверку и тестирование системы. Хотя это очень распространенный шаблон, разработчики API склонны смешивать слои, передавая объекты веб-слоя (Express req, res) в бизнес-логику и слои работы с данными - это делает ваше приложение зависимым и доступным только для Express.

**В противном случае:** Приложение, которое смешивает веб-объекты с другими слоями, сложно поддается покритием тестами, вызовами через CRON и другим обработками не из-под Express.

🔗 [**Подробнее: слои приложения**](/sections/projectstructre/createlayers.russian.md)

<br/><br/>

## ![✔] 1.3 Используйте приватные npm пакеты для переиспользуемых модулей

**TL; DR:** В приложении с большой кодбазой, модули и утилиты, которые могут быть переиспользованя (такие как регистрация, шифрование и т.п.) должны быть обернуты в ваши приватные пакеты npm. Это позволяет делиться ими между кодбазами и проектами

**В противном случае:** Вам придется каждый раз изобретать колесо при реализации подобного функционала

🔗 [**Read More: Структурирование функционала**](/sections/projectstructre/wraputilities.russian.md)

<br/><br/>

## ![✔] 1.4 Разделяйте Express 'app' и 'server'

**TL;DR:** Избегайте неприятной привычки определять все приложение [Express](https://expressjs.com/) в одном огромном файле - разделяйте ваш «Express» как минимум на два файла: объявление API (app.js) и сетевые задачи (WWW). Для еще лучшей структуры объявляйте API в компонентах

**В противном случае:** Ваш API будет доступен для тестирования только через HTTP-вызовы (медленнее и намного сложнее создавать отчеты о покрытии кода тестами). К тому же ни для кого не будет удовольствием работать с файлами в сотни и тысячи строк кода.

🔗 [**Подробнее: разделяйте Express 'app' и 'server'**](/sections/projectstructre/separateexpress.md)

<br/><br/>

## ![✔] 1.5 Конфигурация должна зависеть от переменных окружения, быть безопасной и поддерживать иерархию

**TL;DR:** Идеальная и безупречная конфигурация должна обеспечивать (а) считывание ключей из файла И из переменной среды (б) приватные ключи и пароли хранятся вне репозитория (в) конфигурация является иерархической для облегчения поиска. Есть несколько пакетов, которые могут помочь решить большинство из этих задач. К примеру [rc](https://www.npmjs.com/package/rc), [nconf](https://www.npmjs.com/package/nconf) и [config](https://www.npmjs.com/package/config)

**В противном случае:** Невыполнение каких-либо требований к конфигурации приведет к проблемам с разработкой или к проблемам для команды разработки. Вероятно, оба

🔗 [**Подробнее: правильная конфигурация**](/sections/projectstructre/configguide.russian.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Вернуться наверх</a></p>

# `2. Обработка ошибок`

## ![✔] 2.1 Используйте Async-Await или promises для асинхронной обработки ошибок

**TL; DR:** Обработка асинхронных ошибок в стиле обратного вызова, вероятно, является самым быстрым путем в ад. Лучший подарок, который вы можете дать своему коду, - это использовать Promise'ы или async-await, что обеспечивает гораздо более компактный и знакомый синтаксис кода, такой как try-catch

**В противном случае:** Стиль обратного вызова Node.js, function(err, response), является многообещающим способом непригодного для использования кода из-за сочетания обработки ошибок со случайным кодом, чрезмерного вложения и неуклюжих шаблонов кодирования.

🔗 [**Read More: избегайте функций обратного вызова**](/sections/errorhandling/asyncerrorhandling.md)

<br/><br/>

## ![✔] 2.2 Используйте встроенный Error object для передачи ошибки

**TL; DR:** Многие выбрасывают ошибки в виде строки или некоторого пользовательского типа - это усложняет логику обработки ошибок и взаимодействие между модулями. Вне зависимости от того, отклоняете ли вы обещание, генерируете исключение или генерируете ошибку - используйте встроенный объект Error, это приведет к повышению однородности и предотвращению потери информации

**В противном случае:** При вызове какого-либо компонента, будучи неуверенным, какой тип ошибок приходит в ответ - это значительно затрудняет правильную обработку ошибок. Хуже того, использование пользовательских типов для описания ошибок может привести к потере информации о критических ошибках, таких как трассировка стека!

🔗 [**Read More: использование объекта Error**](/sections/errorhandling/useonlythebuiltinerror.md)

<br/><br/>

## ![✔] 2.3 Различайте операционные ошибки и ошибки программиста

**TL; DR:** Операционные ошибки (например, API получил неверный ввод) относятся к известным случаям, когда влияние ошибки полностью понимается и может быть обработано вдумчиво. С другой стороны, ошибка программиста (например, попытка прочитать неопределенную переменную) относится к неизвестным ошибкам кода, которые требуют изящного перезапуска приложения.

**В противном случае:** Вы всегда можете перезапустить приложение, когда появляется ошибка, но зачем подводить ~ 5000 онлайн-пользователей из-за незначительной, прогнозируемой, операционной ошибки? обратное также не идеально - поддержание приложения в случае возникновения неизвестной проблемы (ошибка программиста) может привести к непредсказуемому поведению. Разграничение между ними позволяет действовать тактично и применять сбалансированный подход, основанный на данном контексте

🔗 [**Read More: операционные ошибки и ошибки программиста**](/sections/errorhandling/operationalvsprogrammererror.md)

<br/><br/>

## ![✔] 2.4 Обрабатывайте ошибки сентрализованно

**TL; DR:** Логика обработки ошибок, такая как отправка емейла администратору и ведение журнала, должна быть вынесена в выделенный централизованный объект, который вызывается всеми конечными точками (например, Express middleware, задания cron, unit-testing) при возникновении ошибки

**В противном случае:** Если не обрабатывать ошибки в одном месте, это приведет к дублированию кода и, возможно, к неправильной обработке ошибок.

🔗 [**Read More: централизованная обработка ошибок**](/sections/errorhandling/centralizedhandling.md)

<br/><br/>

## ![✔] 2.5 Документируйте возможные типы ошибок в API

**TL; DR:** Пользователи вашего API должны иметь возможность узнать, какие ошибки могут прийти в ответ на запрос. В этом случае они смогут обрабатывать их, что не будет приводить к сбоям. Обычно это делается с помощью фреймворков для REST API документации, таких как Swagger

**В противном случае:** Пользователь API может принять решение о сбое и перезапуске только потому, что он получил ошибку, которую он не мог понять. Примечание: вызывающим абонентом вашего API можете быть и вы (очень типично для микросервисной среды)

🔗 [**Read More: документация ошибок в Swagger**](/sections/errorhandling/documentingusingswagger.md)

<br/><br/>

## ![✔] 2.6 Прервите и перезапустите процесс в случае неизвестной ошибки

**TL; DR:** При возникновении неизвестной ошибки (ошибка разработчика, см. Рекомендацию № 3) - существует неопределенность в отношении работоспособности приложения. Обычная практика предполагает осторожный перезапуск процесса с использованием инструмента-перезагрузчика, такого как Forever и PM2.

**В противном случае:** Когда обнаруживается незнакомое исключение, часть функционала может находиться в неисправном состоянии (например, источник событий, который используется глобально и больше не генерирует события из-за некоторого внутреннего сбоя), и все будущие запросы могут завершаться сбоем или вести себя неправильно

🔗 [**Read More: остановка процесса**](/sections/errorhandling/shuttingtheprocess.md)

<br/><br/>

## ![✔] 2.7 Используйте известные инструменты логирования

**TL; DR:** Использование известных инструментов логирования, таких как Winston, Bunyan или Log4J, ускорит обнаружение и понимание ошибок. Так что забудьте о console.log

**В противном случае:** Дебаг через console.log или вручную через сырой текстовый файл без механизма поиска или удобного форматирования может занять вас на работе до поздней ночи.

🔗 [**Read More: использование инструментов логирования**](/sections/errorhandling/usematurelogger.md)

<br/><br/>

## ![✔] 2.8 Тестируйте обработку ошибок через тестовые фреймворки

**TL; DR:** Будь то профессиональный автоматический QA или простое ручное тестирование разработчиком - убедитесь, что ваш код не только удовлетворяет положительному сценарию, но также обрабатывает и возвращает правильные ошибки. Фреймворки для тестирования, такие как Mocha & Chai, могут легко справиться с этим (см. Примеры кода в «Gist popup»)

**В противном случае:** Без тестирования, будь то автоматически или вручную, вы не можете полагаться на наш код с механизмом возврата правильных ошибок. Без значимых ошибок нет обработки ошибок

🔗 [**Read More: способы тестирования ошибок**](/sections/errorhandling/testingerrorflows.md)

<br/><br/>

## ![✔] 2.9 Обнаружение ошибок и простоев используя APM

**TL; DR:** Сервисы для мониторинга состояния и производительности (к прим. APM) проактивно измеряют вашу кодовую базу или API, чтобы они могли автоматически выделять ошибки, сбои и медленные части, которые вы пропустили

**В противном случае:** Вы можете потратить огромные усилия на измерение производительности и времени простоя API, возможно, вы никогда не узнаете, какие ваши самые медленные части кода в реальном сценарии и как они влияют на UX

🔗 [**Read More: использование APM**](/sections/errorhandling/apmproducts.md)

<br/><br/>

## ![✔] 2.10 Обрабатывайте все исключения Promise

**TL; DR:** Любое исключение, отправленное в Promice, будет отброшено, если разработчик не забудет его явно обработать. Даже если ваш код подписан на ]`process.uncaughtException`! Исправьте это, подписавшись на событие `process.unhandledRejection`

**В противном случае:** Ваши ошибки будут утеряны и не оставят следов. Не о чем беспокоиться

🔗 [**Read More: обработка неизсвестных ошибок Promise**](/sections/errorhandling/catchunhandledpromiserejection.md)

<br/><br/>

## ![✔] 2.11 Проверяйте входные данные вашего API

**TL; DR:** Это должно быть частью вашей лучшей практики Express - проверка входных данных API, чтобы избежать неприятных ошибок, которые потом будет намного сложнее отследить. Код проверки обычно утомителен, если вы не используете очень классную вспомогательную библиотеку, такую как Joi

**В противном случае:** Это может привести к непредвиденным ошибкам. К примеру - API ожидает числовой аргумент `Discount`, который вызывающая сторона забывает передать, позже ваш код проверяет наличине ненулевой скиди (`Discount != 0`) и позволяет пользователю ей пользоваться.

🔗 [**Read More: проверка входных данных**](/sections/errorhandling/failfast.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `3. Стиль Кода`

## ![✔] 3.1 Используйте ESLint

**TL;DR:** [ESLint](https://eslint.org) ялвяется стандартом де-факто для проверки кода на возможные ошибки и исправления ошибок стиля кода. ESLint используется не только для исправления неправильных отступов, но и для выявления таких серьезных "анти-паттерных" проблем, как, к примеру, выброс ошибок без классификации. Несмотря на то, что ESLint поддерживает автоматическое исправление кода, такие утилиты как [prettier](https://www.npmjs.com/package/prettier) и [beautify](https://www.npmjs.com/package/js-beautify) сделают это более продуктивно, если их использовать совместно с ESLint.

**В противном случае:** Код не будет иметь единого стилистического подхода

<br/><br/>

## ![✔] 3.2 Node.js-ориентированные линтеры

**TL;DR:** В дополнение к стандартным правилам ESLint, которые покрывают только  Vanilla JS, используйте такие Node-ориентированные плагины, как [eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node), [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) и [eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security)

**В противном случае:** Ряд проблем паттернов кода Node.js могут быть упущены. Как пример, разработчик может использовать `require(variableAsPath)` с переменной, указанной в качестве пути, которая позволяет злоумышленникам выполнить любой сценарий JS. Линтеры Node.js могут обнаружить такие паттерны и известить о проблеме

<br/><br/>

## ![✔] 3.3 Фигурная скоба в той же строке

**TL;DR:** Открывающая фигурная скобка должна находиться в той же строке, что и объявление метода

### Пример кода

```javascript
// Do
function someFunction() {
  // code block
}

// Avoid
function someFunction()
{
  // code block
}
```

**В противном случае:** Отклонение от общей практики может привести неожиданным последствиям. Детальнее об этом можно прочитать в треде на StackOverflow:

🔗 [**Read more:** "Why does a results vary based on curly brace placement?" (Stackoverflow)](https://stackoverflow.com/questions/3641519/why-does-a-results-vary-based-on-curly-brace-placement)

<br/><br/>

## ![✔] 3.4 Не забывайте о точке с запятой

**TL;DR:** Даже с учетом, что нет единого мнения, все же рекомендуется ставить точку с запятой в конце каждого объявления. Это делает ваш код более читабельным и понятным для других разработчиков

**В противном случае:** Так же, как и в примере выше, интерпритатор JavaScript  добавляет точку с запятой в конец объявления, где его нет. Но так же он может посчитать, что объявление еще не завершено, что может привести к нежелательным последствиям

### Пример кода

```javascript
// Do
const count = 2;
(function doSomething() {
  // do something amazing
}());

// Avoid — throws exception
const count = 2 // it tries to run 2(), but 2 is not a function
(function doSomething() {
  // do something amazing
}())
```

<br/><br/>

## ![✔] 3.5 Именуйте функции


**TL;DR:** Именуйте все функциии, включая замыкания и функции обратного вызова. Игнорируйте анонимные функции. Это особенно полезно при профилировании Node.js приложений. Именование всех функций сильно упрощает разбор снимка памяти

**В противном случае:** Отладка производительности используя снимок памяти станет гораздо сложнее, когда основную часть памяти потребляют анонимные функции

<br/><br/>

## ![✔] 3.6 Соглашения об именах переменных, констант, функций и классов

**TL;DR:** Используйте **_lowerCamelCase_** когда именуете константы, переменные и функции и **_UpperCamelCase_** (первая также в верхнем регистре) когда именуете классы. Это поможет вам легко различать простые переменные/функции и классы. Используйте описательные имена, но старайтесь, чтобы они были короткими

**В противном случае:** Javascript - единственный язык в мире, который позволяет напрямую вызывать конструктор («Class»), не создавая его экземпляры. Следовательно, классы и конструкторы функций различаются, начиная с UpperCamelCase

### Пример кода

```javascript
// for class name we use UpperCamelCase
class SomeClassExample {}

// for const names we use the const keyword and lowerCamelCase
const config = {
  key: 'value'
};

// for variables and functions names we use lowerCamelCase
let someVariableExample = 'value';
function doSomething() {}
```

<br/><br/>

## ![✔] 3.7 Prefer const over let. Ditch the var

**TL;DR:** Using `const` means that once a variable is assigned, it cannot be reassigned. Preferring const will help you to not be tempted to use the same variable for different uses, and make your code clearer. If a variable needs to be reassigned, in a for loop, for example, use `let` to declare it. Another important aspect of `let` is that a variable declared using it is only available in the block scope in which it was defined. `var` is function scoped, not block scoped, and [shouldn't be used in ES6](https://hackernoon.com/why-you-shouldnt-use-var-anymore-f109a58b9b70) now that you have const and let at your disposal

**Otherwise:** Debugging becomes way more cumbersome when following a variable that frequently changes

🔗 [**Read more: JavaScript ES6+: var, let, or const?** ](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

<br/><br/>

## ![✔] 3.8 Requires come first, and not inside functions

**TL;DR:** Require modules at the beginning of each file, before and outside of any functions. This simple best practice will not only help you easily and quickly tell the dependencies of a file right at the top but also avoids a couple of potential problems

**Otherwise:** Requires are run synchronously by Node.js. If they are called from within a function, it may block other requests from being handled at a more critical time. Also, if a required module or any of its own dependencies throw an error and crash the server, it is best to find out about it as soon as possible, which might not be the case if that module is required from within a function

<br/><br/>

## ![✔] 3.9 Do Require on the folders, not directly on the files

**TL;DR:** When developing a module/library in a folder, place an index.js file that exposes the module's
internals so every consumer will pass through it. This serves as an 'interface' to your module and eases
future changes without breaking the contract

**Otherwise:** Changing the internal structure of files or the signature may break the interface with
clients

### Code example

```javascript
// Do
module.exports.SMSProvider = require('./SMSProvider');
module.exports.SMSNumberResolver = require('./SMSNumberResolver');

// Avoid
module.exports.SMSProvider = require('./SMSProvider/SMSProvider.js');
module.exports.SMSNumberResolver = require('./SMSNumberResolver/SMSNumberResolver.js');
```

<br/><br/>

## ![✔] 3.10 Use the `===` operator

**TL;DR:** Prefer the strict equality operator `===` over the weaker abstract equality operator `==`. `==` will compare two variables after converting them to a common type. There is no type conversion in `===`, and both variables must be of the same type to be equal

**Otherwise:** Unequal variables might return true when compared with the `==` operator

### Code example

```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```

All statements above will return false if used with `===`

<br/><br/>

## ![✔] 3.11 Use Async Await, avoid callbacks

**TL;DR:** Node 8 LTS now has full support for Async-await. This is a new way of dealing with asynchronous code which supersedes callbacks and promises. Async-await is non-blocking, and it makes asynchronous code look synchronous. The best gift you can give to your code is using async-await which provides a much more compact and familiar code syntax like try-catch

**Otherwise:** Handling async errors in callback style is probably the fastest way to hell - this style forces to check errors all over, deal with awkward code nesting and make it difficult to reason about the code flow

🔗[**Read more:** Guide to async await 1.0](https://github.com/yortus/asyncawait)

<br/><br/>

## ![✔] 3.12 Use Fat (=>) Arrow Functions

**TL;DR:** Though it's recommended to use async-await and avoid function parameters when dealing with older API that accept promises or callbacks - arrow functions make the code structure more compact and keep the lexical context of the root function (i.e. 'this')

**Otherwise:** Longer code (in ES5 functions) is more prone to bugs and cumbersome to read

🔗 [**Read mode: It’s Time to Embrace Arrow Functions**](https://medium.com/javascript-scene/familiarity-bias-is-holding-you-back-its-time-to-embrace-arrow-functions-3d37e1a9bb75)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `4. Практики Тестирования И Поддержания Качества`

## ![✔] 4.1 At the very least, write API (component) testing

**TL;DR:** Most projects just don't have any automated testing due to short timetables or often the 'testing project' run out of control and being abandoned. For that reason, prioritize and start with API testing which is the easiest to write and provide more coverage than unit testing (you may even craft API tests without code using tools like [Postman](https://www.getpostman.com/). Afterward, should you have more resources and time, continue with advanced test types like unit testing, DB testing, performance testing, etc

**Otherwise:** You may spend long days on writing unit tests to find out that you got only 20% system coverage

<br/><br/>

## ![✔] 4.2 Detect code issues with a linter

**TL;DR:** Use a code linter to check basic quality and detect anti-patterns early. Run it before any test and add it as a pre-commit git-hook to minimize the time needed to review and correct any issue. Also check [Section 3](https://github.com/i0natan/nodebestpractices#3-code-style-practices) on Code Style Practices

**Otherwise:** You may let pass some anti-pattern and possible vulnerable code to your production environment.

<br/><br/>

## ![✔] 4.3 Carefully choose your CI platform (Jenkins vs CircleCI vs Travis vs Rest of the world)

**TL;DR:** Your continuous integration platform (CICD) will host all the quality tools (e.g test, lint) so it should come with a vibrant ecosystem of plugins. [Jenkins](https://jenkins.io/) used to be the default for many projects as it has the biggest community along with a very powerful platform at the price of complex setup that demands a steep learning curve. Nowadays, it became much easier to set up a CI solution using SaaS tools like [CircleCI](https://circleci.com) and others. These tools allow crafting a flexible CI pipeline without the burden of managing the whole infrastructure. Eventually, it's a trade-off between robustness and speed - choose your side carefully

**Otherwise:** Choosing some niche vendor might get you blocked once you need some advanced customization. On the other hand, going with Jenkins might burn precious time on infrastructure setup

🔗 [**Read More: Choosing CI platform**](/sections/testingandquality/citools.md)

<br/><br/>

## ![✔] 4.4 Constantly inspect for vulnerable dependencies

**TL;DR:** Even the most reputable dependencies such as Express have known vulnerabilities. This can get easily tamed using community and commercial tools such as 🔗 [npm audit](https://docs.npmjs.com/cli/audit) and 🔗 [snyk.io](https://snyk.io) that can be invoked from your CI on every build

**Otherwise:** Keeping your code clean from vulnerabilities without dedicated tools will require to constantly follow online publications about new threats. Quite tedious

<br/><br/>

## ![✔] 4.5 Tag your tests

**TL;DR:** Different tests must run on different scenarios: quick smoke, IO-less, tests should run when a developer saves or commits a file, full end-to-end tests usually run when a new pull request is submitted, etc. This can be achieved by tagging tests with keywords like #cold #api #sanity so you can grep with your testing harness and invoke the desired subset. For example, this is how you would invoke only the sanity test group with [Mocha](https://mochajs.org/): mocha --grep 'sanity'

**Otherwise:** Running all the tests, including tests that perform dozens of DB queries, any time a developer makes a small change can be extremely slow and keeps developers away from running tests

<br/><br/>

## ![✔] 4.6 Check your test coverage, it helps to identify wrong test patterns

**TL;DR:** Code coverage tools like [Istanbul/NYC ](https://github.com/gotwarlost/istanbul)are great for 3 reasons: it comes for free (no effort is required to benefit this reports), it helps to identify a decrease in testing coverage, and last but not least it highlights testing mismatches: by looking at colored code coverage reports you may notice, for example, code areas that are never tested like catch clauses (meaning that tests only invoke the happy paths and not how the app behaves on errors). Set it to fail builds if the coverage falls under a certain threshold

**Otherwise:** There won't be any automated metric telling you when a large portion of your code is not covered by testing

<br/><br/>

## ![✔] 4.7 Inspect for outdated packages

**TL;DR:** Use your preferred tool (e.g. 'npm outdated' or [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) to detect installed packages which are outdated, inject this check into your CI pipeline and even make a build fail in a severe scenario. For example, a severe scenario might be when an installed package is 5 patch commits behind (e.g. local version is 1.3.1 and repository version is 1.3.8) or it is tagged as deprecated by its author - kill the build and prevent deploying this version

**Otherwise:** Your production will run packages that have been explicitly tagged by their author as risky

<br/><br/>

## ![✔] 4.8 Use docker-compose for e2e testing

**TL;DR:** End to end (e2e) testing which includes live data used to be the weakest link of the CI process as it depends on multiple heavy services like DB. Docker-compose turns this problem into a breeze by crafting production-like environment using a simple text file and easy commands. It allows crafting all the dependent services, DB and isolated network for e2e testing. Last but not least, it can keep a stateless environment that is invoked before each test suite and dies right after

**Otherwise:** Without docker-compose teams must maintain a testing DB for each testing environment including developers machines, keep all those DBs in sync so test results won't vary across environments

<br/><br/>

## ![✔] 4.9 Refactor regularly using static analysis tools

**TL;DR:** Using static analysis tools helps by giving objective ways to improve code quality and keep your code maintainable. You can add static analysis tools to your CI build to fail when it finds code smells. Its main selling points over plain linting are the ability to inspect quality in the context of multiple files (e.g. detect duplications), perform advanced analysis (e.g. code complexity) and follow the history and progress of code issues. Two examples of tools you can use are [Sonarqube](https://www.sonarqube.org/) (2,600+ [stars](https://github.com/SonarSource/sonarqube)) and [Code Climate](https://codeclimate.com/) (1,500+ [stars](https://github.com/codeclimate/codeclimate)).

**Otherwise:** With poor code quality, bugs and performance will always be an issue that no shiny new library or state of the art features can fix.

🔗 [**Read More: Refactoring!**](/sections/testingandquality/refactoring.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `5. Практики Релиза`

## ![✔] 5.1. Monitoring!

**TL;DR:** Monitoring is a game of finding out issues before customers do – obviously this should be assigned unprecedented importance. The market is overwhelmed with offers thus consider starting with defining the basic metrics you must follow (my suggestions inside), then go over additional fancy features and choose the solution that ticks all boxes. Click ‘The Gist’ below for an overview of the solutions

**Otherwise:** Failure === disappointed customers. Simple

🔗 [**Read More: Monitoring!**](/sections/production/monitoring.md)

<br/><br/>

## ![✔] 5.2. Increase transparency using smart logging

**TL;DR:** Logs can be a dumb warehouse of debug statements or the enabler of a beautiful dashboard that tells the story of your app. Plan your logging platform from day 1: how logs are collected, stored and analyzed to ensure that the desired information (e.g. error rate, following an entire transaction through services and servers, etc) can really be extracted

**Otherwise:** You end-up with a black box that is hard to reason about, then you start re-writing all logging statements to add additional information

🔗 [**Read More: Increase transparency using smart logging**](/sections/production/smartlogging.md)

<br/><br/>

## ![✔] 5.3. Delegate anything possible (e.g. gzip, SSL) to a reverse proxy

**TL;DR:** Node is awfully bad at doing CPU intensive tasks like gzipping, SSL termination, etc. You should use ‘real’ middleware services like nginx, HAproxy or cloud vendor services instead

**Otherwise:** Your poor single thread will stay busy doing infrastructural tasks instead of dealing with your application core and performance will degrade accordingly

🔗 [**Read More: Delegate anything possible (e.g. gzip, SSL) to a reverse proxy**](/sections/production/delegatetoproxy.md)

<br/><br/>

## ![✔] 5.4. Lock dependencies

**TL;DR:** Your code must be identical across all environments, but amazingly npm lets dependencies drift across environments by default – when you install packages at various environments it tries to fetch packages’ latest patch version. Overcome this by using npm config files, .npmrc, that tell each environment to save the exact (not the latest) version of each package. Alternatively, for finer grain control use npm” shrinkwrap”. \*Update: as of NPM5, dependencies are locked by default. The new package manager in town, Yarn, also got us covered by default

**Otherwise:** QA will thoroughly test the code and approve a version that will behave differently at production. Even worse, different servers at the same production cluster might run different code

🔗 [**Read More: Lock dependencies**](/sections/production/lockdependencies.md)

<br/><br/>

## ![✔] 5.5. Guard process uptime using the right tool

**TL;DR:** The process must go on and get restarted upon failures. For simple scenarios, ‘restarter’ tools like PM2 might be enough but in today ‘dockerized’ world – a cluster management tools should be considered as well

**Otherwise:** Running dozens of instances without a clear strategy and too many tools together (cluster management, docker, PM2) might lead to a DevOps chaos

🔗 [**Read More: Guard process uptime using the right tool**](/sections/production/guardprocess.md)

<br/><br/>

## ![✔] 5.6. Utilize all CPU cores

**TL;DR:** At its basic form, a Node app runs on a single CPU core while all other are left idling. It’s your duty to replicate the Node process and utilize all CPUs – For small-medium apps you may use Node Cluster or PM2. For a larger app consider replicating the process using some Docker cluster (e.g. K8S, ECS) or deployment scripts that are based on Linux init system (e.g. systemd)

**Otherwise:** Your app will likely utilize only 25% of its available resources(!) or even less. Note that a typical server has 4 CPU cores or more, naive deployment of Node.js utilizes only 1 (even using PaaS services like AWS beanstalk!)

🔗 [**Read More: Utilize all CPU cores**](/sections/production/utilizecpu.md)

<br/><br/>

## ![✔] 5.7. Create a ‘maintenance endpoint’

**TL;DR:** Expose a set of system-related information, like memory usage and REPL, etc in a secured API. Although it’s highly recommended to rely on standard and battle-tests tools, some valuable information and operations are easier done using code

**Otherwise:** You’ll find that you’re performing many “diagnostic deploys” – shipping code to production only to extract some information for diagnostic purposes

🔗 [**Read More: Create a ‘maintenance endpoint’**](/sections/production/createmaintenanceendpoint.md)

<br/><br/>

## ![✔] 5.8. Discover errors and downtime using APM products

**TL;DR:** Monitoring and performance products (a.k.a APM) proactively gauge codebase and API so they can auto-magically go beyond traditional monitoring and measure the overall user-experience across services and tiers. For example, some APM products can highlight a transaction that loads too slow on the end-users side while suggesting the root cause

**Otherwise:** You might spend great effort on measuring API performance and downtimes, probably you’ll never be aware which is your slowest code parts under real-world scenario and how these affects the UX

🔗 [**Read More: Discover errors and downtime using APM products**](/sections/production/apmproducts.md)

<br/><br/>

## ![✔] 5.9. Make your code production-ready

**TL;DR:** Code with the end in mind, plan for production from day 1. This sounds a bit vague so I’ve compiled a few development tips that are closely related to production maintenance (click Gist below)

**Otherwise:** A world champion IT/DevOps guy won’t save a system that is badly written

🔗 [**Read More: Make your code production-ready**](/sections/production/productioncode.md)

<br/><br/>

## ![✔] 5.10. Measure and guard the memory usage

**TL;DR:** Node.js has controversial relationships with memory: the v8 engine has soft limits on memory usage (1.4GB) and there are known paths to leaks memory in Node’s code – thus watching Node’s process memory is a must. In small apps, you may gauge memory periodically using shell commands but in medium-large app consider baking your memory watch into a robust monitoring system

**Otherwise:** Your process memory might leak a hundred megabytes a day like how it happened at [Walmart](https://www.joyent.com/blog/walmart-node-js-memory-leak)

🔗 [**Read More: Measure and guard the memory usage**](/sections/production/measurememory.md)

<br/><br/>

## ![✔] 5.11. Get your frontend assets out of Node

**TL;DR:** Serve frontend content using dedicated middleware (nginx, S3, CDN) because Node performance really gets hurt when dealing with many static files due to its single threaded model

**Otherwise:** Your single Node thread will be busy streaming hundreds of html/images/angular/react files instead of allocating all its resources for the task it was born for – serving dynamic content

🔗 [**Read More: Get your frontend assets out of Node**](/sections/production/frontendout.md)

<br/><br/>

## ![✔] 5.12. Be stateless, kill your Servers almost every day

**TL;DR:** Store any type of data (e.g. users session, cache, uploaded files) within external data stores. Consider ‘killing’ your servers periodically or use ‘serverless’ platform (e.g. AWS Lambda) that explicitly enforces a stateless behavior

**Otherwise:** Failure at a given server will result in application downtime instead of just killing a faulty machine. Moreover, scaling-out elasticity will get more challenging due to the reliance on a specific server

🔗 [**Read More: Be stateless, kill your Servers almost every day**](/sections/production/bestateless.md)

<br/><br/>

## ![✔] 5.13. Use tools that automatically detect vulnerabilities

**TL;DR:** Even the most reputable dependencies such as Express have known vulnerabilities (from time to time) that can put a system at risk. This can get easily tamed using community and commercial tools that constantly check for vulnerabilities and warn (locally or at GitHub), some can even patch them immediately

**Otherwise:** Keeping your code clean from vulnerabilities without dedicated tools will require to constantly follow online publications about new threats. Quite tedious

🔗 [**Read More: Use tools that automatically detect vulnerabilities**](/sections/production/detectvulnerabilities.md)

<br/><br/>

## ![✔] 5.14. Assign ‘TransactionId’ to each log statement

**TL;DR:** Assign the same identifier, transaction-id: {some value}, to each log entry within a single request. Then when inspecting errors in logs, easily conclude what happened before and after. Unfortunately, this is not easy to achieve in Node due to its async nature, see code examples inside

**Otherwise:** Looking at a production error log without the context – what happened before – makes it much harder and slower to reason about the issue

🔗 [**Read More: Assign ‘TransactionId’ to each log statement**](/sections/production/assigntransactionid.md)

<br/><br/>

## ![✔] 5.15. Set NODE_ENV=production

**TL;DR:** Set the environment variable NODE_ENV to ‘production’ or ‘development’ to flag whether production optimizations should get activated – many npm packages determining the current environment and optimize their code for production

**Otherwise:** Omitting this simple property might greatly degrade performance. For example, when using Express for server-side rendering omitting `NODE_ENV` makes the slower by a factor of three!

🔗 [**Read More: Set NODE_ENV=production**](/sections/production/setnodeenv.md)

<br/><br/>

## ![✔] 5.16. Design automated, atomic and zero-downtime deployments

**TL;DR:** Researches show that teams who perform many deployments – lowers the probability of severe production issues. Fast and automated deployments that don’t require risky manual steps and service downtime significantly improves the deployment process. You should probably achieve that using Docker combined with CI tools as they became the industry standard for streamlined deployment

**Otherwise:** Long deployments -> production down time & human-related error -> team unconfident and in making deployment -> less deployments and features

<br/><br/>

## ![✔] 5.17. Use an LTS release of Node.js

**TL;DR:** Ensure you are using an LTS version of Node.js to receive critical bug fixes, security updates and performance improvements

**Otherwise:** Newly discovered bugs or vulnerabilities could be used to exploit an application running in production, and your application may become unsupported by various modules and harder to maintain

🔗 [**Read More: Use an LTS release of Node.js**](/sections/production/LTSrelease.md)

<br/><br/>

## ![✔] 5.18. Don't route logs within the app

**TL;DR:** Log destinations should not be hard-coded by developers within the application code, but instead should be defined by the execution environment the application runs in. Developers should write logs to `stdout` using a logger utility and then let the execution environment (container, server, etc.) pipe the `stdout` stream to the appropriate destination (i.e. Splunk, Graylog, ElasticSearch, etc.).

**Otherwise:** Application handling log routing === hard to scale, loss of logs, poor separation of concerns

🔗 [**Read More: Log Routing**](/sections/production/logrouting.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `6. Лучшие Практики По Безопасности`

<div align="center">
<img src="https://img.shields.io/badge/OWASP%20Threats-Top%2010-green.svg" alt="53 items"/>
</div>

## ![✔] 6.1. Embrace linter security rules

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20XSS%20-green.svg" alt=""/></a>

**TL;DR:** Make use of security-related linter plugins such as [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security) to catch security vulnerabilities and issues as early as possible , at best  while they're being coded. This can help catching security weaknesses like using eval, invoking a child process or importing a module with a string literal (e.g. user input). Click 'Read more' below to see code examples that will get caught by a security linter

**Otherwise:** What could have been a straightforward security weakness during development becomes a major issue in production. Also, the project may not follow consistent code security practices, leading to vulnerabilities being introduced, or sensitive secrets committed into remote repositories

🔗 [**Read More: Lint rules**](/sections/security/lintrules.md)

<br/><br/>

## ![✔] 6.2. Limit concurrent requests using a middleware

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** DOS attacks are very popular and relatively easy to conduct. Implement rate limiting using an external service such as cloud load balancers, cloud firewalls, nginx, or (for smaller and less critical apps) a rate limiting middleware (e.g. [express-rate-limit](https://www.npmjs.com/package/express-rate-limit))

**Otherwise:** An application could be subject to an attack resulting in a denial of service where real users receive a degraded or unavailable service.

🔗 [**Read More: Implement rate limiting**](/sections/security/limitrequests.md)

<br/><br/>

## ![✔] 6.3 Extract secrets from config files or use packages to encrypt them

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A3:Sensitive%20Data%20Exposure%20-green.svg" alt=""/></a>

**TL;DR:** Never store plain-text secrets in configuration files or source code. Instead, make use of secret-management systems like Vault products, Kubernetes/Docker Secrets, or using environment variables. As a last result, secrets stored in source control must be encrypted and managed (rolling keys, expiring, auditing, etc). Make use of pre-commit/push hooks to prevent committing secrets accidentally

**Otherwise:** Source control, even for private repositories, can mistakenly be made public, at which point all secrets are exposed. Access to source control for an external party will inadvertently provide access to related systems (databases, apis, services, etc).

🔗 [**Read More: Secret management**](/sections/security/secretmanagement.md)

<br/><br/>

## ![✔] 6.4. Prevent query injection vulnerabilities with ORM/ODM libraries

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** To prevent SQL/NoSQL injection and other malicious attacks, always make use of an ORM/ODM or a database library that escapes data or supports named or indexed parameterized queries, and takes care of validating user input for expected types. Never just use JavaScript template strings or string concatenation to inject values into queries as this opens your application to a wide spectrum of vulnerabilities. All the reputable Node.js data access libraries (e.g. [Sequelize](https://github.com/sequelize/sequelize), [Knex](https://github.com/tgriesser/knex), [mongoose](https://github.com/Automattic/mongoose)) have built-in protection against injection attacks.

**Otherwise:** Unvalidated or unsanitized user input could lead to operator injection when working with MongoDB for NoSQL, and not using a proper sanitization system or ORM will easily allow SQL injection attacks, creating a giant vulnerability.

🔗 [**Read More: Query injection prevention using ORM/ODM libraries**](/sections/security/ormodmusage.md)

<br/><br/>

## ![✔] 6.5. Collection of generic security best practices

**TL;DR:** This is a collection of security advice that are not related directly to Node.js - the Node implementation is not much different than any other language. Click read more to skim through.

🔗 [**Read More: Common security best practices**](/sections/security/commonsecuritybestpractices.md)

<br/><br/>

## ![✔] 6.6. Adjust the HTTP response headers for enhanced security

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Your application should be using secure headers to prevent attackers from using common attacks like cross-site scripting (XSS), clickjacking and other malicious attacks. These can be configured easily using modules like [helmet](https://www.npmjs.com/package/helmet).

**Otherwise:** Attackers could perform direct attacks on your application's users, leading huge security vulnerabilities

🔗 [**Read More: Using secure headers in your application**](/sections/security/secureheaders.md)

<br/><br/>

## ![✔] 6.7. Constantly and automatically inspect for vulnerable dependencies

<a href="https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Known%20Vulnerabilities%20-green.svg" alt=""/></a>

**TL;DR:** With the npm ecosystem it is common to have many dependencies for a project. Dependencies should always be kept in check as new vulnerabilities are found. Use tools like [npm audit](https://docs.npmjs.com/cli/audit) or [snyk](https://snyk.io/) to track, monitor and patch vulnerable dependencies. Integrate these tools with your CI setup so you catch a vulnerable dependency before it makes it to production.

**Otherwise:** An attacker could detect your web framework and attack all its known vulnerabilities.

🔗 [**Read More: Dependency security**](/sections/security/dependencysecurity.md)

<br/><br/>

## ![✔] 6.8. Avoid using the Node.js crypto library for handling passwords, use Bcrypt

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** Passwords or secrets (API keys) should be stored using a secure hash + salt function like `bcrypt`, that should be a preferred choice over its JavaScript implementation due to performance and security reasons.

**Otherwise:** Passwords or secrets that are persisted without using a secure function are vulnerable to brute forcing and dictionary attacks that will lead to their disclosure eventually.

🔗 [**Read More: Use Bcrypt**](/sections/security/bcryptpasswords.md)

<br/><br/>

## ![✔] 6.9. Escape HTML, JS and CSS output

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a>

**TL;DR:** Untrusted data that is sent down to the browser might get executed instead of just being displayed, this is commonly being referred as a cross-site-scripting (XSS) attack. Mitigate this by using dedicated libraries that explicitly mark the data as pure content that should never get executed (i.e. encoding, escaping)

**Otherwise:** An attacker might store a malicious JavaScript code in your DB which will then be sent as-is to the poor clients

🔗 [**Read More: Escape output**](/sections/security/escape-output.md)

<br/><br/>

## ![✔] 6.10. Validate incoming JSON schemas

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7: XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a>

**TL;DR:** Validate the incoming requests' body payload and ensure it qualifies the expectations, fail fast if it doesn't. To avoid tedious validation coding within each route you may use lightweight JSON-based validation schemas such as [jsonschema](https://www.npmjs.com/package/jsonschema) or [joi](https://www.npmjs.com/package/joi)

**Otherwise:** Your generosity and permissive approach greatly increases the attack surface and encourages the attacker to try out many inputs until they find some combination to crash the application

🔗 [**Read More: Validate incoming JSON schemas**](/sections/security/validation.md)

<br/><br/>

## ![✔] 6.11. Support blacklisting JWTs

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** When using JSON Web Tokens (for example, with [Passport.js](https://github.com/jaredhanson/passport)), by default there's no mechanism to revoke access from issued tokens. Once you discover some malicious user activity, there's no way to stop them from accessing the system as long as they hold a valid token. Mitigate this by implementing a blacklist of untrusted tokens that are validated on each request.

**Otherwise:** Expired, or misplaced tokens could be used maliciously by a third party to access an application and impersonate the owner of the token.

🔗 [**Read More: Blacklist JSON Web Tokens**](/sections/security/expirejwt.md)

<br/><br/>

## ![✔] 6.12. Limit the allowed login requests of each user

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** A brute force protection middleware such as [express-brute](https://www.npmjs.com/package/express-brute) should be used inside an express application to prevent brute force/dictionary attacks on sensitive routes such as /admin or /login based on request properties such as the user name, or other identifiers such as body parameters

**Otherwise:** An attacker can issue unlimited automated password attempts to gain access to privileged accounts on an application

🔗 [**Read More: Login rate limiting**](/sections/security/login-rate-limit.md)

<br/><br/>

## ![✔] 6.13. Run Node.js as non-root user

<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A5:Broken%20Access%20Access%20Control-green.svg" alt=""/></a>

**TL;DR:** There is a common scenario where Node.js runs as a root user with unlimited permissions. For example, this is the default behaviour in Docker containers. It's recommended to create a non-root user and either bake it into the Docker image (examples given below) or run the process on this users' behalf by invoking the container with the flag "-u username"

**Otherwise:** An attacker who manages to run a script on the server gets unlimited power over the local machine (e.g. change iptable and re-route traffic to his server)

🔗 [**Read More: Run Node.js as non-root user**](/sections/security/non-root-user.md)

<br/><br/>

## ![✔] 6.14. Limit payload size using a reverse-proxy or a middleware

<a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** The bigger the body payload is, the harder your single thread works in processing it. This is an opportunity for attackers to bring servers to their knees without tremendous amount of requests (DOS/DDOS attacks). Mitigate this limiting the body size of incoming requests on the edge (e.g. firewall, ELB) or by configuring [express body parser](https://github.com/expressjs/body-parser) to accept only small-size payloads

**Otherwise:** Your application will have to deal with large requests, unable to process the other important work it has to accomplish, leading to performance implications and vulnerability towards DOS attacks

🔗 [**Read More: Limit payload size**](/sections/security/requestpayloadsizelimit.md)

<br/><br/>

## ![✔] 6.15. Avoid JavaScript eval statements

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** `eval` is evil as it allows executing a custom JavaScript code during run time. This is not just a performance concern but also an important security concern due to malicious JavaScript code that may be sourced from user input. Another language feature that should be avoided is `new Function` constructor. `setTimeout` and `setInterval` should never be passed dynamic JavaScript code either.

**Otherwise:** Malicious JavaScript code finds a way into a text passed into `eval` or other real-time evaluating JavaScript language functions, and will gain complete access to JavaScript permissions on the page. This vulnerability is often manifested as an XSS attack.

🔗 [**Read More: Avoid JavaScript eval statements**](/sections/security/avoideval.md)

<br/><br/>

## ![✔] 6.16. Prevent evil RegEx from overloading your single thread execution

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** Regular Expressions, while being handy, pose a real threat to JavaScript applications at large, and the Node.js platform in particular. A user input for text to match might require an outstanding amount of CPU cycles to process. RegEx processing might be inefficient to an extent that a single request that validates 10 words can block the entire event loop for 6 seconds and set the CPU on 🔥. For that reason, prefer third-party validation packages like [validator.js](https://github.com/chriso/validator.js) instead of writing your own Regex patterns, or make use of [safe-regex](https://github.com/substack/safe-regex) to detect vulnerable regex patterns

**Otherwise:** Poorly written regexes could be susceptible to Regular Expression DoS attacks that will block the event loop completely. For example, the popular `moment` package was found vulnerable with malicious RegEx usage in November of 2017

🔗 [**Read More: Prevent malicious RegEx**](/sections/security/regex.md)

<br/><br/>

## ![✔] 6.17. Avoid module loading using a variable

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** Avoid requiring/importing another file with a path that was given as parameter due to the concern that it could have originated from user input. This rule can be extended for accessing files in general (i.e. `fs.readFile()`) or other sensitive resource access with dynamic variables originating from user input. [Eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security) linter can catch such patterns and warn early enough

**Otherwise:** Malicious user input could find its way to a parameter that is used to require tampered files, for example a previously uploaded file on the filesystem, or access already existing system files.

🔗 [**Read More: Safe module loading**](/sections/security/safemoduleloading.md)

<br/><br/>

## ![✔] 6.18. Run unsafe code in a sandbox

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** When tasked to run external code that is given at run-time (e.g. plugin), use any sort of 'sandbox' execution environment that isolates and guards the main code against the plugin. This can be achieved using a dedicated process (e.g. cluster.fork()), serverless environment or dedicated npm packages that acting as a sandbox

**Otherwise:** A plugin can attack through an endless variety of options like infinite loops, memory overloading, and access to sensitive process environment variables

🔗 [**Read More: Run unsafe code in a sandbox**](/sections/security/sandbox.md)

<br/><br/>

## ![✔] 6.19. Take extra care when working with child processes

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** Avoid using child processes when possible and validate and sanitize input to mitigate shell injection attacks if you still have to. Prefer using `child_process.execFile` which by definition will only execute a single command with a set of attributes and will not allow shell parameter expansion.

**Otherwise:** Naive use of child processes could result in remote command execution or shell injection attacks due to malicious user input passed to an unsanitized system command.

🔗 [**Read More: Be cautious when working with child processes**](/sections/security/childprocesses.md)

<br/><br/>

## ![✔] 6.20. Hide error details from clients

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** An integrated express error handler hides the error details by default. However, great are the chances that you implement your own error handling logic with custom Error objects (considered by many as a best practice). If you do so, ensure not to return the entire Error object to the client, which might contain some sensitive application details

**Otherwise:** Sensitive application details such as server file paths, third party modules in use, and other internal workflows of the application which could be exploited by an attacker, could be leaked from information found in a stack trace

🔗 [**Read More: Hide error details from client**](/sections/security/hideerrors.md)

<br/><br/>

## ![✔] 6.21. Configure 2FA for npm or Yarn

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Any step in the development chain should be protected with MFA (multi-factor authentication), npm/Yarn are a sweet opportunity for attackers who can get their hands on some developer's password. Using developer credentials, attackers can inject malicious code into libraries that are widely installed across projects and services. Maybe even across the web if published in public. Enabling 2-factor-authentication in npm leaves almost zero chances for attackers to alter your package code.

**Otherwise:** [Have you heard about the eslint developer who's password was hijacked?](https://medium.com/@oprearocks/eslint-backdoor-what-it-is-and-how-to-fix-the-issue-221f58f1a8c8)

<br/><br/>

## ![✔] 6.22. Modify session middleware settings

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** Each web framework and technology has its known weaknesses - telling an attacker which web framework we use is a great help for them. Using the default settings for session middlewares can expose your app to module- and framework-specific hijacking attacks in a similar way to the `X-Powered-By` header. Try hiding anything that identifies and reveals your tech stack (E.g. Node.js, express)

**Otherwise:** Cookies could be sent over insecure connections, and an attacker might use session identification to identify the underlying framework of the web application, as well as module-specific vulnerabilities

🔗 [**Read More: Cookie and session security**](/sections/security/sessions.md)

<br/><br/>

## ![✔] 6.23. Avoid DOS attacks by explicitly setting when a process should crash

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** The Node process will crash when errors are not handled. Many best practices even recommend to exit even though an error was caught and got handled. Express, for example, will crash on any asynchronous error - unless you wrap routes with a catch clause. This opens a very sweet attack spot for attackers who recognize what input makes the process crash and repeatedly send the same request. There's no instant remedy for this but a few techniques can mitigate the pain: Alert with critical severity anytime a process crashes due to an unhandled error, validate the input and avoid crashing the process due to invalid user input, wrap all routes with a catch and consider not to crash when an error originated within a request (as opposed to what happens globally)

**Otherwise:** This is just an educated guess: given many Node.js applications, if we try passing an empty JSON body to all POST requests - a handful of applications will crash. At that point, we can just repeat sending the same request to take down the applications with ease

<br/><br/>

## ![✔] 6.24. Prevent unsafe redirects

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** Redirects that do not validate user input can enable attackers to launch phishing scams, steal user credentials, and perform other malicious actions.

**Otherwise:** If an attacker discovers that you are not validating external, user-supplied input, they may exploit this vulnerability by posting specially-crafted links on forums, social media, and other public places to get users to click it.

🔗 [**Read More: Prevent unsafe redirects**](/sections/security/saferedirects.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `7. Лучшие Практики По Быстродействию`

## Our contributors are working on this section. [Would you like to join?](https://github.com/i0natan/nodebestpractices/issues/256)

<br/><br/><br/>

# Milestones

To maintain this guide and keep it up to date, we are constantly updating and improving the guidelines and best practices with the help of the community. You can follow our [milestones](https://github.com/i0natan/nodebestpractices/milestones) and join the working groups if you want to contribute to this project

<br/><br/>

## Translations

All translations are contributed by the community. We will be happy to get any help with either completed, ongoing or new translations!

### Completed translations

- ![CN](/assets/flags/CN.png) [Chinese](README.chinese.md) - Courtesy of [Matt Jin](https://github.com/mattjin)

### Translations in progress

- ![FR](/assets/flags/FR.png) [French](https://github.com/gaspaonrocks/nodebestpractices/blob/french-translation/README.french.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/129))
- ![HE](/assets/flags/HE.png) Hebrew ([Discussion](https://github.com/i0natan/nodebestpractices/issues/156))
- ![KR](/assets/flags/KR.png) [Korean](README.korean.md) - Courtesy of [Sangbeom Han](https://github.com/uronly14me) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/94))
- ![RU](/assets/flags/RU.png) [Russian](https://github.com/i0natan/nodebestpractices/blob/russian-translation/README.russian.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/105))
- ![ES](/assets/flags/ES.png) [Spanish](https://github.com/i0natan/nodebestpractices/blob/spanish-translation/README.spanish.md) ([Discussion](https://github.com/i0natan/nodebestpractices/issues/95))
- ![TR](/assets/flags/TR.png) Turkish ([Discussion](https://github.com/i0natan/nodebestpractices/issues/139))

<br/><br/><br/>

# Core Contributors

## `Yoni Goldberg`

Independent Node.js consultant who works with customers in USA, Europe, and Israel on building large-scale scalable Node applications. Many of the best practices above were first published in his blog post at [goldbergyoni.com](https://goldbergyoni.com). Reach Yoni at @goldbergyoni or me@goldbergyoni.com

## `Ido Richter`

👨‍💻 Software engineer, 🌐 web developer, 🤖 emojis enthusiast

## `Refael Ackermann` [@refack](https://github.com/refack) &lt;refack@gmail.com&gt; (he/him)

Node.js Core Collaborator, been noding since 0.4, and have noded in multiple production sites. Founded `node4good` home of [`lodash-contrib`](https://github.com/node4good/lodash-contrib), [`formage`](https://github.com/node4good/formage), and [`asynctrace`](https://github.com/node4good/asynctrace).
`refack` on freenode, Twitter, GitHub, GMail, and many other platforms. DMs are open, happy to help

## `Bruno Scheufler`

💻 full-stack web developer and Node.js enthusiast

## `Kyle Martin` [@js-kyle](https://github.com/js-kyle)

Full Stack Developer based in New Zealand, interested in architecting and building Node.js applications to perform at global scale. Keen contributor to open source software, including Node.js Core.

## `Sagir Khan`

Deep specialist in JavaScript and its ecosystem — React, Node.js, MongoDB, pretty much anything that involves using JavaScript/JSON in any layer of the system — building products using the web platform for the world’s most recognized brands. Individual Member of the Node.js Foundation, collaborating on the Community Committee's Website Redesign Initiative.

Social: gh. [sagirk](https://github.com/sagirk) | t. [@sagir_k](https://twitter.com/sagir_k) | li. [sagirk](https://linkedin.com/in/sagirk) | w. [sagirk.com](https://sagirk.com/)

<br/><br/><br/>

# Thank You Notes

This repository is being kept up to date thanks to the help from the community. We appreciate any contribution, from a single word fix to a new best practice. Below is a list of everyone who contributed to this project. A 🌻 marks a successful pull request and a ⭐ marks an approved new best practice

### Flowers

🌻 [Kevin Rambaud](https://github.com/kevinrambaud),
🌻 [Michael Fine](https://github.com/mfine15),
🌻 [Shreya Dahal](https://github.com/squgeim),
🌻 [ChangJoo Park](https://github.com/ChangJoo-Park),
🌻 [Matheus Cruz Rocha](https://github.com/matheusrocha89),
🌻 [Yog Mehta](https://github.com/BitYog),
🌻 [Kudakwashe Paradzayi](https://github.com/kudapara),
🌻 [t1st3](https://github.com/t1st3),
🌻 [mulijordan1976](https://github.com/mulijordan1976),
🌻 [Matan Kushner](https://github.com/matchai),
🌻 [Fabio Hiroki](https://github.com/fabiothiroki),
🌻 [James Sumners](https://github.com/jsumners),
🌻 [Chandan Rai](https://github.com/crowchirp),
🌻 [Dan Gamble](https://github.com/dan-gamble),
🌻 [PJ Trainor](https://github.com/trainorpj),
🌻 [Remek Ambroziak](https://github.com/reod),
🌻 [Yoni Jah](https://github.com/yonjah),
🌻 [Misha Khokhlov](https://github.com/hazolsky),
🌻 [Evgeny Orekhov](https://github.com/EvgenyOrekhov),
🌻 [Gediminas Petrikas](https://github.com/gediminasml),
🌻 [Isaac Halvorson](https://github.com/hisaac),
🌻 [Vedran Karačić](https://github.com/vkaracic),
🌻 [lallenlowe](https://github.com/lallenlowe),
🌻 [Nathan Wells](https://github.com/nwwells),
🌻 [Paulo Vítor S Reis](https://github.com/paulovitin),
🌻 [syzer](https://github.com/syzer),
🌻 [David Sancho](https://github.com/davesnx),
🌻 [Robert Manolea](https://github.com/pupix),
🌻 [Xavier Ho](https://github.com/spaxe),
🌻 [Aaron Arney](https://github.com/ocularrhythm),
🌻 [Jan Charles Maghirang Adona](https://github.com/septa97),
🌻 [Allen Fang](https://github.com/AllenFang),
🌻 [Leonardo Villela](https://github.com/leonardovillela),
🌻 [Michal Zalecki](https://github.com/MichalZalecki)
🌻 [Chris Nicola](https://github.com/chrisnicola),
🌻 [Alejandro Corredor](https://github.com/aecorredor),
🌻 [Ye Min Htut](https://github.com/ymhtut),
🌻 [cwar](https://github.com/cwar),
🌻 [Yuwei](https://github.com/keyfoxth),
🌻 [Utkarsh Bhatt](https://github.com/utkarshbhatt12),
🌻 [Duarte Mendes](https://github.com/duartemendes),
🌻 [Sagir Khan](https://github.com/sagirk),
🌻 [Jason Kim](https://github.com/serv),
🌻 [Mitja O.](https://github.com/Max101),
🌻 [Sandro Miguel Marques](https://github.com/SandroMiguel),
🌻 [Gabe Kuslansky](https://github.com/GabeKuslansky),
🌻 [Ron Gross](https://github.com/ripper234),
🌻 [Valeri Karpov](https://github.com/vkarpov15)
🌻 [Sergio](https://github.com/imsergiobernal),
🌻 [Duarte Mendes](https://github.com/duartemendes),
🌻 [Nikola Telkedzhiev](https://github.com/ntelkedzhiev),
🌻 [Vitor Godoy](https://github.com/vitordagamagodoy),
🌻 [Manish Saraan](https://github.com/manishsaraan),
🌻 [Sangbeom Han](https://github.com/uronly14me),
🌻 [blackmatch](https://github.com/blackmatch),
🌻 [Joe Reeve](https://github.com/ISNIT0),
🌻 [Marcelo Melo](https://github.com/marcelosdm),
🌻 [Ryan Busby](https://github.com/BusbyActual),
🌻 [Iman Mohamadi](https://github.com/ImanMh),
🌻 [Remek Ambroziak](https://github.com/reod),
🌻 [Sergii Paryzhskyi](https://github.com/HeeL),
🌻 [Kapil Patel](https://github.com/kapilepatel),
🌻 [迷渡](https://github.com/justjavac)

### Stars <br/>

⭐ [Kyle Martin](https://github.com/js-kyle),
⭐ [Keith Holliday](https://github.com/TheHollidayInn),
⭐ [Corey Cleary](https://github.com/coreyc)

<br/><br/><br/>
