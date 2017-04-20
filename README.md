# npmdoc-gulp-sourcemaps

#### api documentation for  [gulp-sourcemaps (v2.6.0)](http://github.com/gulp-sourcemaps/gulp-sourcemaps)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-sourcemaps.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-sourcemaps) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-sourcemaps.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-sourcemaps)

#### Source map support for Gulp.js

[![NPM](https://nodei.co/npm/gulp-sourcemaps.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/gulp-sourcemaps)

- [https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "name": "gulp-sourcemaps",
    "version": "2.6.0",
    "description": "Source map support for Gulp.js",
    "homepage": "http://github.com/gulp-sourcemaps/gulp-sourcemaps",
    "repository": "git://github.com/gulp-sourcemaps/gulp-sourcemaps.git",
    "main": "index.js",
    "scripts": {
        "lint": "jshint ./src/**/*.js test/*.js",
        "test": "npm run lint && faucet test/*.js $@",
        "tap": "tape test/*.js",
        "cover": "istanbul cover --dir reports/coverage tape \"test/*.js\"",
        "coveralls": "istanbul cover tape \"test/*.js\" --report lcovonly && cat ./coverage/lcov.info | coveralls && rm -rf ./coverage",
        "serve": "http-server",
        "test:int": "rm -rf ./tmp && tape ./test/integration.js"
    },
    "keywords": [
        "gulpplugin",
        "gulp",
        "source maps",
        "sourcemaps"
    ],
    "author": "Florian Reiterer <me@florianreiterer.com>",
    "license": "ISC",
    "dependencies": {
        "@gulp-sourcemaps/identity-map": "1.X",
        "@gulp-sourcemaps/map-sources": "1.X",
        "acorn": "4.X",
        "convert-source-map": "1.X",
        "css": "2.X",
        "debug-fabulous": "0.1.X",
        "detect-newline": "2.X",
        "graceful-fs": "4.X",
        "source-map": "0.X",
        "strip-bom-string": "1.X",
        "through2": "2.X",
        "vinyl": "1.X"
    },
    "devDependencies": {
        "bootstrap": "3.3.7",
        "coveralls": "2.X",
        "faucet": "0.0.X",
        "gulp": "3.X",
        "gulp-concat": "2.X",
        "gulp-if": "2.0.2",
        "gulp-less": "3.3.0",
        "gulp-load-plugins": "1.X",
        "hook-std": "0.2.X",
        "http-server": "0.9.0",
        "istanbul": "0.X",
        "jshint": "2.X",
        "lodash": "4.17.4",
        "mississippi": "^1.3.0",
        "object-assign": "^4.1.0",
        "tape": "4.X",
        "yargs": "6.6.0"
    },
    "files": [
        "index.js",
        "src"
    ],
    "engines": {
        "node": ">=4"
    }
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
