+++
date = "2017-02-25T16:46:24-06:00"
title = "elixirscript 0.26.0 released"
draft = true
+++

For a full list of changes, check out out the [changelog](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md).

This version of Elixirscript has a lot of major changes. Here are some of the changes in the new release:

## Bundled output

This release is the bundle all output into a single file. Now the only file output will be `Elixir.App.js.`

## Removed `@on_js_load`

`@on_js_load` is no more. To start an application, do the following:

```js
//Note: An ES module example. Update for your module output of choice
import Elixir from "./Elixir.App";

const my_inital_args = [];
Elixir.start(Elixir.MyApp, my_inital_args);
```

This looks for a `start/2` function in the `MyApp` module. It tries to mimick the API of a normal Elixir Application.

```elixir
  def start(type, args) do
  end
```

## Removed `JS.import`

`JS.import` is also no more. External JavaScript modules are now defined in configuration. A new configuration, `js_modules` is where they go.

```elixir
js_modules: [
  {React, "react"},
  {ReactDOM, "react-dom"}
]
```

Elixirscript adds both `React` and `ReactDOM`  imports to the top of the bundled output. Each item must be 2-tuple or a 3-tuple. The third element is an optional keyword list of options.

The CLI now has a `js-module` to support the above.

`elixirscript input/path -o output/path --js-module React:react --js-module ReactDOM:react-dom`