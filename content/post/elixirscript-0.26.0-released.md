+++
date = "2017-02-25T16:46:24-06:00"
title = "elixirscript 0.26.0 released"
draft = true
+++

For a full list of changes, check out out the [changelog](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md).

This version of Elixirscript has a lot of major changes. Here are some of the changes in the new release:

## Bundled output

This release is the first release to bundle all modules into one JavaScript file. 
The output will now include only the following:

  * `Elixir.Bootstrap.js` - The Elixirscript bootstrapping JavaScript
  * `Elixir.App.js`. - The bundled modules

## Removed `@on_js_load`

`@on_js_load` is no more. In order to start you application, you would do the following:

```js
//Note: An ES module example. Update for your module output of choice
import Elixir from "./Elixir.App";

const my_inital_args = [];
Elixir.start(Elixir.MyApp, my_inital_args);
```

This looks for a `start/2` function in the `MyApp` module. It tries to mimick the API of a normal
Elixir Application.

```elixir
  def start(type, args) do
  end
```

## Removed `JS.import`

`JS.import` is also no more. External JavaScript modules now must be defined in configuration.
A new configuration, `js_modules` is where they go.

```elixir
js_modules: [
  {React, "react"},
  {ReactDOM, "react-dom"}
]
```

In the example above, both `React` and `ReactDOM` will be imported at the top of the bundled output.
Each item can be a 2-tuple or a 3-tuple with the third element being a keyword list of options for the
defined module output format.


## elixirscript.exs config file

If you are using Elixirscript outside of a mix project, 
you can still give it configuration using an `elixirscript.exs` file. The contents must be a keyword list
with the exact same options are the elixir_script config defined in mix projects.

```elixir
#example elixirscript.exs file

[ 
  input: ["app/elixirscript"], 
  output: "dist",
  format: :common,
  js_modules: [
    {React, "react"},
    {ReactDOM, "react-dom"}
  ]
]

```

The `elixirscript` CLI will look for an `elixirscript.exs` file in the current directory, 
or you can specify one with the `-c` flag

