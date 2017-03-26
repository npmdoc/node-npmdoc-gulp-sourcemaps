# api documentation for  [gulp-sourcemaps (v2.4.1)](http://github.com/floridoo/gulp-sourcemaps)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-sourcemaps.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-sourcemaps)
#### Source map support for Gulp.js

[![NPM](https://nodei.co/npm/gulp-sourcemaps.png?downloads=true)](https://www.npmjs.com/package/gulp-sourcemaps)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-sourcemaps_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-gulp-sourcemaps/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "Florian Reiterer",
        "email": "me@florianreiterer.com"
    },
    "bugs": {
        "url": "https://github.com/floridoo/gulp-sourcemaps/issues"
    },
    "dependencies": {
        "acorn": "4.X",
        "convert-source-map": "1.X",
        "css": "2.X",
        "debug-fabulous": "0.0.X",
        "detect-newline": "2.X",
        "graceful-fs": "4.X",
        "source-map": "0.X",
        "strip-bom": "3.X",
        "through2": "2.X",
        "vinyl": "1.X"
    },
    "description": "Source map support for Gulp.js",
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
        "object-assign": "^4.1.0",
        "tape": "4.X",
        "yargs": "6.6.0"
    },
    "directories": {},
    "dist": {
        "shasum": "8f65dc5c0d07b2fd5c88bc60ec7f13e56716bf74",
        "tarball": "https://registry.npmjs.org/gulp-sourcemaps/-/gulp-sourcemaps-2.4.1.tgz"
    },
    "engines": {
        "node": ">=4"
    },
    "files": [
        "index.js",
        "src"
    ],
    "gitHead": "8166cbe72fc2c731b0a98242f5e6b1a055fd5ca2",
    "homepage": "http://github.com/floridoo/gulp-sourcemaps",
    "keywords": [
        "gulpplugin",
        "gulp",
        "source maps",
        "sourcemaps"
    ],
    "license": "ISC",
    "main": "index.js",
    "maintainers": [
        {
            "name": "nmccready",
            "email": "nemtcan@gmail.com"
        }
    ],
    "name": "gulp-sourcemaps",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/floridoo/gulp-sourcemaps.git"
    },
    "scripts": {
        "cover": "istanbul cover --dir reports/coverage tape \"test/*.js\"",
        "coveralls": "istanbul cover tape \"test/*.js\" --report lcovonly && cat ./coverage/lcov.info | coveralls && rm -rf ./coverage",
        "lint": "jshint ./src/**/*.js test/*.js",
        "serve": "http-server",
        "tap": "tape test/*.js",
        "test": "npm run lint && faucet test/*.js $@",
        "test:int": "rm -rf ./tmp && tape ./test/integration.js"
    },
    "version": "2.4.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-sourcemaps](#apidoc.module.gulp-sourcemaps)
