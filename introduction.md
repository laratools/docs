---
currentMenu: introduction
---

# Introduction

Laratools is a collection of useful everyday tools for Laravel which implement common functionality found in many applications.

## Installation

```bash
composer require laratools/laratools
```

You only need to add the service providers and facades if you're using Laravel 5.4 or below. Laravel 5.5 and above will use the auto-discover feature to set this up for you.

```php
'providers' => [
    \Laratools\Providers\LaratoolsServiceProvider::class,
],
```

## Security

If you discover any security related issues, please email wade@iwader.co.uk instead of using the issue tracker.
