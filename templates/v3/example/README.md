# Front End Styleguide

Living Styleguide for componentized front end development.

Uses the [Gulp](http://gulpjs.com/) task runner to compile and [lint](https://stylelint.io/) [Sass](http://sass-lang.com/), [lint](http://eslint.org/) and [transpile](https://babeljs.io/) ES6 Code and create static HTML from [Nunjucks](https://mozilla.github.io/nunjucks/).


## Contents
1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Usage](#usage)
  1. [Tasks](#tasks)
  2. [CSS](#css)
  3. [JavaScript](#javascript)
  4. [HTML](#html)
  5. [Images and Icons](#images-and-icons)
  6. [Copy](#copy)
5. [Credits](#credits)


## Prerequisites
* [Node.js with npm](https://nodejs.org/)
* [Front End Styleguide](https://github.com/mvsde/styleguide)


## Installation
Make sure CLI is installed globally with `npm install -g front-end-styleguide-cli`.
If you don't want to or cannot install the CLI please use `./node_modules/.bin/front-end-styleguide` instead of `front-end-styleguide`.

Run `npm install` to get all dependencies. `npm update` will install and update all dependencies.

*To check for outdated packages without updating them run `npm outdated`.*


## Configuration

### Styleguide
`config/config.json`

```js
{
  "css": {
    "dev": {
     /* Sass options */
    },
    "dist": {
      /* Sass options */
    },
    "autoprefixer": {
      /* Autoprefixer options */
    }
  },
  "html": {
    "browsersync": {
      /* Browsersync options */
    }
  },
  "img": {
    "svgSpriteDev": {
      /* SVG Sprite options */
    },
    "svgSpriteDist": {
      /* SVG Sprite options */
    },
    "imagemin": {
      /* Imagemin options */
    }
  }
}
```

`config/paths.json`

```json
{
  "output": {
    "css": {
      "path": "css",
      "name": "styles.css"
    },
    "js": {
      "path": "js",
      "name": "scripts.js"
    },
    "img": {
      "path": "img",
      "icons": "icons.svg"
    }
  }
}
```

### Dotfiles

* `.babelrc`: [Configuration for Babel](https://babeljs.io/docs/usage/babelrc/)
* `.editorconfig`: [Set basic rules for editors/IDEs](http://editorconfig.org/)
* `.eslintignore`: [Files ignored by ESLint](http://eslint.org/docs/user-guide/configuring#ignoring-files-and-directories)
* `.eslintrc.json`: [Configuration and rules for ESLint](http://eslint.org/docs/user-guide/configuring)
* `.stylelintrc.json`: [Configuration and rules for Stylelint](https://stylelint.io/user-guide/configuration/)


## Folder Structure
```
.
├── config
|   ├── components
|   |   |── example
|   |   |   |── default.guide.njk
|   |   |   |── default.njk
|   |   |   |── scripts.js
|   |   |   └── styles.scss
|   |   └── icons
|   |       |── usage.guide.svg
|   |       └── menu.svg
|   ├── images
|   |   └── img-name.png
|   ├── layouts
|   |   |── components.njk
|   |   └── prototypes.njk
|   ├── prototypes
|   |   └── index.njk
|   ├── setup
|   |   |── scaffolding.scss
|   |   └── variables.scss
|   ├── copy.js
|   ├── main.js
|   └── main.scss
├── .babelrc
├── .editorconfig
├── .eslintignore
├── .eslint.rc
└── .stylelintrc.json
```


## Usage

### Tasks
These are the main tasks:
* `front-end-styleguide` to start the default task.
  * Watches for file changes.
  * Starts Browsersync.
* `front-end-styleguide development` to start the default task
  * No file watching / Browsersync.
* `front-end-styleguide preview` to create a prototype preview.
  * Minifies CSS, JavaScript and images.
  * Doesn't generate component HTML.
  * Errors break pipe.
* `front-end-styleguide production` to create production ready files.
  * Minifies CSS, JavaScript and images.
  * Doesn't generate any HTML.
  * Errors break pipe.

There are more tasks available for standalone execution:
* `css:dev`, `css:prev` and `css:dist` for Sass compilation.
* `css:lint` and `css:lint:break` for CSS linting.
* `js:dev`, `js:prev` and `js:dist` for JavaScript concatenation.
* `js:lint` and `js:lint:break` for JavaScript linting.
* `html:dev` and `html:prev` for static HTML file generation.
* `img:dev`, `img:prev` and `img:dist` for image copying and icon sprite generation.
* `copy:dev`, `copy:prev` and `copy:dist` for copying files from Node modules.

*The generated folders `dev`, `prev` and `dist` are excluded from Git.*


### CSS
Output to `dev/css`, `prev/css` or `dist/css`.

[Sass](http://sass-lang.com/) is a CSS preprocessor supporting variables, nesting and mixins – among many other features. For a quick start jump to the [Sass Basics](http://sass-lang.com/guide). [stylelint](http://stylelint.io/) monitors the code for errors and consistency deviations.

This styleguide splits the CSS into small parts. This ensures a better organization of style declarations. Each component sits in it's own folder and is re-usable across the project.

The function `@import` includes Sass or CSS files in the main Sass file. The final output is one large CSS file to minimize browser requests. See `src/main.scss` for more information.

Global [stylelint rules](http://stylelint.io/user-guide/rules/) are set in `.stylelintrc.json`. Per-file rules can be set comments (e.g. `/* stylelint-enable selector-no-id */`). With a `.stylelintignore` file in the project root, CSS files can be [excluded from linting](http://stylelint.io/user-guide/configuration/#stylelintignore).

*The development task generates sourcemaps. The preview and production tasks minify the CSS.*


### JavaScript
Output to `dev/js`, `prev/js` or `dist/js`.

JavaScript files are bundled together with [Browserify](http://browserify.org/) and transpiled with [Babel](https://babeljs.io/) and the ES2015 preset. To learn more about JavaScript modules head over to the [MDN article about `import`](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Statements/import). [ESLint](http://eslint.org) checks if the code follows the [JavaScript Standard Style](https://standardjs.com/).

Global [ESLint rules](http://eslint.org/docs/rules/) are set in `.eslintrc.json`. Per-file rules can be set with comments (e.g. `/* eslint no-console: "off" */`). With a `.eslintignore` file in the project root, JavaScript files can be [excluded from linting](http://eslint.org/docs/user-guide/configuring#ignoring-files-and-directories).

*The development task generates sourcemaps. The preview and production tasks uglify the source.*


### HTML
Output to `dev` and `dev/components` or `prev`.

[Nunjucks](https://mozilla.github.io/nunjucks/) is an HTML templating engine based on JavaScript. The styleguide creates static HTML from Nunjucks files. Take a look at the [templating docs](https://mozilla.github.io/nunjucks/templating.html) for further information.


#### Components
Component pages are references for UI elements. They will be generated only if the component folder contains at least on `*.guide.njk` file. Each of these files represent a section within the component page, ordered by filename. The task `preview` will not render component pages.

Each `component-name/*.guide.njk` file should follow this pattern for a styled frame:
```njk
{% set title = "Default" %}
{% set description = "Just a little component." %}

{% extends sgSection %}

{% block body %}
  {% include "./default.njk" %}
{% endblock %}
```

The following variables can be used:
* [optional] `title` sets a heading for the component variation.
* [optional] `description` provides additional information.
* [optional] `background` injects `style="background: value"` into the component.
* [optional] `padding` sets the padding.


#### Prototypes
Prototypes represent final pages. The prototypes folder support hierarchy with subfolders. Go ahead and try out a basic website structure!

The `default` and `development` tasks inject a navigation bar into both component and prototype HTML files for faster site switching and additional settings.


#### Default Variables

* `meta.version`: Current styleguide version (e.g. "1.7.0").
* `meta.env`: `development` or `production`.
* `meta.path.fileName`: The name of the current page file with extension (e.g. "index.html").
* `meta.path.filePath`: Relative path containing the filename of the current page (e.g. "components/header.html").
* `meta.path.toRoot`: Relative path to the root (e.g. "../").



### Images and Icons
Output to `dev/img`, `prev/img` or `dist/img`.

All files and folders placed in `src/images` will be copied to `dev/img`, `prev/img` or `dist/img`.

SVG files placed in the `src/components/icons` folder will be transformed into an [SVG icon sprite](https://github.com/jkphl/gulp-svg-sprite) named `icons.svg`. The original icons will *not* be copied to output folders. SVG symbols can be referenced with their ID. The icon workflow creates IDs from the filename of the original SVG placed in `src/components/icons`. Each ID is suffixed with "-icon" for better compatibility with browsers that need a polyfill.

Icons can be used in HTML with the following syntax:
```html
<svg width="24" height="24">
  <use xlink:href="img/icons.svg#filename-icon"></use>
</svg>
```

Specifying dimensions is considered good style. Without a defined width and height browsers often scale SVGs to fill the whole viewport. This happens if dimensions set in CSS fail to load.

The styleguide ships with [svgxuse](https://github.com/Keyamoon/svgxuse), a polyfill for browsers that do not support external SVG reference.

*The preview and production tasks minify images with a lossless compressor.*


### Copy
Files that don't fit in the above mentioned categories can be integrated into the styleguide with the file `src/copy.js`. To integrate files from Node modules, simply install the module with `npm install --save-dev module-name` and add files or folders to `copy.js`.

`copy.js` contains an array of objects. Each object is a copy instruction and has a `folder`, `files`, `dest` and optional `exclude` key.

* `folder` is the base path.
* `files` specifies which files and folders will be copied. The folder structure will be kept (e.g. `dist/**` results in `dev/js/dist/**`). Use [globs](https://github.com/isaacs/node-glob#glob-primer) to copy more than one file.
* `dest` sets the destination for the copy process. The development, preview and production tasks each prefix the destination with their specific output folders (e.g. `dev/` for development). Base path variables from `config/paths.js` can be used (`path.css.base`, `path.js.base`, `path.html.base` and `path.img.base`). But feel free to set arbitrary destinations.
* [optional] `excludes` contains a string or an array of strings with names for tasks that will not perform the copy instruction. Available task names are `dev`, `prev` and `dist`.

Beware of overwriting files from other tasks (e.g. `css/styles.css`), the copy task is started last. Due to asynchronous task execution the exact write order is unknown.

#### Copy single files
```javascript
module.exports = [
  {
    folder: 'node_modules/svgxuse',
    files: 'svgxuse.{js,min.js}',
    dest: paths.js.base,
    exclude: 'dist'
  }
];
```
This declaration copies the files `node_modules/svgxuse/svgxuse.js` and `node_modules/svgxuse/svgxuse.min.js` to `taskfolder/js/svgxuse.js` and `taskfolder/js/svgxuse.min.js`. `taskfolder` is either `dev` or `prev` depending on the task. The `dist` task is excluded.

#### Preserve original folder structure
```javascript
module.exports = [
  {
    folder: 'node_modules/@polymer',
    files: '**',
    dest: 'polymer'
  }
];
```
Everything from `node_modules/@polymer` will be copied to `taskfolder/polymer`. Subfolders will be kept as is.

#### Remove original folder structure
```javascript
module.exports = [
  {
    folder: 'node_modules/@polymer/font-roboto',
    files: '**',
    dest: 'polymer'
  }
];
```
Everything from `node_modules/@polymer/font-roboto` will be copied to `taskfolder/polymer`. No `font-roboto` folder will be created.


## Credits

* [Material Icons by Google](https://material.io/icons/)