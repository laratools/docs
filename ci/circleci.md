---
currentMenu: circleci
---

# Laratools CI on CircleCI

Here are a few examples on getting up and running using the image on CircleCI.
You must be using CircleCI 2.0 otherwise you won't be able to use the docker image.

#### Contents

* [Getting Started](#getting-started)
* [Laravel Mix](#laravel-mix)
* [Laravel Dusk](#laravel-dusk)

## Getting Started

This is the minimal setup you'll need to get your unit tests running.

> 💡 Don't forget to choose the correct image you need for your project's PHP version requirement

```yaml
version: 2

jobs:
  build:
    working_directory: /var/www
    
    docker:
      - image: laratools/ci:7.4
      
    steps:

      - checkout
      
      - run:
          name: Install Composer Dependencies
          command: composer install --no-progress --no-suggest

      - run:
          name: Unit Tests
          command: ./vendor/bin/phpunit
```

## Laravel Mix

The `laratools/ci` image comes with [nvm](https://github.com/creationix/nvm) installed which allows you to easily install different node versions.

Here we'll install node version 8, install our npm dependencies and then run laravel mix to check our production assets can be built.

```yaml
version: 2

jobs:
  build:
    working_directory: /var/www
    
    docker:
      - image: laratools/ci:7.4
      
    steps:

      - checkout
      
      - run:
          name: Install Composer Dependencies
          command: composer install --no-progress --no-suggest

      - run:
          name: Unit Tests
          command: ./vendor/bin/phpunit
      
      - run:
          name: Install Node Dependencies
          command: |
            nvm install 8
            npm install

      - run:
          name: Mix
          command: npm run prod
``` 

## Laravel Dusk

Before you can run your browser tests you'll need to serve your application so the browser can interact with it.
Luckily Artisan comes with a handy serve command `php artisan serve`, lets use that.

In this example we also store the artifacts. Artifacts are files that are generated during the build process.
Defining them means CircleCI will store them once the build has finished for you to access later.
In this case, it means you can see what the browser was doing if the tests fail.


```yaml
version: 2

jobs:
  build:
    working_directory: /var/www
    
    docker:
      - image: laratools/ci:7.4
      
    steps:

      - checkout
      
      - run:
          name: Install Composer Dependencies
          command: composer install --no-progress --no-suggest

      - run:
          name: Unit Tests
          command: ./vendor/bin/phpunit
          
      - run:
          name: Install Node Dependencies
          command: |
            nvm install 8
            npm install

      - run:
          name: Mix
          command: npm run prod

      - run:
          name: Serve Application
          background: true
          command: php artisan serve

      - run:
          name: Dusk
          command: php artisan dusk

      - store_artifacts:
          path: ./tests/Browser/console
          destination: console

      - store_artifacts:
          path: ./tests/Browser/screenshots
          destination: screenshots
``` 