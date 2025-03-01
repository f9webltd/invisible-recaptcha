Laravel Invisible reCAPTCHA v3
==========
[![Packagist PHP Version](https://img.shields.io/packagist/php-v/f9webltd/invisible-recaptcha?style=flat-square)](https://packagist.org/packages/f9webltd/invisible-recaptcha)
[![Packagist Version](https://img.shields.io/packagist/v/f9webltd/invisible-recaptcha?style=flat-square)](https://packagist.org/packages/f9webltd/invisible-recaptcha)

![invisible_recaptcha_demo](http://i.imgur.com/1dZ9XKn.png)

## Background

28/06/2024 - the [original repository](https://github.com/albertcht/invisible-recaptcha) has not been updated for over 2 years and lacks Laravel 11 support and a [critical PR](https://github.com/albertcht/invisible-recaptcha/pull/173) relating to patch the recent polyfillio[dot]io attack. This fork includes both fixes and drops support for old Laravel and PHP versions. I plan to tidy up the forked repository, add GitHub workflows and set a different namespace.

## Support

Laravel 10 / 11 / 12, PHP `*8.0`.

## Installation

```
composer require f9webltd/invisible-recaptcha
```

### Setup

Add ServiceProvider to the providers array in `app/config/app.php`.

```
AlbertCht\InvisibleReCaptcha\InvisibleReCaptchaServiceProvider::class,
```

### Configuration
Before you set your config, remember to choose `invisible reCAPTCHA` while applying for keys.
![invisible_recaptcha_setting](http://i.imgur.com/zIAlKbY.jpg)

Add `INVISIBLE_RECAPTCHA_SITEKEY`, `INVISIBLE_RECAPTCHA_SECRETKEY` to **.env** file.

```
// required
INVISIBLE_RECAPTCHA_SITEKEY={siteKey}
INVISIBLE_RECAPTCHA_SECRETKEY={secretKey}

// optional
INVISIBLE_RECAPTCHA_BADGEHIDE=false
INVISIBLE_RECAPTCHA_DATABADGE='bottomright'
INVISIBLE_RECAPTCHA_TIMEOUT=5
INVISIBLE_RECAPTCHA_DEBUG=false
```

> There are three different captcha styles you can set: `bottomright`, `bottomleft`, `inline`

> If you set `INVISIBLE_RECAPTCHA_BADGEHIDE` to true, you can hide the badge logo.

> You can see the binding status of those catcha elements on browser console by setting `INVISIBLE_RECAPTCHA_DEBUG` as true.

### Usage

Before you render the captcha, please keep those notices in mind:

* `render()` or `renderHTML()` function needs to be called within a form element.
* You have to ensure the `type` attribute of your submit button has to be `submit`.
* There can only be one submit button in your form.

##### Display reCAPTCHA in Your View

```php
{!! app('captcha')->render() !!}

// or you can use this in blade
@captcha
```

With custom language support:

```php
{!! app('captcha')->render('en') !!}

// or you can use this in blade
@captcha('en')
```

##### Usage with Javascript frameworks like VueJS:

The `render()` process includes three distinct sections that can be rendered separately incase you're using the package with a framework like VueJS which throws console errors when `<script>` tags are included in templates.

You can render the polyfill (do this somewhere like the head of your HTML:)

```php
{!! app('captcha')->renderPolyfill() !!}
// Or with blade directive:
@captchaPolyfill
```

You can render the HTML using this following, this needs to be INSIDE your `<form>` tag:

```php
{!! app('captcha')->renderCaptchaHTML() !!}
// Or with blade directive:
@captchaHTML
```

And you can render the neccessary `<script>` tags including the optional language support by using:

```php
// The argument is optional.
{!! app('captcha')->renderFooterJS('en') !!}

// Or with blade directive:
@captchaScripts
// blade directive, with language support:
@captchaScripts('en')

```

##### Validation

Add `'g-recaptcha-response' => 'required|captcha'` to rules array.

```php
$validate = Validator::make(Input::all(), [
    'g-recaptcha-response' => 'required|captcha'
]);
```

## Example Repository
Repo: https://github.com/albertcht/invisible-recaptcha-example

This repo demonstrates how to use this package with ajax way.
