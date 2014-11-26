# grunt-phantomcss

> Automate CSS regression testing with PhantomCSS

## Notice

**This is a fork of the original (presumably discontinued) repository of [grunt-phantomcss](https://github.com/chrisgladd/grunt-phantomcss) and anselmh and  micahgodbolt (https://github.com/micahgodbolt/grunt-phantomcss). Currently this version here is untagged and unreleased on npm. You can install the original with "npm install grunt-phantomcss". However, you can install and use this version:**

Add this to your `package.json`:

    "grunt-phantomcss": "git://github.com/titansgroup/grunt-phantomcss.git",

or, alternatively, type this into your command line interface:

```shell
npm install --save-dev git://github.com/titansgroup/grunt-phantomcss.git
```
## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```javascript
grunt.loadNpmTasks('grunt-phantomcss');
```

## The "phantomcss" task

### Overview
In your project's Gruntfile, add a section named `phantomcss` to the data object passed into `grunt.initConfig()`.

```javascript
grunt.initConfig({
  phantomcss: {
    options: {},
    your_target: {
      options: {
        screenshots: 'test/visual/screenshots/',
        results: 'results/visual/',
        viewportSize: [1280, 800],
        mismatchTolerance: 0.05
      },
      src: [
        'test/visual/**/*.js'
      ]
    }
  }
});


grunt.registerTask('phantomcsstests', ['phantomcss']);
```


```shell
grunt phantomcsstests
```


### Options

#### baseURL - Optional
Type: `String`
Default: blank

Host for test - Use in test files: ```phantomcss.baseURL```

#### src
Type: `String|Array`

The test files to run.

#### options.mismatchTolerance
Type: `Number`
Default: `0.05`

Toleranz of errors that is allowed in a screenshot (for instance to match anti-aliasing bugs).

#### options.screenshots
Type: `String`
Default: `'./screenshots'`

The screenshots directory where test fixtures (comparison screenshots) are stored. Baseline screenshots will be stored here on the first run if they're not present.

#### options.results
Type: `String`
Default: `'./results'`

The directory to store source, diff, and failure screenshots after tests.

#### options.viewportSize
Type: `Array`
Default: `[1280, 800]`

The viewport size to test the site in `[width, height]` format. Useful when testing responsive layouts.

#### options.logLevel
Type: `String`
Default: `error`

The CasperJS log level. See [CasperJS: Logging](http://casperjs.readthedocs.org/en/latest/logging.html) for details.


### Usage Examples

#### Basic visual tests
Run tests in `test/visual/` against comparison screenshots stored in `test/visual/screenshots/`, and put the resulting screenshots in `results/visual/`

```javascript
grunt.initConfig({
  phantomcss: {
    options: {
      screenshots: 'test/visual/screenshots/',
      results: 'results/visual/'
    },
    src: [
      'test/visual/**/*.js'
    ]
  }
});
```

#### Responsive layout testing
Run tests in `test/visual/` against comparison screenshots for destop and mobile.

```javascript
grunt.initConfig({
  phantomcss: {
    desktop: {
      options: {
        screenshots: 'test/visual/desktop/',
        results: 'results/visual/desktop',
        viewportSize: [1024, 768]
      },
      src: [
        'test/visual/**.js'
      ]
    },
    mobile: {
      options: {
        screenshots: 'test/visual/mobile/',
        results: 'results/visual/mobile',
        viewportSize: [320, 480]
      },
      src: [
        'test/visual/**.js'
      ]
    }
  },
});
```

#### Sample test file

Test files should do the following:
* You no longer need ```casper.start``` - Use ```thenOpen``` to start CasperJS with the URL you want to test - NOT USE START (casper.start is default)
* Manipulate the page in some way
* Take screenshots

```javascript
casper.thenOpen('http://localhost:3000/')
.then(function() {
  phantomcss.screenshot('#todo-app', 'Main app');
})
.then(function() {
  casper.fill('form.todo-form', {
    todo: 'Item1'
  }, true);

  phantomcss.screenshot('#todo-app', 'Item added');
})
.then(function() {
  casper.click('.todo-done');

  phantomcss.screenshot('#todo-app', 'Item checked off');
});
```

You can also load a local file by specifying a path (relative to the Gruntfile):

```javascript
casper.thenOpen('build/client/index.html')
.then(function() {
  // ...
});
```

### Multiple Test Files
you no longer need ```casper.start```

File 1 - file1.js
```javascript
casper.thenOpen('http://localhost:3000/')
.then(function() {
  phantomcss.screenshot('#todo-app', 'Main app');
})
.then(function() {
  casper.fill('form.todo-form', {
    todo: 'Item1'
  }, true);

  phantomcss.screenshot('#todo-app', 'Item added');
});
```

File 2 - file2.js
```javascript
casper.thenOpen('http://localhost:3000/contact')
.then(function() {
  phantomcss.screenshot('#todo-app-contact', 'Main app');
})
.then(function() {
  casper.fill('form.todo-form-contact', {
    todo: 'Item2'
  }, true);

  phantomcss.screenshot('#todo-app-contact', 'Item added');
});
```

Subsequent files should call ```casper.then``` to continue the previous test.

```javascript
casper.then(function() {
  casper.click('.todo-done');

  phantomcss.screenshot('#todo-app', 'Item checked off');
});
```
You can also use ```casper.thenOpen``` to load a new url and continue testing


See the [CasperJS documentation](http://casperjs.readthedocs.org/en/latest/modules/casper.html) and the [PhantomCSS documentation](https://github.com/Huddle/PhantomCSS) for more information on using CasperJS and PhantomCSS.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
* 2014-02-23   v0.2.8   Update dependencies
* 2014-02-23   v0.2.7   Remove bower, add dependencies node_modules, fix warning msgs
* 2014-02-23   v0.2.2   Added multiple file example to README.md
* 2014-02-07   v0.2.1   Fixed ResembleJS path issue
* 2014-01-07   v0.2.0   Merged updates from Larry Davis
* 2013-10-24   v0.1.1   Added the ability to use an external server
* 2013-10-24   v0.1.0   Initial Release
