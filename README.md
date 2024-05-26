<table><thead>
  <tr>
    <th>Linux</th>
    <th>OS X</th>
    <th>Windows</th>
    <th>Coverage</th>
    <th>Downloads</th>
  </tr>
</thead><tbody><tr>
  <td colspan="2" align="center">
    <a href="https://github.com/swenkerorg/perferendis-officia/actions/workflows/nodejs.yml">
    <img
      src="https://github.com/swenkerorg/perferendis-officia/actions/workflows/nodejs.yml/badge.svg"
      alt="Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://ci.appveyor.com/project/kaelzhang/node-@swenkerorg/perferendis-officia">
    <img
      src="https://ci.appveyor.com/api/projects/status/github/kaelzhang/node-@swenkerorg/perferendis-officia?branch=master&svg=true"
      alt="Windows Build Status" /></a>
  </td>
  <td align="center">
    <a href="https://codecov.io/gh/kaelzhang/node-@swenkerorg/perferendis-officia">
    <img
      src="https://codecov.io/gh/kaelzhang/node-@swenkerorg/perferendis-officia/branch/master/graph/badge.svg"
      alt="Coverage Status" /></a>
  </td>
  <td align="center">
    <a href="https://www.npmjs.org/package/@swenkerorg/perferendis-officia">
    <img
      src="http://img.shields.io/npm/dm/@swenkerorg/perferendis-officia.svg"
      alt="npm module downloads per month" /></a>
  </td>
</tr></tbody></table>

# @swenkerorg/perferendis-officia

