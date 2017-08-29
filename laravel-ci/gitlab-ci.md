---
currentMenu: gitlab-ci
---

# Laravel-CI on GitLab CI

Heres a few examples on getting up and running using the image on GitLab CI.

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

Heres where things get a little trickier. Firstly we must open a browser, by default dusk uses chrome so we'll open a new instance.
Next you'll need to serve your application to chrome can interact with it. Luckily Artisan comes with a handy serve command `php artisan serve`, lets use that.
Finally we can run the dusk tests.

In this example we also store the artifacts. Artifacts are any files that are generated during the build process. Defining them means CircleCI will store them
for you to access later. In this case, it means you can see what the browser was doing if the tests fail.  

```yaml
image: laratools/laravel-ci:latest

before_script:
  - composer install --no-progress --no-suggest
  - source ~/.nvm/nvm.sh
  - nvm install 8
  - npm install
  - nohup ./vendor/laravel/dusk/bin/chromedriver-linux &
  - nohup php artisan serve &

test:

  artifacts:
    paths:
      - ./tests/Browser/console
      - ./tests/Browser/screenshots
      
  script:
    - ./vendor/bin/phpunit
    - npm run prod
    - DISPLAY=:0 php artisan dusk
``` 