[![Code Climate](https://codeclimate.com/github/ktquez/react-lumen/badges/gpa.svg)](https://codeclimate.com/github/ktquez/react-lumen) [![Build Status](https://travis-ci.org/ktquez/react-lumen.svg?branch=master)](https://travis-ci.org/ktquez/react-lumen) [![Total Downloads](https://poser.pugx.org/ktquez/react-lumen/downloads)](https://packagist.org/packages/ktquez/react-lumen)

# react-Lumen

With `react-lumen` you'll be able to use [ReactJS](https://facebook.github.io/react/) components right from your Blade views, with optional server-side rendering, and use them on the client-side with React due to unobtrusive JavaScript.

# Installation

## V8js dependency

It's important to know that `react-lumen` has an indirect dependency of the [v8js](https://pecl.php.net/package/v8js) PHP extension.

You can see how to install it here: [how to install v8js](install_v8js.md).

## Composer

Set the `minimum-stability` of your `composer.json` to `dev`, adding this:

```json
  "minimum-stability": "dev"
```

Then run:

```sh
  $ composer require ktquez/react-lumen
```

After that you should add this to your providers at the `config/app.php` file of your Laravel app:

```php
  'React\ReactServiceProvider'
```

Add command of publication in ``app/Console/Kernel.php`` 
```php
  protected $commands = [
      \Laravelista\LumenVendorPublish\VendorPublishCommand::class
  ];
```

You are now able to run the command in the lumen by ``laravellista/lumen-vendor-publish``

```sh
  php artisan vendor:publish
```

And the `react.php` file will be available at the `config` folder of your app.

## Install react from npm
```sh
npm install --save react react-dom
```

# Usage

After the installation and configuration, you'll be able to use the `@react_component` directive in your views.

The `@react_component` directive accepts 3 arguments:

```php
  @react_component(<componentName>[, props, options])

  //example
  @react_component('Message', [ 'title' => 'Hello, World' ], [ 'prerender' => true ])

  // example using namespaced component
  @react_component('Acme.Message', [ 'title' => 'Hello, World' ], [ 'prerender' => true ])
```

* `componentName`: Is the name of the global variable that holds your component.  When using [Namespaced Components](https://facebook.github.io/react/docs/jsx-in-depth.html#namespaced-components) you may use dot-notation for the component name.
* `props`: Associative of the `props` that'll be passed to your component
* `options`: Associative array of options that you can pass to the `react-lumen`:
  * `prerender`: Tells react-lumen to render your component server-side, and then just _mount_ it on the client-side. Default to __true__.
  * `tag`: The tag of the element that'll hold your component. Default to __'div'__.
  * _html attributes_: Any other valid HTML attribute that will be added to the wrapper element of your component. Example: `'id' => 'my_component'`.

All your components should be inside `public/js/components.js` (you can configure it, see below) and be global.

You must include `react.js`, `react-dom.js` and `react_ujs.js` (in this order) in your view. You can concatenate these files together using laravel-elixir.

`react-lumen` provides a ReactJS installation and the `react_us.js` file, they'll be at `public/vendor/react-lumen` folder after you install `react-lumen` and run:

```sh
  $ php artisan vendor:publish --force
```

For using the files provided by `react-lumen` and your `components.js` file, add this to your view:

```html
  <script src="{{ base_path('public/js/app.js') }}"></script> <!-- Use webpack for generate (including react and react-dom) -->
  <script src="{{ base_path('public/js/components.js') }}"></script>
  <script src="{{ base_path('public/vendor/react-lumen/react_ujs.js') }}"></script>
```

If you'll use a different version from the one provided by react-lumen (see `composer.json`), you got to configure it (see below).

# Configurations

You can change settings to `react-lumen` at the `config/react.php` file:

```php
  return [
    'source' => 'path_for_react.js',
    'dom-source' => 'path_for_react-dom.js',
    'dom-server-source' => 'path_for_react-dom-server.js',
    'components' => 'path_for_file_containing_your_components.js'
  ];
```

Your `components.js` file should also be included at your view, and all your components must be at the `window` object.

# Thanks

This package is based at [react-laravel](https://github.com/talyssonoc/react-laravel).
