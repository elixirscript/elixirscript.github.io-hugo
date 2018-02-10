+++
title = "Elixirscript 0.32.0 Released"
date = 2018-02-10T10:10:21-06:00
draft = false
+++

[ElixirScript 0.32](https://hex.pm/packages/elixir_script) is released. This release includes one major addition and a number of important changes.

## ElixirScript.Test

ElixirScript.Test is a framework for testing Elixir modules that interact with JavaScript via the FFI. For all other modules, ExUnit is still recommended. ElixirScript.Test's API is similar to ExUnit's API. ElixirScript.Test files must be placed in a folder named `test_elixir_script`. Tests are compiled and then are executed using node.js.

## Changes

Changes for this release include:

* ElixirScript now requires Elixir 1.6
* `mix clean` now correctly cleans up ElixirScript output
* Compiler now outputs a JavaScript file per Elixir module.
* Modules with a `start/2` function must be started directly.

  ```elixir
  # Before ElixirScript 0.32.0:
  import Elixir from './elixirscript.build.js'
  Elixir.start(Elixir.Main, [1, 2, 3])

  # ElixirScript 0.32.0 and later:
  import Main from './Elixir.Main.js'
  Main.start(Symbol.for('normal'), [1, 2, 3])
  ```

For more information regarding changes, please check the [changelog](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md).
