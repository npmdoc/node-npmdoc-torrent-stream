# api documentation for  [torrent-stream (v1.0.3)](https://github.com/mafintosh/torrent-stream)  [![npm package](https://img.shields.io/npm/v/npmdoc-torrent-stream.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-torrent-stream) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-torrent-stream.svg)](https://travis-ci.org/npmdoc/node-npmdoc-torrent-stream)
#### Low level streaming torrent client that exposes files as node.js streams and downloads pieces based on demand

[![NPM](https://nodei.co/npm/torrent-stream.png?downloads=true)](https://www.npmjs.com/package/torrent-stream)

[![apidoc](https://npmdoc.github.io/node-npmdoc-torrent-stream/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-torrent-stream_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-torrent-stream/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-torrent-stream/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-torrent-stream/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/mafintosh/torrent-stream/issues"
    },
    "dependencies": {
        "bitfield": "^0.1.0",
        "bncode": "^0.5.2",
        "end-of-stream": "^0.1.4",
        "fs-chunk-store": "^1.3.0",
        "hat": "0.0.3",
        "immediate-chunk-store": "^1.0.5",
        "ip-set": "^1.0.0",
        "mkdirp": "^0.3.5",
        "parse-torrent": "^4.0.0",
        "peer-wire-swarm": "^0.12.0",
        "rimraf": "^2.2.5",
        "torrent-discovery": "^5.2.0",
        "torrent-piece": "^1.0.0"
    },
    "description": "Low level streaming torrent client that exposes files as node.js streams and downloads pieces based on demand",
    "devDependencies": {
        "bittorrent-tracker": "^2.6.0",
        "fs-extra": "^0.26.4",
        "standard": "^5.1.0",
        "tap": "^0.4.8"
    },
    "directories": {},
    "dist": {
        "shasum": "d8c043b44c3c448c9397a3aec42d2df55887037b",
        "tarball": "https://registry.npmjs.org/torrent-stream/-/torrent-stream-1.0.3.tgz"
    },
    "gitHead": "025bd62df93cd0accccd720f28209770424aa4f2",
    "homepage": "https://github.com/mafintosh/torrent-stream",
    "maintainers": [
        {
            "name": "mafintosh",
            "email": "mathiasbuus@gmail.com"
        },
        {
            "name": "ralphtheninja",
            "email": "ralphtheninja@riseup.net"
        }
    ],
    "name": "torrent-stream",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/mafintosh/torrent-stream.git"
    },
    "scripts": {
        "test": "standard && tap test/*.js"
    },
    "version": "1.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module torrent-stream](#apidoc.module.torrent-stream)
1.  [function <span class="apidocSignatureSpan">torrent-stream.</span>file_stream (engine, file, opts)](#apidoc.element.torrent-stream.file_stream)
1.  object <span class="apidocSignatureSpan">torrent-stream.</span>file_stream.prototype

#### [module torrent-stream.file_stream](#apidoc.module.torrent-stream.file_stream)
1.  [function <span class="apidocSignatureSpan">torrent-stream.</span>file_stream (engine, file, opts)](#apidoc.element.torrent-stream.file_stream.file_stream)
1.  [function <span class="apidocSignatureSpan">torrent-stream.file_stream.</span>super_ (options)](#apidoc.element.torrent-stream.file_stream.super_)

#### [module torrent-stream.file_stream.prototype](#apidoc.module.torrent-stream.file_stream.prototype)
1.  [function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>_read ()](#apidoc.element.torrent-stream.file_stream.prototype._read)
1.  [function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>destroy ()](#apidoc.element.torrent-stream.file_stream.prototype.destroy)
1.  [function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>notify ()](#apidoc.element.torrent-stream.file_stream.prototype.notify)



# <a name="apidoc.module.torrent-stream"></a>[module torrent-stream](#apidoc.module.torrent-stream)

#### <a name="apidoc.element.torrent-stream.file_stream"></a>[function <span class="apidocSignatureSpan">torrent-stream.</span>file_stream (engine, file, opts)](#apidoc.element.torrent-stream.file_stream)
- description and source-code
```javascript
file_stream = function (engine, file, opts) {
  if (!(this instanceof FileStream)) return new FileStream(engine, file, opts)
  stream.Readable.call(this)

  if (!opts) opts = {}
  if (!opts.start) opts.start = 0
  if (!opts.end && typeof opts.end !== 'number') opts.end = file.length - 1

  var offset = opts.start + file.offset
  var pieceLength = engine.torrent.pieceLength

  this.length = opts.end - opts.start + 1
  this.startPiece = (offset / pieceLength) | 0
  this.endPiece = ((opts.end + file.offset) / pieceLength) | 0
  this._destroyed = false
  this._engine = engine
  this._piece = this.startPiece
  this._missing = this.length
  this._reading = false
  this._notifying = false
  this._critical = Math.min(1024 * 1024 / pieceLength, 2) | 0
  this._offset = offset - this.startPiece * pieceLength
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.torrent-stream.file_stream"></a>[module torrent-stream.file_stream](#apidoc.module.torrent-stream.file_stream)

#### <a name="apidoc.element.torrent-stream.file_stream.file_stream"></a>[function <span class="apidocSignatureSpan">torrent-stream.</span>file_stream (engine, file, opts)](#apidoc.element.torrent-stream.file_stream.file_stream)
- description and source-code
```javascript
file_stream = function (engine, file, opts) {
  if (!(this instanceof FileStream)) return new FileStream(engine, file, opts)
  stream.Readable.call(this)

  if (!opts) opts = {}
  if (!opts.start) opts.start = 0
  if (!opts.end && typeof opts.end !== 'number') opts.end = file.length - 1

  var offset = opts.start + file.offset
  var pieceLength = engine.torrent.pieceLength

  this.length = opts.end - opts.start + 1
  this.startPiece = (offset / pieceLength) | 0
  this.endPiece = ((opts.end + file.offset) / pieceLength) | 0
  this._destroyed = false
  this._engine = engine
  this._piece = this.startPiece
  this._missing = this.length
  this._reading = false
  this._notifying = false
  this._critical = Math.min(1024 * 1024 / pieceLength, 2) | 0
  this._offset = offset - this.startPiece * pieceLength
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.torrent-stream.file_stream.super_"></a>[function <span class="apidocSignatureSpan">torrent-stream.file_stream.</span>super_ (options)](#apidoc.element.torrent-stream.file_stream.super_)
- description and source-code
```javascript
function Readable(options) {
  if (!(this instanceof Readable))
    return new Readable(options);

  this._readableState = new ReadableState(options, this);

  // legacy
  this.readable = true;

  if (options && typeof options.read === 'function')
    this._read = options.read;

  Stream.call(this);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.torrent-stream.file_stream.prototype"></a>[module torrent-stream.file_stream.prototype](#apidoc.module.torrent-stream.file_stream.prototype)

#### <a name="apidoc.element.torrent-stream.file_stream.prototype._read"></a>[function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>_read ()](#apidoc.element.torrent-stream.file_stream.prototype._read)
- description and source-code
```javascript
_read = function () {
  if (this._reading) return
  this._reading = true
  this.notify()
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.torrent-stream.file_stream.prototype.destroy"></a>[function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>destroy ()](#apidoc.element.torrent-stream.file_stream.prototype.destroy)
- description and source-code
```javascript
destroy = function () {
  if (this._destroyed) return
  this._destroyed = true
  this.emit('close')
}
```
- example usage
```shell
...

Emitted when all selected files have been completely downloaded.

#### 'engine.files[...]'

An array of all files in the torrent. See the file section for more info on what methods the file has

#### 'engine.destroy(cb)'

Destroy the engine. Destroys all connections to peers

#### 'engine.connect('127.0.0.0:6881')'

Connect to a peer manually
...
```

#### <a name="apidoc.element.torrent-stream.file_stream.prototype.notify"></a>[function <span class="apidocSignatureSpan">torrent-stream.file_stream.prototype.</span>notify ()](#apidoc.element.torrent-stream.file_stream.prototype.notify)
- description and source-code
```javascript
notify = function () {
  if (!this._reading || !this._missing) return
  if (!this._engine.bitfield.get(this._piece)) return this._engine.critical(this._piece, this._critical)

  var self = this

  if (this._notifying) return
  this._notifying = true
  this._engine.store.get(this._piece++, function (err, buffer) {
    self._notifying = false

    if (self._destroyed || !self._reading) return

    if (err) return self.destroy(err)

    if (self._offset) {
      buffer = buffer.slice(self._offset)
      self._offset = 0
    }

    if (self._missing < buffer.length) buffer = buffer.slice(0, self._missing)

    self._missing -= buffer.length

    if (!self._missing) {
      self.push(buffer)
      self.push(null)
      return
    }

    self._reading = false
    self.push(buffer)
  })
}
```
- example usage
```shell
...
    var gc = function () {
      for (var i = 0; i < engine.selection.length; i++) {
var s = engine.selection[i]
var oldOffset = s.offset

while (!pieces[s.from + s.offset] && s.from + s.offset < s.to) s.offset++

if (oldOffset !== s.offset) s.notify()
if (s.to !== s.from + s.offset) continue
if (pieces[s.from + s.offset]) continue

engine.selection.splice(i, 1)
i-- // -1 to offset splice
s.notify()
oninterestchange()
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
