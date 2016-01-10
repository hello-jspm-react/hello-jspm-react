
Walkthrough for [hello-jspm-react-base](https://github.com/hello-jspm-react/hello-jspm-react-base) Jspm + React.js Base Boilerplate

Using:

- JSPM 0.16
- Gulp 3.9
- React 0.14

Usage of **Eslint** and **EditorConfig** is recommended, but not mandatory.

# 0. Preparation

## 0.1. Install **Node.js** and **npm**
See [https://docs.npmjs.com/getting-started/installing-node](https://docs.npmjs.com/getting-started/installing-node)

## 0.2. Install **jspm**

```shell
$ sudo npm install -g jspm
```

## 0.3. Install **gulp**

```shell
$ sudo npm install -g gulp
```

# 1. Create basic **npm**/**jspm** project

I prefer to edit package.json myself

## 1.1. Create basic **npm** project

```shell
$ mkdir hello-app
$ cd hello-app
```

## 1.2. Create "package.json"

```json
{
  "name": "hello-app",
  "version": "0.0.1",
  "private": true,
  "description": "Hello Application"
}
```

## 1.3. Add **jspm**

```shell
$ npm install jspm --save-dev
$ jspm init
```

_Last command will ask you some questions. Choose default answers._

## 1.4. Create ".gitignore"

```
jspm_packages
node_modules
npm-debug.log
```

**_Commit_**: [10db387](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/10db387fcdfe32f8e2e684a4e7f1f35687689f5c)

# 2. Adjust project for development

## 2.1. Install **babel**, **eslint** and **gulp**

```shell
$ npm install --save-dev babel-core
$ npm install --save-dev babel-preset-es2015
$ npm install --save-dev eslint
$ npm install --save-dev eslint-config-airbnb
$ npm install --save-dev eslint-plugin-react
$ npm install --save-dev gulp
```

## 2.2. Create ".babelrc"

```javascript
{
  "presets": ["es2015"]
}
```

## 2.3. Create ".eslintrc"

```json
{
  "extends": "airbnb",
  "rules": {
    "comma-dangle": [2, "never"],
    "space-before-function-paren": [2, "never"],
    "max-len": [2, 120, 4]
  }
}
```

## 2.4. Create ".editorconfig"

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
```

**_Commit_**: [68b7f3c](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/68b7f3c63e24a4d5488d4684394849f19101ebb0)

# 3. Prepare project for server serving

## 3.1. Install dev dependencies

```shell
$ npm install --save-dev express
```

## 3.2. Create "gulp/config.js"

```js
const config = {};

config.ports = {
  dev: 3000
};

export default config;
```

## 3.3. Create "gulp/server.js"

```js
import express from 'express';

import config from './config';

export function serveDev() {
  const app = express();
  app.use(express.static('.'));
  app.listen(config.ports.dev, () => {
    console.log('express listening on %s', config.ports.dev);
  });
}
```

## 3.4. Create "gulpfile.babel.js"

```js
import gulp from 'gulp';

import * as server from './gulp/server';

gulp.task('serve:dev', () => { server.serveDev(); });
```

## 3.5. Add npm script to launch server in "package.json"

```json
...
  "description": "Hello Application",
  "scripts": {
    "start": "gulp serve:dev"
  },
  "devDependencies": {
  }
...
```

## 3.6. Create "index.html"

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Hello Application</title>
</head>
<body>
  <div>Hello World!</div>
</body>
</html>
```

## 3.7. Test

```shell
$ npm start
```

Navigate to [http://localhost:3000](http://localhost:3000) and verify that "Hello World!" is shown on your screen

**_Commit_**: [884fc04](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/884fc0430f905cb6ccd675de7ff426103468fbfc)

# 4. "Hello World" from **Jspm**

## 4.1. Create "src/client/main.js"

```js
console.log('Hello World!');
```

## 4.2. Modify "index.html"

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Hello Application</title>
</head>
<body>
  <div>Hello World!</div>
  <script src="/jspm_packages/system.js"></script>
  <script src="/config.js"></script>
  <script>
     System.import('/src/client/main');
  </script>
</body>
</html>
```

## 4.3. Test

```shell
$ npm start
```

Navigate to [http://localhost:3000](http://localhost:3000) and verify that "Hello World!" is shown on your screen and in console panel

**_Commit_**: [2120174](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/212017448ce54e1514158d7f270f3db93ee9910d)

# 5. Rewrite "Hello World" with React

## 5.1. Add jspm dependencies

```
$ jspm install react
$ jspm install react-dom
```

## 5.2. Create "src/client/HelloWorld.js"

```js
import React from 'react';

export default class HelloWorld extends React.Component {
  render() {
    return (
      <h1>Hello World!</h1>
    );
  }
}
```

## 5.3. Rewrite "src/client/main.js"

```js
import React from 'react';
import ReactDOM from 'react-dom';

import HelloWorld from './HelloWorld';

ReactDOM.render(
  <HelloWorld />,
    document.getElementById('app')
);
```

## 5.4. Modify "index.html"

```html
...
<body>
  <div id="app"></div>
  <script src="/jspm_packages/system.js"></script>
...
```

## 5.5. Test

```shell
$ npm start
```

Navigate to [http://localhost:3000](http://localhost:3000) and verify that "Hello World!" is shown on your screen

**_Commit_**: [f7ed4c7](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/f7ed4c7c875ec8010e4da3f91c2320bf3b80b19f)

# 6. Add Hot Loading for dev environment

## 6.1. Install dev dependencies

```shell
$ npm install --save-dev gulp-sequence
$ jspm install github:capaj/systemjs-hot-reloader
$ npm install --save-dev chokidar-socket-emitter
```

## 6.2. Modify "gulp/config.js"

```js
const config = {};

config.ports = {
  dev: 3000,
  watch: 9111
};

config.paths = {
  src: {}
};

config.paths.src.base = 'src';

export default config;
```

## 6.3. Modify "gulp/server.js"

```js
import express from 'express';
import watcher from 'chokidar-socket-emitter';

import config from './config';

export function serveDev() {
  const app = express();
  app.use(express.static('.'));
  app.listen(config.ports.dev, () => {
    console.log('express listening on %s', config.ports.dev);
  });
}

export function serveWatch() {
  watcher({ port: config.ports.watch, path: config.paths.src.base });
}
```

## 6.4. Modify "gulpfile.babel.js"

```js
import gulp from 'gulp';
import run from 'gulp-sequence';

import * as server from './gulp/server';

gulp.task('serve:dev', () => { server.serveDev(); });
gulp.task('serve:watch', () => { server.serveWatch(); });

gulp.task('serve', (done) => {
  run(['serve:dev', 'serve:watch'], done);
});
```

## 6.5. Add Hot Loading support to "index.html"

```html
...
  <script src="/config.js"></script>
  <script>
    System.trace = true;
    readyForMainLoad = System.import('capaj/systemjs-hot-reloader').then(function(HotReloader) {
      hr = new HotReloader.default('http://localhost:9111');
    });

    Promise.resolve(readyForMainLoad).then(() => {
     System.import('/src/client/main').then(function () {
       console.log('ran at ', new Date());
     })
    })
  </script>
```

## 6.6. Modify "package.json"

```json
  "scripts": {
    "start": "gulp serve"
  },
```

## 6.7. Test

```
$ npm start
```

Navigate to [http://localhost:3000](http://localhost:3000) then try to edit "src/client/HelloWorld.js" by changing existing message (don't forget to save file if your editor not support auto save) and verify in your browser that "Hello World!" message was changed

**_Commit_**: [1f38184](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/1f38184880f181a515d4bc18eb5c293affdfe4ce)

# 7. Production bundle: JSPM bundle

## 7.1. Install dev dependencies

```
$ npm install --save-dev path
$ npm install --save-dev gulp-jspm-build
```

## 7.2. Modify "gulp/config.js"

```js
import path from 'path';

const config = {};

config.ports = {
  dev: 3000,
  watch: 9111
};

config.paths = {
  src: {},
  dist: {}
};

config.paths.src.base = 'src';
config.paths.src.main = path.join(config.paths.src.base, 'client', 'main');
config.paths.src.systemjs = ['jspm_packages/system.js'];

config.paths.dist.base = 'dist';
config.paths.dist.js = path.join(config.paths.dist.base, 'js');
config.paths.dist.app = 'app.js';

export default config;
```

## 7.3. Create "gulp/scripts.js"

```js
import gulp from 'gulp';
import jspm from 'gulp-jspm-build';

import config from './config';

export function dist() {
  return jspm(
    {
      bundleOptions: {
        minify: true,
        mangle: true
      },
      bundles: [
        { src: config.paths.src.main, dst: config.paths.dist.app }
      ]
    })
    .pipe(gulp.dest(config.paths.dist.js));
}

export function systemjs() {
  return gulp.src(config.paths.src.systemjs)
    .pipe(gulp.dest(config.paths.dist.js));
}
```

## 7.4. Modify "gulpfile.babel.js"

```
import gulp from 'gulp';
import run from 'gulp-sequence';

import * as server from './gulp/server';
import * as scripts from './gulp/scripts';

gulp.task('serve:dev', () => { server.serveDev(); });
gulp.task('serve:watch', () => { server.serveWatch(); });

gulp.task('serve', (done) => {
  run(['serve:dev', 'serve:watch'], done);
});

gulp.task('js:dist', () => { scripts.dist(); });
gulp.task('js:systemjs', () => { scripts.systemjs(); });

gulp.task('build', (done) => {
  run(['js:dist', 'js:systemjs'], done);
});
```

## 7.5. Modify ".gitignore"

```
jspm_packages
node_modules
npm-debug.log
dist
```
 
## 7.6. Test

```shell
$ gulp build
```

**_Commit_**: [94c011f](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/94c011f2f082f034afe09eaf1babf5847d67aa88)

# 8. Production bundle: Clean task

## 8.1 Install dev dependencies

```shell
$ npm install --save-dev del
```

## 8.2. Create "gulp/clean.js"

```js
import del from 'del';

import config from './config';

export default function() {
  return del.sync([
    config.paths.dist.base + '/**/*'
  ]);
}
```

## 8.3. Modify "gulpfile.babel.js"

```js
...
import clean from './gulp/clean';
import * as server from './gulp/server';
import * as scripts from './gulp/scripts';

gulp.task('clean', () => { clean(); });

...
gulp.task('build', (done) => {
  run('clean', ['js:dist', 'js:systemjs'], done);
});
```

## 8.4. Test

```shell
$ gulp clean
```

Should remove all files from "dist" directory

**_Commit_**: [c5aa9b5](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/c5aa9b5e18b019d579220377243a9c5a3a35d16e)

# 9. Production bundle: Build HTML entry file

## 9.1. Install dev dependencies

```shell
$ npm install --save-dev gulp-html-replace
```

## 9.2. Modify "gulp/config.js"

```js
...
config.paths.src.base = 'src';
config.paths.src.main = path.join(config.paths.src.base, 'client', 'main');
config.paths.src.systemjs = ['jspm_packages/system.js'];
config.paths.src.html = 'index.html';

config.paths.dist.base = 'dist';
config.paths.dist.js = path.join(config.paths.dist.base, 'js');
config.paths.dist.app = 'app.js';

config.htmlReplace = {
  js: ['/js/system.js', '/js/config.js', '/js/app.js'],
  entry_point: `<script>System.import("${config.paths.src.main}");</script>`
};

export default config;
```

## 9.3. Create "gulp/html.js"

```js
import gulp from 'gulp';
import htmlreplace from 'gulp-html-replace';

import config from './config';

export function dist() {
  return gulp.src(config.paths.src.html)
    .pipe(htmlreplace(config.htmlReplace))
    .pipe(gulp.dest(config.paths.dist.html));
}
```

## 9.4. Modify "gulpfile.babel.js"

```js
...
import * as server from './gulp/server';
import * as scripts from './gulp/scripts';
import * as html from './gulp/html';

...

gulp.task('js:dist', () => { scripts.dist(); });
gulp.task('js:systemjs', () => { scripts.systemjs(); });

gulp.task('html:dist', () => { html.dist(); });

gulp.task('build', (done) => {
  run('clean', ['js:dist', 'js:systemjs', 'html:dist'], done);
});
```

## 9.5. Modify "index.html"

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>Hello Application</title>
</head>
<body>
  <div id="app"></div>
  <!-- build:js -->
  <script src="/jspm_packages/system.js"></script>
  <script src="/config.js"></script>
  <script>
    System.trace = true;
    readyForMainLoad = System.import('capaj/systemjs-hot-reloader').then(function(HotReloader) {
      hr = new HotReloader.default('http://localhost:9111');
    });

    Promise.resolve(readyForMainLoad).then(() => {
     System.import('/src/client/main').then(function () {
       console.log('ran at ', new Date());
     })
    })
  </script>
  <!-- endbuild -->

  <!-- build:entry_point-->
  <!-- endbuild -->
</body>
</html>
```

## 9.6. Test

```shell
$ gulp html:dist
```

Should create "index.html" in "dist" directory

**_Commit_**: [64f3b56](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/64f3b5698a396d0b53e3632c97217ac1ed6fc932)

# 10. Production bundle: Move libs to its own bundle

## 10.1. Modify "gulp/config.js"

```js
...
config.paths.src.html = 'index.html';
config.paths.src.libs = 'react + react-dom';
config.paths.src.libsRev = 'react - react-dom';

config.paths.dist.base = 'dist';
config.paths.dist.js = path.join(config.paths.dist.base, 'js');
config.paths.dist.app = 'app.js';
config.paths.dist.lib = 'lib.js';

config.htmlReplace = {
  js: ['/js/system.js', '/js/config.js', '/js/lib.js', '/js/app.js'],
  entry_point: `<script>System.import("${config.paths.src.main}");</script>`
};

export default config;
```

## 10.2. Modify "gulp/scripts.js"

```js
...
      bundles: [
        { src: config.paths.src.main + ' - ' + config.paths.src.libsRev, dst: config.paths.dist.app },
        { src: config.paths.src.libs, dst: config.paths.dist.lib }
      ]
    })
    .pipe(gulp.dest(config.paths.dist.js));
...
```

## 10.3. Test

```shell
$ gulp build
```

Should create "lib.js" in "dist/js" directory

**_Commit_**: [4c3165d](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/4c3165d78034d2200c1f480203858b9677ce1fc5)

# 11. Production bundle: Serve distribution

## 11.1. Modify "gulp/config.js"

```json
...
config.ports = {
  dev: 3000,
  watch: 9111,
  dist: 8080
};
...
```

## 11.2. Modify "gulp/server.js"

```js
...
export function serveDist() {
  const app = express();
  app.use(express.static(config.paths.dist.base));
  app.listen(config.ports.dist, () => {
    console.log('express listening on %s', config.ports.dist);
  });
}
```

## 11.3. Modify "gulpfile.babel.js"

```js
...
gulp.task('serve:dev', () => { server.serveDev(); });
gulp.task('serve:watch', () => { server.serveWatch(); });
gulp.task('serve:dist', () => { server.serveDist(); });

...

gulp.task('dist', (done) => {
  run('build', 'serve:dist', done);
});
```

## 11.4. Test

```shell
$ gulp dist
```

Navigate to [http://localhost:8080](http://localhost:8080) and verify that "Hello World!" is shown on your screen

**_Commit_**: [9ba1d25](https://github.com/hello-jspm-react/hello-jspm-react-base/tree/9ba1d25e8d9dc1832bbd1063180137531e1f550c)
