# PHP APM Setup

[![packagist](https://img.shields.io/badge/dev--master-%23f28d1a?label=packagist)](https://packagist.org/packages/middleware/agent-apm-php)

|  Traces  |  Metrics  |  Profiling  |  Logs (App/Custom)  |
|:--------:|:---------:|:-----------:|:-------------------:|
|   Yes    |    No     |     No      |       No/Yes        |

## Prerequisites

* To monitor APM data on dashboard, Middleware Host agent needs to be installed, You can refer [this demo project](https://github.com/middleware-labs/demo-apm/tree/master/php) to refer use cases of APM.
* PHP requires at least PHP 8+ and a PHP-Extension to run this agent.

--------------------

## Guide

### Initial Setup:

Before installing this agent, you need to install PHP-Extension(named otel_instrumentation) to run this agent. You can follow below steps to install & enable it:
* Run `sudo pecl install channel://pecl.php.net/opentelemetry-1.0.0beta3`.
* Then, Add the extension to your `php.ini` file like: `extension=opentelemetry.so`.
* And verify that the extension is installed and enabled using: `php -m | grep  opentelemetry`.

#### Troubleshoot:-
While installing PHP-Extension:
* If you are facing `pecl:command not found`, then you need to run follow cmd:
  ```shell
  sudo apt-get update
  apt-get install php-pear php8.1-dev
  ```
* If you are facing any kind of broken dependencies issues like: `libpcre2-dev : Depends: libpcre2-8-0 / libpcre2-16-0 / libpcre2-32-0`, then you need to run follow cmd:
  ```shell
  sudo apt --fix-broken install
  ```
* If you are facing `ERROR: 'phpize' failed`, then you need to run follow cmd:
  ```shell
  sudo apt-get update
  sudo apt-get install php8.1-dev
  ```
  ```shell
  sudo apt-get update
  sudo pecl channel-update pecl.php.net
  ```

### Step 1: Install APM-PHP package

Run below command in your terminal to install Middleware's APM-PHP package.
```shell
composer require middleware/agent-apm-php
```

### Step 2: Prepend APM script

Add these lines given below at the very start of your project.

```php
require 'vendor/autoload.php';
use Middleware\AgentApmPhp\MwTracker;
```

### Step 3: Use APM Collector & Start the Tracing-scope
To proceed further you need to decide whether you want to proceed with the manual or auto instrumentation. Here are steps mentioned for each:

- **Auto Instrumentation**
If you want to automatically track the performance and behavior of a function, you can do so by adding the following code to it:
```php
$tracker->instrumentFunction(<CLASS_NAME>::class,"<FUNCTION_NAME>");
```
You can also use custom attributes to instrument functions. This allows you to specify additional information about the function, such as its purpose, or dependencies.
```php
$tracker->instrumentFunction(<CLASS_NAME>::class,"<FUNCTION_NAME>",[
		'custom.attr1' => 'custom.val1',
		'custom.attr2' => 'custom.val2',
]);
```
***NOTE***: Only non static functions can be auto instrumented. 

- **Manual Instrumentation**
To manually instrument a function, register a hook using the following function:
```php
$tracker->registerHook('<CLASS_NAME>', '<FUNCTION_NAME>');
```
Similar to auto instrumentation we can use custom attributes here as well:
```php
$tracker->registerHook('<CLASS_NAME>', '<FUNCTION_NAME>',[
		'custom.attr1' => 'custom.val1',
		'custom.attr2' => 'custom.val2',
]);
```
## Logging
Custom logs are also available. You can use them by calling the mentioned function for different types of logs.
```php
$tracker->warn("<CUSTOM_MESSAGE>"); 	//Warning Log
$tracker->error("<CUSTOM_MESSAGE>"); 	//Error Log
$tracker->info("<CUSTOM_MESSAGE>"); 	//Info Log
$tracker->debug("<CUSTOM_MESSAGE>"); 	//Debug Log
```
You can use these functions throughout your code to generate meaningful logs. This will help you track and debug your code more effectively.