1.  [function <span class="apidocSignatureSpan">gulp-sourcemaps.</span>init (options)](#apidoc.element.gulp-sourcemaps.init)
1.  [function <span class="apidocSignatureSpan">gulp-sourcemaps.</span>write (destPath, options)](#apidoc.element.gulp-sourcemaps.write)



# <a name="apidoc.module.gulp-sourcemaps"></a>[module gulp-sourcemaps](#apidoc.module.gulp-sourcemaps)

#### <a name="apidoc.element.gulp-sourcemaps.init"></a>[function <span class="apidocSignatureSpan">gulp-sourcemaps.</span>init (options)](#apidoc.element.gulp-sourcemaps.init)
- description and source-code
```javascript
function init(options) {
  var debug = require('debug-fabulous')()(PLUGIN_NAME + ':init');

  function sourceMapInit(file, encoding, callback) {
<span class="apidocCodeCommentSpan">    /*jshint validthis:true */
</span>
    // pass through if file is null or already has a source map
    if (file.isNull() || file.sourceMap) {
      this.push(file);
      return callback();
    }

    if (file.isStream()) {
      return callback(new Error(PLUGIN_NAME + '-init: Streaming not supported'));
    }

    if (options === undefined) {
      options = {};
    }
    debug(function() {
      return options;
    });

    var fileContent = file.contents.toString();
    var sourceMap, preExistingComment;
    var internals = initInternals(options, file, fileContent);

    if (options.loadMaps) {
      var result = internals.loadMaps();
      sourceMap = result.map;
      fileContent = result.content;
      preExistingComment = result.preExistingComment;
    }

    if (!sourceMap && options.identityMap) {
      debug(function() {
        return 'identityMap';
      });
      var fileType = path.extname(file.path);
      var source = unixStylePath(file.relative);
      var generator = new SourceMapGenerator({file: source});

      if (fileType === '.js') {
        var tokenizer = acorn.tokenizer(fileContent, {locations: true});
        while (true) {
          var token = tokenizer.getToken();
          if (token.type.label === "eof")
            break;
          var mapping = {
            original: token.loc.start,
            generated: token.loc.start,
            source: source
          };
          if (token.type.label === 'name') {
            mapping.name = token.value;
          }
          generator.addMapping(mapping);
        }
        generator.setSourceContent(source, fileContent);
        sourceMap = generator.toJSON();

      } else if (fileType === '.css') {
        debug('css');
        var ast = css.parse(fileContent, {silent: true});
        debug(function() {
          return ast;
        });
        var registerTokens = function(ast) {
          if (ast.position) {
            generator.addMapping({original: ast.position.start, generated: ast.position.start, source: source});
          }

          function logAst(key, ast) {
            debug(function() {
              return 'key: ' + key;
            });
            debug(function() {
              return ast[key];
            });
          }

          for (var key in ast) {
            logAst(key, ast);
            if (key !== "position") {
              if (Object.prototype.toString.call(ast[key]) === '[object Object]') {
                registerTokens(ast[key]);
              } else if (Array.isArray(ast[key])) {
                debug(function() {
                  return "@@@@ ast[key] isArray @@@@";
                });
                for (var i = 0; i < ast[key].length; i++) {
                  registerTokens(ast[key][i]);
                }
              }
            }
          }
        };
        registerTokens(ast);
        generator.setSourceContent(source, fileContent);
        sourceMap = generator.toJSON();
      }
    }

    if (!sourceMap) {
      // Make an empty source map
      sourceMap = {
        version: 3,
        names: [],
        mappings: '',
        sources: [unixStylePath(file.relative)],
        sourcesContent: [fileContent]
      };
    }
    else if(preExistingComment !== null && typeof preExistingComment !== 'undefined')
      sourceMap.preExistingComment = preExistingComment;

    sourceMap.file = unixStylePath(file.relative);
    file.sourceMap = sourceMap;

    this.push(file);
    callback();
  }

  return through.obj(sourceMapInit);
}
```
- example usage
```shell
...
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
'''
...
```

#### <a name="apidoc.element.gulp-sourcemaps.write"></a>[function <span class="apidocSignatureSpan">gulp-sourcemaps.</span>write (destPath, options)](#apidoc.element.gulp-sourcemaps.write)
- description and source-code
```javascript
function write(destPath, options) {
  var debug = require('debug-fabulous')()(PLUGIN_NAME + ':write');

  debug(utils.logCb("destPath"));
  debug(utils.logCb(destPath));

  debug(utils.logCb("original options"));
  debug(utils.logCb(options));

  if (options === undefined && typeof destPath !== 'string') {
    options = destPath;
    destPath = undefined;
  }
  options = options || {};

  // set defaults for options if unset
  if (options.includeContent === undefined)
    options.includeContent = true;
  if (options.addComment === undefined)
    options.addComment = true;
  if (options.charset === undefined)
    options.charset = "utf8";

  debug(utils.logCb("derrived options"));
  debug(utils.logCb(options));

  var internals = internalsInit(destPath, options);

  function sourceMapWrite(file, encoding, callback) {
<span class="apidocCodeCommentSpan">    /*jshint validthis:true */
</span>
    if (file.isNull() || !file.sourceMap) {
      this.push(file);
      return callback();
    }

    if (file.isStream()) {
      return callback(new Error(PLUGIN_NAME + '-write: Streaming not supported'));
    }

    // fix paths if Windows style paths
    file.sourceMap.file = unixStylePath(file.relative);

    internals.setSourceRoot(file);
    internals.loadContent(file);
    internals.mapSources(file);
    internals.mapDestPath(file, this);

    this.push(file);
    callback();
  }



  return through.obj(sourceMapWrite);
}
```
- example usage
```shell
...
var sourcemaps = require('gulp-sourcemaps');

gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
'''

All plugins between 'sourcemaps.init()' and 'sourcemaps.write()' need to have support for 'gulp-sourcemaps'. You can find a list
 of such plugins in the [wiki](https://github.com/floridoo/gulp-sourcemaps/wiki/Plugins-with-gulp-sourcemaps-support).
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