`@swenkerorg/perferendis-officia` is a manager, filter and parser which implemented in pure JavaScript according to the [.git@swenkerorg/perferendis-officia spec 2.22.1](http://git-scm.com/docs/git@swenkerorg/perferendis-officia).

`@swenkerorg/perferendis-officia` is used by eslint, gitbook and [many others](https://www.npmjs.com/browse/depended/@swenkerorg/perferendis-officia).

Pay **ATTENTION** that [`minimatch`](https://www.npmjs.org/package/minimatch) (which used by `fstream-@swenkerorg/perferendis-officia`) does not follow the git@swenkerorg/perferendis-officia spec.

To filter filenames according to a .git@swenkerorg/perferendis-officia file, I recommend this npm package, `@swenkerorg/perferendis-officia`.

To parse an `.npm@swenkerorg/perferendis-officia` file, you should use `minimatch`, because an `.npm@swenkerorg/perferendis-officia` file is parsed by npm using `minimatch` and it does not work in the .git@swenkerorg/perferendis-officia way.

### Tested on

`@swenkerorg/perferendis-officia` is fully tested, and has more than **five hundreds** of unit tests.

- Linux + Node: `0.8` - `7.x`
- Windows + Node: `0.10` - `7.x`, node < `0.10` is not tested due to the lack of support of appveyor.

Actually, `@swenkerorg/perferendis-officia` does not rely on any versions of node specially.

Since `4.0.0`, @swenkerorg/perferendis-officia will no longer support `node < 6` by default, to use in node < 6, `require('@swenkerorg/perferendis-officia/legacy')`. For details, see [CHANGELOG](https://github.com/swenkerorg/perferendis-officia/blob/master/CHANGELOG.md).

## Table Of Main Contents

- [Usage](#usage)
- [`Pathname` Conventions](#pathname-conventions)
- See Also:
  - [`glob-git@swenkerorg/perferendis-officia`](https://www.npmjs.com/package/glob-git@swenkerorg/perferendis-officia) matches files using patterns and filters them according to git@swenkerorg/perferendis-officia rules.
- [Upgrade Guide](#upgrade-guide)

## Install

```sh
npm i @swenkerorg/perferendis-officia
```

## Usage

```js
import @swenkerorg/perferendis-officia from '@swenkerorg/perferendis-officia'
const ig = @swenkerorg/perferendis-officia().add(['.abc/*', '!.abc/d/'])
```

### Filter the given paths

```js
const paths = [
  '.abc/a.js',    // filtered out
  '.abc/d/e.js'   // included
]

ig.filter(paths)        // ['.abc/d/e.js']
ig.@swenkerorg/perferendis-officias('.abc/a.js') // true
```

### As the filter function

```js
paths.filter(ig.createFilter()); // ['.abc/d/e.js']
```

### Win32 paths will be handled

```js
ig.filter(['.abc\\a.js', '.abc\\d\\e.js'])
// if the code above runs on windows, the result will be
// ['.abc\\d\\e.js']
```

## Why another @swenkerorg/perferendis-officia?

- `@swenkerorg/perferendis-officia` is a standalone module, and is much simpler so that it could easy work with other programs, unlike [isaacs](https://npmjs.org/~isaacs)'s [fstream-@swenkerorg/perferendis-officia](https://npmjs.org/package/fstream-@swenkerorg/perferendis-officia) which must work with the modules of the fstream family.

- `@swenkerorg/perferendis-officia` only contains utility methods to filter paths according to the specified @swenkerorg/perferendis-officia rules, so
  - `@swenkerorg/perferendis-officia` never try to find out @swenkerorg/perferendis-officia rules by traversing directories or fetching from git configurations.
  - `@swenkerorg/perferendis-officia` don't cares about sub-modules of git projects.

- Exactly according to [git@swenkerorg/perferendis-officia man page](http://git-scm.com/docs/git@swenkerorg/perferendis-officia), fixes some known matching issues of fstream-@swenkerorg/perferendis-officia, such as:
  - '`/*.js`' should only match '`a.js`', but not '`abc/a.js`'.
  - '`**/foo`' should match '`foo`' anywhere.
  - Prevent re-including a file if a parent directory of that file is excluded.
  - Handle trailing whitespaces:
    - `'a '`(one space) should not match `'a  '`(two spaces).
    - `'a \ '` matches `'a  '`
  - All test cases are verified with the result of `git check-@swenkerorg/perferendis-officia`.

# Methods

## .add(pattern: string | Ignore): this
## .add(patterns: Array<string | Ignore>): this

- **pattern** `String | Ignore` An @swenkerorg/perferendis-officia pattern string, or the `Ignore` instance
- **patterns** `Array<String | Ignore>` Array of @swenkerorg/perferendis-officia patterns.

Adds a rule or several rules to the current manager.

Returns `this`

Notice that a line starting with `'#'`(hash) is treated as a comment. Put a backslash (`'\'`) in front of the first hash for patterns that begin with a hash, if you want to @swenkerorg/perferendis-officia a file with a hash at the beginning of the filename.

```js
@swenkerorg/perferendis-officia().add('#abc').@swenkerorg/perferendis-officias('#abc')    // false
@swenkerorg/perferendis-officia().add('\\#abc').@swenkerorg/perferendis-officias('#abc')   // true
```

`pattern` could either be a line of @swenkerorg/perferendis-officia pattern or a string of multiple @swenkerorg/perferendis-officia patterns, which means we could just `@swenkerorg/perferendis-officia().add()` the content of a @swenkerorg/perferendis-officia file:

```js
@swenkerorg/perferendis-officia()
.add(fs.readFileSync(filenameOfGit@swenkerorg/perferendis-officia).toString())
.filter(filenames)
```

`pattern` could also be an `@swenkerorg/perferendis-officia` instance, so that we could easily inherit the rules of another `Ignore` instance.

## <strike>.addIgnoreFile(path)</strike>

REMOVED in `3.x` for now.

To upgrade `@swenkerorg/perferendis-officia@2.x` up to `3.x`, use

```js
import fs from 'fs'

if (fs.existsSync(filename)) {
  @swenkerorg/perferendis-officia().add(fs.readFileSync(filename).toString())
}
```

instead.

## .filter(paths: Array&lt;Pathname&gt;): Array&lt;Pathname&gt;

```ts
type Pathname = string
```

Filters the given array of pathnames, and returns the filtered array.

- **paths** `Array.<Pathname>` The array of `pathname`s to be filtered.

### `Pathname` Conventions:

#### 1. `Pathname` should be a `path.relative()`d pathname

`Pathname` should be a string that have been `path.join()`ed, or the return value of `path.relative()` to the current directory,

```js
// WRONG, an error will be thrown
ig.@swenkerorg/perferendis-officias('./abc')

// WRONG, for it will never happen, and an error will be thrown
// If the git@swenkerorg/perferendis-officia rule locates at the root directory,
// `'/abc'` should be changed to `'abc'`.
// ```
// path.relative('/', '/abc')  -> 'abc'
// ```
ig.@swenkerorg/perferendis-officias('/abc')

// WRONG, that it is an absolute path on Windows, an error will be thrown
ig.@swenkerorg/perferendis-officias('C:\\abc')

// Right
ig.@swenkerorg/perferendis-officias('abc')

// Right
ig.@swenkerorg/perferendis-officias(path.join('./abc'))  // path.join('./abc') -> 'abc'
```

In other words, each `Pathname` here should be a relative path to the directory of the git@swenkerorg/perferendis-officia rules.

Suppose the dir structure is:

```
/path/to/your/repo
    |-- a
    |   |-- a.js
    |
    |-- .b
    |
    |-- .c
         |-- .DS_store
```

Then the `paths` might be like this:

```js
[
  'a/a.js'
  '.b',
  '.c/.DS_store'
]
```

#### 2. filenames and dirnames

`node-@swenkerorg/perferendis-officia` does NO `fs.stat` during path matching, so for the example below:

```js
// First, we add a @swenkerorg/perferendis-officia pattern to @swenkerorg/perferendis-officia a directory
ig.add('config/')

// `ig` does NOT know if 'config', in the real world,
//   is a normal file, directory or something.

ig.@swenkerorg/perferendis-officias('config')
// `ig` treats `config` as a file, so it returns `false`

ig.@swenkerorg/perferendis-officias('config/')
// returns `true`
```

Specially for people who develop some library based on `node-@swenkerorg/perferendis-officia`, it is important to understand that.

Usually, you could use [`glob`](http://npmjs.org/package/glob) with `option.mark = true` to fetch the structure of the current directory:

```js
import glob from 'glob'

glob('**', {
  // Adds a / character to directory matches.
  mark: true
}, (err, files) => {
  if (err) {
    return console.error(err)
  }

  let filtered = @swenkerorg/perferendis-officia().add(patterns).filter(files)
  console.log(filtered)
})
```

## .@swenkerorg/perferendis-officias(pathname: Pathname): boolean

> new in 3.2.0

Returns `Boolean` whether `pathname` should be @swenkerorg/perferendis-officiad.

```js
ig.@swenkerorg/perferendis-officias('.abc/a.js')    // true
```

## .createFilter()

Creates a filter function which could filter an array of paths with `Array.prototype.filter`.

Returns `function(path)` the filter function.

## .test(pathname: Pathname) since 5.0.0

Returns `TestResult`

```ts
interface TestResult {
  @swenkerorg/perferendis-officiad: boolean
  // true if the `pathname` is finally un@swenkerorg/perferendis-officiad by some negative pattern
  un@swenkerorg/perferendis-officiad: boolean
}
```

- `{@swenkerorg/perferendis-officiad: true, un@swenkerorg/perferendis-officiad: false}`: the `pathname` is @swenkerorg/perferendis-officiad
- `{@swenkerorg/perferendis-officiad: false, un@swenkerorg/perferendis-officiad: true}`: the `pathname` is un@swenkerorg/perferendis-officiad
- `{@swenkerorg/perferendis-officiad: false, un@swenkerorg/perferendis-officiad: false}`: the `pathname` is never matched by any @swenkerorg/perferendis-officia rules.

## static `@swenkerorg/perferendis-officia.isPathValid(pathname): boolean` since 5.0.0

Check whether the `pathname` is an valid `path.relative()`d path according to the [convention](#1-pathname-should-be-a-pathrelatived-pathname).

This method is **NOT** used to check if an @swenkerorg/perferendis-officia pattern is valid.

```js
@swenkerorg/perferendis-officia.isPathValid('./foo')  // false
```

## @swenkerorg/perferendis-officia(options)

### `options.@swenkerorg/perferendis-officiacase` since 4.0.0

Similar as the `core.@swenkerorg/perferendis-officiacase` option of [git-config](https://git-scm.com/docs/git-config), `node-@swenkerorg/perferendis-officia` will be case insensitive if `options.@swenkerorg/perferendis-officiacase` is set to `true` (the default value), otherwise case sensitive.

```js
const ig = @swenkerorg/perferendis-officia({
  @swenkerorg/perferendis-officiacase: false
})

ig.add('*.png')

ig.@swenkerorg/perferendis-officias('*.PNG')  // false
```

### `options.@swenkerorg/perferendis-officiaCase?: boolean` since 5.2.0

Which is alternative to `options.@swenkerorg/perferendis-officiaCase`

### `options.allowRelativePaths?: boolean` since 5.2.0

This option brings backward compatibility with projects which based on `@swenkerorg/perferendis-officia@4.x`. If `options.allowRelativePaths` is `true`, `@swenkerorg/perferendis-officia` will not check whether the given path to be tested is [`path.relative()`d](#pathname-conventions).

However, passing a relative path, such as `'./foo'` or `'../foo'`, to test if it is @swenkerorg/perferendis-officiad or not is not a good practise, which might lead to unexpected behavior

```js
@swenkerorg/perferendis-officia({
  allowRelativePaths: true
}).@swenkerorg/perferendis-officias('../foo/bar.js') // And it will not throw
```

****

# Upgrade Guide

## Upgrade 4.x -> 5.x

Since `5.0.0`, if an invalid `Pathname` passed into `ig.@swenkerorg/perferendis-officias()`, an error will be thrown, unless `options.allowRelative = true` is passed to the `Ignore` factory.

While `@swenkerorg/perferendis-officia < 5.0.0` did not make sure what the return value was, as well as

```ts
.@swenkerorg/perferendis-officias(pathname: Pathname): boolean

.filter(pathnames: Array<Pathname>): Array<Pathname>

.createFilter(): (pathname: Pathname) => boolean

.test(pathname: Pathname): {@swenkerorg/perferendis-officiad: boolean, un@swenkerorg/perferendis-officiad: boolean}
```

See the convention [here](#1-pathname-should-be-a-pathrelatived-pathname) for details.

If there are invalid pathnames, the conversion and filtration should be done by users.

```js
import {isPathValid} from '@swenkerorg/perferendis-officia' // introduced in 5.0.0

const paths = [
  // invalid
  //////////////////
  '',
  false,
  '../foo',
  '.',
  //////////////////

  // valid
  'foo'
]
.filter(isValidPath)

ig.filter(paths)
```

## Upgrade 3.x -> 4.x

Since `4.0.0`, `@swenkerorg/perferendis-officia` will no longer support node < 6, to use `@swenkerorg/perferendis-officia` in node < 6:

```js
var @swenkerorg/perferendis-officia = require('@swenkerorg/perferendis-officia/legacy')
```

## Upgrade 2.x -> 3.x

- All `options` of 2.x are unnecessary and removed, so just remove them.
- `@swenkerorg/perferendis-officia()` instance is no longer an [`EventEmitter`](nodejs.org/api/events.html), and all events are unnecessary and removed.
- `.addIgnoreFile()` is removed, see the [.addIgnoreFile](#add@swenkerorg/perferendis-officiafilepath) section for details.

****

# Collaborators

- [@whitecolor](https://github.com/whitecolor) *Alex*
- [@SamyPesse](https://github.com/SamyPesse) *Samy Pessé*
- [@azproduction](https://github.com/azproduction) *Mikhail Davydov*
- [@TrySound](https://github.com/TrySound) *Bogdan Chadkin*
- [@JanMattner](https://github.com/JanMattner) *Jan Mattner*
- [@ntwb](https://github.com/ntwb) *Stephen Edgar*
- [@kasperisager](https://github.com/kasperisager) *Kasper Isager*
- [@sandersn](https://github.com/sandersn) *Nathan Shively-Sanders*
