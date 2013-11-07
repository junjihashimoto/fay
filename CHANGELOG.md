## Changelog

See full history at: <https://github.com/faylang/fay/commits>

### 0.18.1 (2013-11-07)

* Add support for TupleSections

#### 0.18.0.5 (2013-10-28)

Bugfixes:
* Disallow unsupported patterns in where/let declarations instead of `<<loop>>`ing on them

Minor:
* Put upper bounds on all dependencies

#### 0.18.0.4 (2013-10-25)

Bugfixes:
* Allow `//` as an operator name (added flag to `hse-cpp`)
* Don't transcode function values when using an EmptyDataDecl

#### 0.18.0.3 (2013-10-23)

Minor:
* Allow `optparse-applicative == 0.7.*`
* Fix `examples/Cont.hs`

#### 0.18.0.2 (2013-10-16)

Bug fixes:
* Regression: Work around a bug in optparse-applicative 0.6 that prevents `--strict` from being used.

#### 0.18.0.1 (2013-10-16)

* Source maps for top level definitions, use `--sourcemap`

Bug fixes:
* Regression: Equality checks for (G)ADTs (`deriving Eq`)
* Fix `--strict` for top level ADT values (such as `module M where g = R`)
* Regression: Serialization in the presence of compression/renaming
* Pass NoImplicitPrelude (and other enabled extensions) to haskell-names to resolve ambiguities when Prelude isn't imported.

Minor:
* Bump optparse-applicative to 0.6.*
* Bump haskell-names to 0.3.1 to allow compilation with Cabal 1.14
* Ignore more declarations (useful when code sharing with GHC)

## 0.18.0.0 (2013-09-24)

New features:
* Support for qualified imports. Note: You still can't have multiple constructors with the same name in the FFI since the `instance` field in the serialization is still unqualified.
* `Automatic` transcoding now works for functions. See [[Calling Fay From JavaScript]]
* `--strict modulename[, ..]` generates strict and transcoding wrappers for a module's exports. See [[Calling Fay From JavaScript]]
* `--typecheck-only` just runs the GHC type checker with the appropriate Fay flags.
* `--runtime-path FILEPATH` allows you to supply a custom runtime. Probably only useful for debugging.

Bug fixes:
* Don't crash when trying to get the fayToJsFun of an object without constructor.name
* Fixed bug that accidentally flattened list arguments in `jsToFay`
* Fix construction with RecordWildCards not taking already listed fields into account

Breaking Changes:
* Fay.Compiler.Debug has been removed (for now)
* The interactive compilation mode has been removed (for now)

Internal changes:
* Migrated to haskell-src-ext's annotated AST.
* Name resolution is now done using haskell-names, Fay's name resolution code is now pure and a lot simpler.

### fay-jquery 0.4.0.1

* Fixed bug in definition of `clone`
* Don't define `fromIntegral`.

## 0.17.0.0 (2013-08-27)

* With the `RebindableSyntax` and `OverloadedStrings` extensions Fay will treat Haskell string literals as JavaScript Strings. Add this in all modules and import Fay.Text (from the `fay-text` package). This is *not* a breaking change, without these extensions in a module `String` will be used, as before. All modules can still interoperate normally even if only some of them use this feature. Note that you may have to define `fromInteger` when using this with Num literals.

* The type signature of `Fay.FFI.ffi` (in fay) and `FFI.ffi` (in fay-base) has been generalized to `IsString s => s -> a` to support `RebindableSyntax`.

* Much faster compile time (of the compiler itself) by having the executables depend on the library.

Bugfixes:
* The empty list and unit is now serialized to `null` when using Automatic (it used to throw an error).

Minor:
* Restrict upper bound on `language-ecmascript` to `< 1.0`

#### 0.16.0.3 (2013-08-23)

* Support for tuple constructors (`(,,) 1,2,3`)

Minor:
* Bump `pretty-show` to `>= 1.6`
* Remove the `-fdevel` flag (when compiling fay itself)

#### 0.16.0.2 (2013-08-21)

Minor:
* Bump `haskell-src-exts` to `>= 1.14`

#### 0.16.0.1 (2013-08-08)

Bugfixes:
 * Allow combining multiline strings with CPP

## 0.16.0.0 (2013-08-05)
 * New module generation, modules generate code separately in the format `My.Module.foo` instead of `My$Module$foo`
 * Transcoding information is also produced separately for each module
 * Removed `--naked`, `--dispatcher`, and `--no-dispatcher`. They are probably not needed anymore
 * Removed `Fay$$fayToJsUserDefined` and `Fay$$jsToFayUserDefined`, instead call `Fay$$fayToJs` and `Fay$$jsToFay` respectively
 * Escape semi reserved words from `Object` when printing (`constructor` -> `$constructor`). This only matters if you call Fay from JS
 * `Automatic` now handles lists and tuples (#251)
 * `Language.Fay.FFI` renamed to `Fay.FFI` (as before, Fay code can import `FFI` from `fay-base`)

Minor:
 * Print location of parse errors
 * Compile with -XNoImplicitPrelude
 * Support for testing nested modules (module A, module A.B)
 * Bump `language-ecmascript to >= 0.15` (new API)
 * Rename/remove some CompileErrors
 * All tests are now included in dist

Bug fixes:
 * Force cars in string serialization (#306)

## 0.15.0.0 (2013-06-08)
 * Expression level FFI calls, `ffi "alert('hello!')" :: Fay ()`
 * Support let pattern matches
 * Smaller output for serialization code
 * --base-path flag to use a custom base (mainly for fay-prim)
 * Allow ExistentialQuantification, FlexibleContexts, FlexibleInstances, KindSignatures
 * Verify that GADTs using non-record syntax works
 * JS->Fay function serialization
 * Serialization support for `()`, tuples and `Char`
 * Add more reserved words for Google Closure

Bugfixes:
 * Fix a bug when an imported module contains types
 * Fix a bug with EModuleContents exports
 * Fix where clause inside pattern guards in function definitions
 * Fix type variables in serialization for multiple constructors
 * Fix EThingAll exports for types with constructors with a different name
 * Don't export types in EThingAll and EThingWith


### 0.14.5.0 (2013-04-24)
* Support for newtypes (with no runtime cost!)
* --base-path flag to specify custom locations for fay-base
* Fix a bug where imports shadowing local bindings would prevent the local binding from being exported


### 0.14.4.0 (2013-04-21)
* Fix record updates on IE <= 8
* Import tweaks, will make compilation a lot faster (4x reported) when there are a lot of imports
* Parse hs sources with base fixities