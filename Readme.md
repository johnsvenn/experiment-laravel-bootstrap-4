How to install Bootstrap 4 in Laravel 5.5
=========================================

Painless steps to remove [Bootstrap 3](https://getbootstrap.com/docs/3.3/) with [Bootstrap 4](https://getbootstrap.com/)
This will include the the individual .scss and .js files so you can mix and match and only include the components you actually need.

This is inside Homestead, otherwise you'll need to make sure that Node, npm etc. are all properly installed.

Create a new project
--------------------

Create a new Laravel project, then add the Auth scaffolding

```
laravel new www.example.com
cd www.laravel.example.com
nano .env
php artisan migrate
php artisan make:auth
```

Update packages.json
--------------------

In Homestead `npm run watch` isn't working at the moment.
Edit `/packages.json` and add `--watch-poll` to the watch line replace:

```
"watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
```

With:

```
"watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --watch-poll --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
```

This means that you can now `npm run watch` to recompile your JS and CSS automatically.

Update the packages
-------------------

1. Remove bootstrap-sass `npm uninstall bootstrap-sass`

2. Add popper.js `npm install popper.js -D`

3. Update jQuery `npm install jquery -D`

4. Install Bootstrap 4 (currently Currently v4.0.0-beta.2) `npm install bootstrap@4.0.0-beta.2 -D`

The `-D` option will update `devDependencies` in `/packages.json` which should now look something like this:

```
"devDependencies": {
    "axios": "^0.16.2",
    "bootstrap": "^4.0.0-beta.2",
    "cross-env": "^5.0.1",
    "jquery": "^3.2.1",
    "laravel-mix": "^1.0",
    "lodash": "^4.17.4",
    "popper.js": "^1.12.6",
    "vue": "^2.1.10"
  }
```

Setup scss 
----------

Within Laravel we are using Laravel which is based on webpack, so following the Bootstrap 4 webpack instructions https://getbootstrap.com/docs/4.0/getting-started/webpack/ We will create a `_custom.scss` that let's us override the built in variables, then include the individual `.scss` files and then create `_base.scss` where we can start defaining our own styles. 

1. Delete `/resouces/assets/sass/_variables.scss`

2. Create `/resouces/assets/sass/_custom.scss`

3. Edit `/resouces/assets/sass/app.scss` to include the individual files:

```
// Fonts
@import url("https://fonts.googleapis.com/css?family=Raleway:300,400,600");

// Variables
@import "custom";

// Bootstrap

// Required
@import "~bootstrap/scss/functions";
@import "~bootstrap/scss/variables";
@import "~bootstrap/scss/mixins";

// Optional
@import "~bootstrap/scss/root";
@import "~bootstrap/scss/print";
@import "~bootstrap/scss/reboot";
@import "~bootstrap/scss/type";
@import "~bootstrap/scss/images";
@import "~bootstrap/scss/code";
@import "~bootstrap/scss/grid";
@import "~bootstrap/scss/tables";
@import "~bootstrap/scss/forms";
@import "~bootstrap/scss/buttons";
@import "~bootstrap/scss/transitions";
@import "~bootstrap/scss/dropdown";
@import "~bootstrap/scss/button-group";
@import "~bootstrap/scss/input-group";
@import "~bootstrap/scss/custom-forms";
@import "~bootstrap/scss/nav";
@import "~bootstrap/scss/navbar";
@import "~bootstrap/scss/card";
@import "~bootstrap/scss/breadcrumb";
@import "~bootstrap/scss/pagination";
@import "~bootstrap/scss/badge";
@import "~bootstrap/scss/jumbotron";
@import "~bootstrap/scss/alert";
@import "~bootstrap/scss/progress";
@import "~bootstrap/scss/media";
@import "~bootstrap/scss/list-group";
@import "~bootstrap/scss/close";
@import "~bootstrap/scss/modal";
@import "~bootstrap/scss/tooltip";
@import "~bootstrap/scss/popover";
@import "~bootstrap/scss/carousel";
@import "~bootstrap/scss/utilities";

@import "base";
```

Setup JS
--------

Similarly edit `/resources/assets/js/bootstrap.js` remove the require for `bootstrap-sass` and include the individual bootstrap plugins.

Update this section:
```
try {
    window.$ = window.jQuery = require('jquery');

    require('bootstrap-sass');
} catch (e) {}
```
To look like this:

```
try {

    window.$ = window.jQuery = require('jquery');

    require('bootstrap/js/dist/alert');
    require('bootstrap/js/dist/button');
    require('bootstrap/js/dist/carousel');
    require('bootstrap/js/dist/dropdown');
    require('bootstrap/js/dist/index');
    require('bootstrap/js/dist/modal');
    require('bootstrap/js/dist/popover');
    require('bootstrap/js/dist/scrollspy');
    require('bootstrap/js/dist/tab');
    require('bootstrap/js/dist/tooltip');
    require('bootstrap/js/dist/util'); 
    
} catch (e) {}

```

Try it out
----------

Edit `/resouces/assets/sass/_custom.scss` e.g.

```
$body-bg: red;
```

Then `npm install && npm run dev`

Visit /login and you should now see the Laravel login form with red background.

