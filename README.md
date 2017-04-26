PHP Docker Images
============================

This repository contains PHP images with composer install preformed via `ONBUILD`. This images are based on official PHP
 images to increase security and stability.

## Difference from official PHP images

 - Installed extensions
    - apcu
    - bcmath
    - opcache
    - pdo_pgsql
    - xdebug
    - zip
 - Configured xdebug for remote debugging
 - Installed Cron for cases when application requires scheduled tasks to be executed (as separate container)
 - Added latest version of composer
 - Added `ONBUILD` instruction which adds composer.json and composer.lock (if exists) and runs composer install

## Why?

On our projects we have a lot of incremental application builds. Since we are copy `vendor` directory in every build
this slows down build time and increases layer size. So instead of few megabytes we have ~100Mb layers for each build.

Also we have some common dependencies for our applications, so we just want to put them in one place.

## Pros/Cons

Pros

  - Overal incremental application image size is decreased drammaticly since vendor directory is changed 
    rarly and will not affect on your incremental builds;
  - Build speed is increased since it doesn't need to check whole vendor directory.

Cons

 - The huge downside is that if you added/updated/removed only one dependency, you will have to reinstall all of them. To
 eliminate negative effect from this I recommend you to use [toran proxy](https://toranproxy.com/) to increase 
 build stability.
 
## License

```
Copyright (c) 2017 Sergey (Fesor) Protko
Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
```