+++
date = "2017-03-05T14:27:53-06:00"
title = "using with phoenix"
+++

This guide will walk through setting up a Phoenix project with Elixirscript. This guide assumes you have already created a Phoenix project

**NOTE**: This guide covers Phoenix 1.2. It will be updated when Phoenix 1.3 is released

Update your mix.exs file to add the current version of elixirscript to your dependencies:

```elixir
  defp deps do
    [
      #other deps
     {:elixir_script, "~> x.x.x"}
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
    compilers: [:phoenix, :gettext, :elixir_script] ++ Mix.compilers,
    build_embedded: Mix.env == :prod,
    start_permanent: Mix.env == :prod,
    deps: deps(),
    elixir_script: [
      input: "web/static/elixirscript",
      output: "web/static/js/build"
    ]
  ]
end
```

Elixirscript by default will looks for input in the `lib/elixirscript` directory. It will also by default output to `priv/elixirscript`. Update the input directory to `web/static/elixirscript`. Update the output directory to `web/static/js/build`. This lets it tie into Brunch's pipeline.

Next, update the watcher configuration to use the Elixirscript watcher:

```elixir
  watchers: [node: ["node_modules/brunch/bin/brunch", "watch", "--stdin",
                    cd: Path.expand("../", __DIR__)], mix: ["elixirscript.watch"]]
```

Whenever your Elixirscript code changes, the elixirscript compiler will recompile it.

Create `app.ex` in the `web/static/elixirscript` directory

```bash
touch web/static/elixirscript/app.ex
```

For this example, write a simple module that will write `Hello, world` to the console on start:

```elixir
defmodule App do

  def start(_type, _args) do
    :console.log("Hello, world")
  end

end
```

Finally, update `web/static/js/app.js` to start your Elixirscript app:

```javascript
import Elixir from './build/Elixir.App';
Elixir.start(Elixir.App, [])
```

The empty array is list of initial arguments for your app.


If you run `mix compile`, you should see a JavaScript file, `Elixir.App.js` in your `web/static/js/build` directory. 

If you run `mix phoenix.server`, open your browser, and then open your console, you should see `Hello, world`. Any changes should cause a reload

