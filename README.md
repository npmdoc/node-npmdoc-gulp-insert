# api documentation for  [gulp-insert (v0.5.0)](https://github.com/rschmukler/gulp-insert/)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-insert.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-insert) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-insert.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-insert)
#### Append or Prepend a string with gulp

[![NPM](https://nodei.co/npm/gulp-insert.png?downloads=true)](https://www.npmjs.com/package/gulp-insert)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-insert/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-insert_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-insert/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-insert/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-insert/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Ryan Schmukler",
        "email": "ryan@slingingcode.com",
        "url": "http://slingingcode.com/"
    },
    "bugs": {
        "url": "https://github.com/rschmukler/gulp-insert/issues"
    },
    "dependencies": {
        "readable-stream": "^1.0.26-4",
        "streamqueue": "0.0.6"
    },
    "description": "Append or Prepend a string with gulp",
    "devDependencies": {
        "chai": "^1.9.1",
        "gulp-util": "~2.2.12",
        "mocha": "~1.17.0"
    },
    "directories": {},
    "dist": {
        "shasum": "32313f13e4a23cf5acca5ce5f0c080923c778602",
        "tarball": "https://registry.npmjs.org/gulp-insert/-/gulp-insert-0.5.0.tgz"
    },
    "gitHead": "8d35acb4fbf402cac02143b477ba1b4ca5f14b6a",
    "homepage": "https://github.com/rschmukler/gulp-insert/",
    "keywords": [
        "gulp",
        "gulpplugin",
        "append",
        "insert",
        "prepend"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "rschmukler",
            "email": "ryan@slingingcode.com"
        },
        {
            "name": "nfroidure",
            "email": "nfroidure@elitwork.com"
        }
    ],
    "name": "gulp-insert",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/rschmukler/gulp-insert.git"
    },
    "scripts": {
        "test": "mocha test/*.js"
    },
    "version": "0.5.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-insert](#apidoc.module.gulp-insert)
1.  [function <span class="apidocSignatureSpan">gulp-insert.</span>append (append)](#apidoc.element.gulp-insert.append)
1.  [function <span class="apidocSignatureSpan">gulp-insert.</span>prepend (prepend)](#apidoc.element.gulp-insert.prepend)
1.  [function <span class="apidocSignatureSpan">gulp-insert.</span>transform (fn)](#apidoc.element.gulp-insert.transform)
1.  [function <span class="apidocSignatureSpan">gulp-insert.</span>wrap (begin, end)](#apidoc.element.gulp-insert.wrap)



# <a name="apidoc.module.gulp-insert"></a>[module gulp-insert](#apidoc.module.gulp-insert)

#### <a name="apidoc.element.gulp-insert.append"></a>[function <span class="apidocSignatureSpan">gulp-insert.</span>append (append)](#apidoc.element.gulp-insert.append)
- description and source-code
```javascript
append = function (append) {
  var stream = new Stream.Transform({objectMode: true});

  stream._transform = function(file, unused, cb) {
    if(file.isNull()) {
      return cb(null, file);
    }
    var appendedBuffer = new Buffer(getInsertString(append, file));
    if(file.isStream()) {
      file.contents = new StreamQueue(
        file.contents,
        getStreamFromBuffer(appendedBuffer)
      );
      return cb(null, file);
    }
    file.contents = Buffer.concat([file.contents, appendedBuffer],
      appendedBuffer.length + file.contents.length);
    cb(null, file);
  };

  return stream;
}
```
- example usage
```shell
...
'''

## Append

Appends a string onto the contents.

'''js
.pipe(insert.append('world')); // Appends 'world' to the contents of every file
'''

## Prepend

Prepends a string onto the contents.

'''js
...
```

#### <a name="apidoc.element.gulp-insert.prepend"></a>[function <span class="apidocSignatureSpan">gulp-insert.</span>prepend (prepend)](#apidoc.element.gulp-insert.prepend)
- description and source-code
```javascript
prepend = function (prepend) {
  var stream = new Stream.Transform({objectMode: true});

  stream._transform = function(file, unused, cb) {
    if(file.isNull()) {
      return cb(null, file);
    }
    var prependedBuffer = new Buffer(getInsertString(prepend, file));
    if(file.isStream()) {
      file.contents = new StreamQueue(
        getStreamFromBuffer(prependedBuffer),
        file.contents
      );
      return cb(null, file);
    }
    file.contents = Buffer.concat([prependedBuffer, file.contents],
      prependedBuffer.length + file.contents.length);
    cb(null, file);
  };

  return stream;
}
```
- example usage
```shell
...
'''

## Prepend

Prepends a string onto the contents.

'''js
.pipe(insert.prepend('Hello')); // Prepends 'Hello' to the contents of every file
'''
## Wrap

Wraps the contents with two strings.

'''js
.pipe(insert.wrap('Hello', 'World')); // prepends 'hello' and appends 'world' to the contents
...
```

#### <a name="apidoc.element.gulp-insert.transform"></a>[function <span class="apidocSignatureSpan">gulp-insert.</span>transform (fn)](#apidoc.element.gulp-insert.transform)
- description and source-code
```javascript
transform = function (fn) {
  var stream = new Stream.Transform({objectMode: true});

  stream._transform = function(file, unused, cb) {
    if(file.isNull()) {
      return cb(null, file);
    }
    if(file.isStream()) {
      file.contents = file.contents.pipe(new Stream.Transform());
      file.contents._transform = function(chunk, encoding, cb) {
        cb(null, new Buffer(fn(chunk.toString(), file)))
      };
      return cb(null, file);
    }
    file.contents = new Buffer(fn(file.contents.toString(), file));
    cb(null, file);
  };

  return stream;
}
```
- example usage
```shell
...
'''

## Transform

Calls a function with the contents of the file.

'''js
.pipe(insert.transform(function(contents, file) {
  return contents.toUpperCase();
}));
'''

Transform has access to the underlying vinyl file. The following code adds a '//' comment with the full file name before the actual
 content.

'''js
...
```

#### <a name="apidoc.element.gulp-insert.wrap"></a>[function <span class="apidocSignatureSpan">gulp-insert.</span>wrap (begin, end)](#apidoc.element.gulp-insert.wrap)
- description and source-code
```javascript
wrap = function (begin, end) {
  var stream = new Stream.Transform({objectMode: true});

  stream._transform = function(file, unused, cb) {
    if(file.isNull()) {
      return cb(null, file);
    }
    var prependedBuffer = new Buffer(getInsertString(begin, file));
    var appendedBuffer = new Buffer(getInsertString(end, file));
    if(file.isStream()) {
      file.contents = new StreamQueue(
        getStreamFromBuffer(prependedBuffer),
        file.contents,
        getStreamFromBuffer(appendedBuffer)
      );
      return cb(null, file);
    }
    file.contents = Buffer.concat([prependedBuffer, file.contents, appendedBuffer],
      appendedBuffer.length + file.contents.length + prependedBuffer.length);
    cb(null, file);
  };

  return stream;
}
```
- example usage
```shell
...
.pipe(insert.prepend('Hello')); // Prepends 'Hello' to the contents of every file
'''
## Wrap

Wraps the contents with two strings.

'''js
.pipe(insert.wrap('Hello', 'World')); // prepends 'hello' and appends 'world' to the contents
'''

## Transform

Calls a function with the contents of the file.

'''js
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
