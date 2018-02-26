---
currentMenu: introduction
---

# Introduction

Laratools CI is a Docker image that's ready to use, out of the box for automated testing with Laravel. That means you can setup Dusk testing within minutes using CircleCI or GitLab CI.

Of course it may be used with any platform that supports Docker, so feel free to submit a PR with a sample config for your platform.

## Contents

- [PHP Versions](#php-versions)
- [Pre-Installed Software](#pre-installed-software)
- Platforms 
  - [CircleCI](/ci/circleci.html)
  - [GitLab CI](/ci/gitlab-ci.html)

## PHP Versions

There are several images based on the PHP version you require. Currently we support `7.0`, `7.1` and `7.2`.

* `laratools/ci:7.0`
* `laratools/ci:7.1`
* `laratools/ci:7.2`

You can view the full list on the [docker repository](https://hub.docker.com/r/laratools/ci/tags/)
  
## Pre-Installed Software

All images come with the following software already installed.

* PHP (Version depends upon the image you've selected)
* Composer
* NVM
* Yarn

## Security

If you discover any security related issues, please email [security@iwader.co.uk](mailto:security@iwader.co.uk) instead of using the issue tracker.
