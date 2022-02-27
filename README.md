[![Author](https://img.shields.io/badge/author-Leonid74-blue.svg)](https://github.com/Leonid74)
[![License](https://img.shields.io/badge/license-BSD-blue.svg?maxAge=43200)](./LICENSE)
[![Latest Stable Version](https://img.shields.io/github/v/release/Leonid74/wildberries-api-php)](https://github.com/Leonid74/wildberries-api-php/releases/latest)
![GitHub all releases](https://img.shields.io/github/downloads/Leonid74/wildberries-api-php/total?color=blue)
[![CodeFactor](https://www.codefactor.io/repository/github/leonid74/wildberries-api-php/badge)](https://www.codefactor.io/repository/github/leonid74/wildberries-api-php)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/218b45bd6c75462c82b444a265b434bf)](https://www.codacy.com/gh/Leonid74/wildberries-api-php/dashboard)
[![Code Inspector](https://api.codiga.io/project/30883/score/svg)](https://app.codiga.io/public/project/30883/wildberries-api-php/dashboard)

# Wildberries REST API statistics client library with throttling requests
## [(russian) Русское описание ниже](#%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%B0%D1%8F-rest-api-%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA%D0%B0-%D1%81%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B8-wildberries-%D1%81-%D1%80%D0%B5%D0%B3%D1%83%D0%BB%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%D0%BC-%D0%B7%D0%B0%D0%BF%D1%80%D0%BE%D1%81%D0%BE%D0%B2)

A simple Wildberries REST API statistics client library with throttling requests (for example, no more than 10 requests per second according to API rules) and an example for PHP. Automatic request resending is supported when the http response code "429: too many requests" is received.

Statistics API Documentation [Wildberries REST API statistics Documentation](https://images.wbstatic.net/portal/education/Kak_rabotat'_s_servisom_statistiki.pdf)

New API Documentation [Wildberries REST API Documentation](https://suppliers-api.wildberries.ru/swagger/index.html)

### Installing

Via Composer:

```bash
composer require Leonid74/wildberries-api-php
```

### Usage

```php
<?php
require 'vendor/autoload.php';

// Without Composer (and instead of "require 'vendor/autoload.php'"):
// require("your-path/wildberries-api-php/src/WildberriesInterface.php");
// require("your-path/wildberries-api-php/src/Client.php");

use Leonid74\Wildberries\Client;

require_once 'vendor/autoload.php';

$token = '<you token x64>';
$dateFrom = '01-01-2022';
$dateTo = '19-01-2022';

try {
    // Create new client
    $WbApiClient = new Client( $token );

    // DEBUG level can be one of: DEBUG_NONE (default) or DEBUG_URL, DEBUG_HEADERS, DEBUG_CONTENT
    // no debug
    // $Client->debugLevel = Client::DEBUG_NONE;
    // only URL level debug
    // $Client->debugLevel = Client::DEBUG_URL;
    // only URL and HEADERS level debug
    // $Client->debugLevel = Client::DEBUG_HEADERS;
    // max level of debug messages to STDOUT
    // $Client->debugLevel = Client::DEBUG_CONTENT;
    $WbApiClient->debugLevel = Client::DEBUG_URL;

    // set the trottling of HTTP requests to 2 per second
    $WbApiClient->throttle = 2;
} catch ( Exception $e ) {
    die( "Critical exception when creating ApiClient: ({$e->getCode()}) " . $e->getMessage() );
}

/*
 * Example: Get the sales
 */
$sales = $WbApiClient->sales( $dateFrom );
if ( isset( $sales->is_error ) ) {
    echo "\nError: " . implode( '; ', $sales->errors );
} else {
    var_dump( $sales );
}

/*
 * Example: Get the report detail by period
 */
$reportDetailByPeriod = $WbApiClient->reportDetailByPeriod( $dateFrom, $dateTo );
if ( isset( $reportDetailByPeriod->is_error ) ) {
    echo "\nError: " . implode( '; ', $reportDetailByPeriod->errors );
} else {
    var_dump( $reportDetailByPeriod );
}

// You can set a common date (dateFrom) via the setDateFrom() function and then access other functions
// without passing the date
$WbApiClient->setDateFrom( $dateFrom );
$sales = $WbApiClient->sales();
$incomes = $WbApiClient->incomes();

```

## Author
Leonid74 (https://github.com/Leonid74)

## License
This software is distributed under the [BSD 3-Clause license](./LICENSE) terms.


## Клиентская REST API библиотека статистики Wildberries с регулированием запросов

Простая клиентская REST API библиотека статистики Wildberries с регулированием запросов (например, не более 10 запросов в секунду в соответствии с правилами API) и примером для PHP. Поддерживается автоматическая повторная отправка запроса при получении http кода ответа "429: too many requests".

Описание API статистики [Wildberries REST API statistics](https://images.wbstatic.net/portal/education/Kak_rabotat'_s_servisom_statistiki.pdf)

Описание нового API [Wildberries REST API](https://suppliers-api.wildberries.ru/swagger/index.html)

### Установка

Через Composer:

```bash
composer require Leonid74/wildberries-api-php
```

### Использование

```php
<?php
require 'vendor/autoload.php';

// Без Composer можно подключить вот так (вместо "require 'vendor/autoload.php'"):
// require("your-path/wildberries-api-php/src/WildberriesInterface.php");
// require("your-path/wildberries-api-php/src/Client.php");

use Leonid74\Wildberries\Client;

require_once 'vendor/autoload.php';

$token = '<Ваш токен партнера x64>';
$dateFrom = '01-01-2022';
$dateTo = '19-01-2022';

try {
    // Создаем новый клиент
    $WbApiClient = new Client( $token );

    // Уровень DEBUG может быть одним из: DEBUG_NONE (по умолчанию) или DEBUG_URL, DEBUG_HEADERS, DEBUG_CONTENT
    // без вывода отладочной информации
    // $Client->debugLevel = Client::DEBUG_NONE;
    // выводим только URL запросов/ответов
    // $Client->debugLevel = Client::DEBUG_URL;
    // выводим только URL и заголовки запросов/ответов
    // $Client->debugLevel = Client::DEBUG_HEADERS;
    // выводим  URL, заголовки и всю остальную информацию запросов/ответов в STDOUT
    // $Client->debugLevel = Client::DEBUG_CONTENT;
    $WbApiClient->debugLevel = Client::DEBUG_URL;

    // Устанавливаем троттлинг в 2 запроса в секунду
    $WbApiClient->throttle = 2;
} catch ( Exception $e ) {
    die( "Критическая ошибка при создании ApiClient: ({$e->getCode()}) " . $e->getMessage() );
}

/*
 * Пример: Получаем продажи
 */
$sales = $WbApiClient->sales( $dateFrom );
if ( isset( $sales->is_error ) ) {
    echo "\nError: " . implode( '; ', $sales->errors );
} else {
    var_dump( $sales );
}

/*
 * Пример: Получаем отчет о продажах по реализации
 */
$reportDetailByPeriod = $WbApiClient->reportDetailByPeriod( $dateFrom, $dateTo );
if ( isset( $reportDetailByPeriod->is_error ) ) {
    echo "\nError: " . implode( '; ', $reportDetailByPeriod->errors );
} else {
    var_dump( $reportDetailByPeriod );
}

// Можно задать общую дату (dateFrom) через функцию setDateFrom() и затем обращаться к другим функциям,
// не передавая дату
$WbApiClient->setDateFrom( $dateFrom );
$sales = $WbApiClient->sales();
$incomes = $WbApiClient->incomes();

```

## Автор
Leonid74 (https://github.com/Leonid74)

## Лицензия
Данное ПО распространяется по лицензии [BSD 3-Clause license](./LICENSE)
