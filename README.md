# gulp-jade-usemin
> Replaces references to non-optimized scripts or stylesheets into a set of Jade files (or any templates/views).

This task is designed for gulp 3.
> Attention: v0.3.0 options does not compatible with v0.2.0.

## Basic Usage

First, install `gulp-jade-usemin` as a development dependency:

```shell
npm install --save-dev gulp-jade-usemin
```

Then, add it to your `gulpfile.js`:

```javascript
var usemin = require('gulp-jade-usemin');
var uglify = require('gulp-uglify');
var minifyHtml = require('gulp-minify-html');
var minifyCss = require('gulp-minify-css');
var rev = require('gulp-rev');

gulp.task('usemin', function() {
  gulp.src('./*.jade')
    .pipe(usemin({
      css: [minifyCss(), 'concat'],
      html: [minifyHtml({empty: true})],
      js: [uglify(), rev()]
    }))
    .pipe(gulp.dest('build/'));
});
```

Sample usage in Jade file:

```jade
//- build:css /css/app.css
block stylesheets
  link(rel='stylesheet', href='/css/style.css')
//- endbuild

//- build:js /js/app.js
block scripts
    script(src='/js/script1.js')
    script(src='/js/script2.js')
//- endbuild
```

## Options
-  `assetsBasePath`: jade assets base url

    Usage:
    ```javascript
    gulp.task('usemin', function() {
      gulp.src('./*.jade')
        .pipe(usemin({
          assetsBasePath: 'ASSETS/BASEPATH'
          js: [uglify()]
        }))
        .pipe(gulp.dest('build/'));
    });
    ```
    
    ```jade
    //- build:js /js/app.js
    block scripts
        script(src='/js/script1.js')
        script(src='/js/script2.js')
    //- endbuild
    ```
    
    Output:
    ```jade
     script(type='text/javascript', src='ASSETS/BASEPATH/js/app.js')
    ```

-  `outputBasePath`: assets output basepath

    Usage:
    ```javascript
    gulp.task('usemin', function() {
      gulp.src('./*.jade')
        .pipe(usemin({
          outputBasePath: '../public'
          js: [uglify()]
        }))
        .pipe(gulp.dest('build/'));
    });
    ```
    
    ```jade
    //- build:js /js/app.js
    block scripts
        script(src='/js/script1.js')
        script(src='/js/script2.js')
    //- endbuild
    ```
    
    Output:
    ```
    |
    +- app
    |   +- index.html
    |   +- assets
    |       +- js
    |          +- script1.js
    |          +- script1.js
    +- build
    |   +- index.html
    +- public
    |     +- js
    |         +- app.js <- output file
    +- gulpfile.json
    ```

## Changelog

### 1.1.0
- fixed RegExp issue, now requires you to have a `/` or `.` as first character in your rev replacement (e.g. you need to have relative or absolute paths via `script(src='/foo.js')` as opposed to `script(src='foo.js')`)

### 1.0.0
- added `video` and img support
- support `append` and `prepend`
- jade style syntax

### 0.0.1
- initial release
