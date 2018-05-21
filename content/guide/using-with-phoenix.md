+++
date = "2017-03-05T14:27:53-06:00"
title = "using with phoenix"
+++

This guide will walk through setting up a Phoenix project with Elixirscript. This guide assumes you have already created a Phoenix project

**Update: 2018-05-21**: This guide has been updated to cover both ElixirScript 0.32 and Phoenix 1.3

Update your mix.exs file to add the current version of elixirscript to your dependencies:

```elixir
  defp deps do
    [
      #other deps
     {:elixir_script, "~> 0.32"}
    ]
```

Run `mix deps.get`:

```bash
mix deps.get
```

Next, Add the `elixir_script` compiler to your list of mix compilers. Also add in the `elixir_script` configuration:

```elixir
def project do
  [app: :my_app,
    version: "0.0.1",
    elixir: "~> 1.2",
    elixirc_paths: elixirc_paths(Mix.env),
    compilers: [:phoenix, :gettext] ++ Mix.compilers ++ [:elixir_script],
    build_embedded: Mix.env == :prod,
    start_permanent: Mix.env == :prod,
    deps: deps(),
    elixir_script: [
      input: MyApp.App,
      output: "assets/js/build"
    ]
  ]
end
```

Make the `input` the entry point module of your ElixirScript App. Here is will be `MyApp.App` which we will
define later. Next, add the `output` to the configuration and make it `"assets/js/build"`. By default the output
goes to `priv/elixir_script/build`, but we want to place our output somewhere that our asset compilation process can pick it up and bundle it with any other JavaScript.

Create `app.ex` in the `lib/my_app_frontend` directory

```bash
touch lib/my_app_frontend/app.ex
```

For this example, write a simple module that will write `Hello, world` to the console on start:

```elixir
defmodule MyApp.App do

  def start(_type, _args) do
    IO.puts("Hello, world")
  end

end
```

Finally, update `assets/js/app.js` to start your Elixirscript app:

```javascript
import App from './build/Elixir.MyApp.App.js';
App.start(Symbol.for('normal'), []);
```

The empty array is list of initial arguments for your app.


If you run `mix compile`, you should see a JavaScript file, `Elixir.MyApp.App.js` in your `assets/js/build` directory.

If you run `mix phx.server`, open your browser, and then open your console, you should see `Hello, world`. Any changes should cause a recompilation of your ElixirScript code and a reload of the browser

