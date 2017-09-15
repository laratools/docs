---
currentMenu: gitlab-ci
---

# Laravel-CI on GitLab CI

Here are a few examples on getting up and running using the image on GitLab CI.

#### Contents

* [Getting Started](#getting-started)
* [Laravel Mix](#laravel-mix)
* [Laravel Dusk](#laravel-dusk)

## Getting Started

This is the minimal setup you'll need to get your unit tests running.

> 💡 Don't forget to choose the correct image you need for your project's PHP version requirement

```yaml
image: laratools/laravel-ci:latest

before_script:
  - composer install --no-progress --no-suggest

test:
  script:
    - ./vendor/bin/phpunit
```

## Laravel Mix

The `laratools/laravel-ci` image comes with [nvm](https://github.com/creationix/nvm) installed which allows you to easily install different node versions.

Here we'll install node version 8, install our npm dependencies and then run laravel mix to check our production assets can be built.

```yaml
image: laratools/laravel-ci:latest

before_script:
  - composer install --no-progress --no-suggest
  - source ~/.nvm/nvm.sh
  - nvm install 8
  - npm install

test:
  script:
    - ./vendor/bin/phpunit
    - npm run prod
```

## Laravel Dusk

Before you can run your browser tests you'll need to serve your application so the browser can interact with it.
Luckily Artisan comes with a handy serve command `php artisan serve`, lets use that.
You'll need to combine it with `nohup` so the process runs in the background so that you can also run the tests at the same time.

In this example we also store the artifacts. Artifacts are files that are generated during the build process.
Defining them means GitLab CI will store them once the build has finished for you to access later.
In this case, it means you can see what the browser was doing if the tests fail.

```yaml
image: laratools/laravel-ci:latest

before_script:
  - composer install --no-progress --no-suggest
  - source ~/.nvm/nvm.sh
  - nvm install 8
  - npm install
  - nohup php artisan serve &

test:

  artifacts:
    paths:
      - ./tests/Browser/console
      - ./tests/Browser/screenshots
      
  script:
    - ./vendor/bin/phpunit
    - npm run prod
    - php artisan dusk
``` 