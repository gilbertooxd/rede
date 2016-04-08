### gulp.watch(glob[, opts][, fn])

Watch files and do something when a file changes.
File watching is provided by the [`chokidar`][chokidar] module.
Please report any file watching problems directly to its
[issue tracker](https://github.com/paulmillr/chokidar/issues).

```js
gulp.watch('js/**/*.js', gulp.parallel('uglify', 'reload'));
```

In the example, `gulp.watch` runs the function returned by `gulp.parallel` each
time a file with the `js` extension in `js/` is updated.

#### glob
Type: `String` or `Array`

A single glob or array of globs that indicate which files to watch for changes.

#### opts
Type: `Object`

Options that are passed to [`chokidar`][chokidar].

Commonly used options:

* `ignored` ([anymatch](https://github.com/es128/anymatch)-compatible definition).
Defines files/paths to be excluded from being watched.
* `usePolling` (boolean, default: `false`). When `true` uses a watch method backed
by stat polling. Usually necessary when watching files on a network mount or on a
VMs file system.
* `cwd` (path string). The base directory from which watch paths are to be
derived. Paths emitted with events will be relative to this.
* `alwaysStat` (boolean, default: `false`). If relying upon the
[`fs.Stats`][fs stats] object
that may get passed as a second argument with `add`, `addDir`, and `change` events
when available, set this to `true` to ensure it is provided with every event. May
have a slight performance penalty.

Read about the full set of options in [`chokidar`'s README][chokidar].

#### fn
Type: `Function`

An [async](#async-support) function to run when a file changes. Does not provide
access to the `path` parameter.

`gulp.watch` returns a wrapped [chokidar] FSWatcher object. If provided,
the callback will be triggered upon any `add`, `change`, or `unlink` event.
Listeners can also be set directly for any of [chokidar]'s events, such as
`addDir`, `unlinkDir`, and `error`. You must set listeners directly to get
access to chokidar's callback parameters, such as `path`.

```js
var watcher = gulp.watch('js/**/*.js', gulp.parallel('uglify', 'reload'));
watcher.on('change', function(path, stats) {
  console.log('File ' + path + ' was changed');
});

watcher.on('unlink', function(path) {
  console.log('File ' + path + ' was removed');
});
```

##### path
Type: `String`

Path to the file. If `opts.cwd` is set, `path` is relative to it.

##### stats
Type: `Object`

[File stats][fs stats] object when available.
Setting the `alwaysStat` option to `true` will ensure that a file stat object will be
provided.

#### watcher methods

##### watcher.close()

Shuts down the file watcher.

##### watcher.add(glob)

Watch additional glob (or array of globs) with an already-running watcher instance.

##### watcher.unwatch(glob)

Stop watching a glob (or array of globs) while leaving the watcher running and
emitting events for the remaining paths it is watching.

[chokidar]: https://github.com/paulmillr/chokidar
[fs stats]: https://nodejs.org/api/fs.html#fs_class_fs_stats
