## electron-compile

<a href="http://electronjs.github.io/electron-compile/docs">
![](http://electronjs.github.io/electron-compile/docs/badge.svg)
</a>

electron-compile compiles JS and CSS on the fly with a single call in your app's 'ready' function.

For JavaScript:

* JavaScript ES6/ES7 (via Babel)
* TypeScript
* CoffeeScript

For CSS:

* LESS

For HTML:

* Jade

### How does it work? (Easiest Way)

Change your reference to `electron-prebuilt` to `electron-prebuilt-compile`. Tada! You did it.

### Wait, seriously?

Yeah. `electron-prebuilt-compile` is like an `electron-prebuilt` that Just Works with all of these languages above.

### How does it work? (Slightly Harder Way)

First, add `electron-compile` and `electron-compilers` as a `devDependency`.

```sh
npm install --save electron-compile
npm install --save-dev electron-compilers
```

Create a new file that will be the entry point of your app (perhaps changing 'main' in package.json) - you need to pass in the root directory of your application, which will vary based on your setup. The root directory is the directory that your `package.json` is in.

```js
// Assuming this file is ./src/es6-init.js
var appRoot = path.join(__dirname, '..');

// ...and that your main app is called ./src/main.js. This is written as if
// you were going to `require` the file from here.
require('electron-compile').init(appRoot, './main');
```


### I did it, now what?

From then on, you can now simply include files directly in your HTML, no need for cross-compilation:

```html
<head>
  <script type="text/coffeescript" src="main.coffee"></script>
  <link rel="stylesheet" type="text/less" href="main.less" />
</head>
```

or just require them in:

```js
require('./mylib')   // mylib.ts
```

### How do I set up (Babel / LESS / whatever) the way I want?

If you've got a .babelrc and that's all you want to customize, you can simply use it directly. electron-compile will respect it, even the environment-specific settings. If you want to customize other compilers, use a `.compilerc` file. Here's an example:

```js
{
  "application/javascript": {
    "presets": ["stage-0", "es2015", "react"],
    "sourceMaps": "inline"
  },
  "text/less": {
    "dumpLineNumbers": "comments"
  }
}
```

The opening Object is a list of MIME Types, and options passed to the compiler implementation. These parameters are documented here:

* Babel - http://babeljs.io/docs/usage/options
* CoffeeScript - http://coffeescript.org/documentation/docs/coffee-script.html#section-5
* TypeScript - https://github.com/Microsoft/TypeScript/blob/v1.5.0-beta/bin/typescriptServices.d.ts#L1076 
* LESS - http://lesscss.org/usage/index.html#command-line-usage-options
* Jade - http://jade-lang.com/api

## How can I precompile my code for release-time?

electron-compile comes with a command-line application to pre-create a cache for you.

```sh
Usage: electron-compile --appDir [root-app-dir] paths...

Options:
  -a, --appdir  The top-level application directory (i.e. where your
                package.json is)
  -v, --verbose  Print verbose information
  -h, --help     Show help
```

Run `electron-compile` on all of your application assets, even if they aren't strictly code (i.e. your static assets like PNGs). electron-compile will recursively walk the given directories.

```sh
electron-compile --appDir . ./src ./static
```

### But I use Grunt / Gulp / I want to do Something Interesting

Compilation also has its own API, check out the [documentation](http://electronjs.github.io/electron-compile/docs/badge.svg) for more information.