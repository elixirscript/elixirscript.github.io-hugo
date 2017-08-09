+++
title = "ElixirScript 0.30.0 Released"
date = 2017-08-09T00:29:03-05:00
draft = true
+++

ElixirScript 0.30 is a large release with lots of changes. It includes a rewrite of the compiler, a change in JavaScript interoperability and much more. These changes make writing ElixirScript projects feel like writing a normal Elixir project. These changes make this the best release of ElixirScript so far! This is a great point to give ElixirScript a try for yourself. This post will describe the major changes in this version.

## Erlang 20 and Elixir 1.5 (compiled with Erlang 20) required

This release requires the above because it reads debug information from beam files. This is the same information used for `Exception.blame/3` in Elixir 1.5. This means ElixirScript can compile any Elixir module. One benefit is ElixirScript can now compile the actual Elixir standard library. In earlier versions, it had to reimplement the standard library. Sections below describe the other benefits of this.

## Can now use normal Elixir dependencies

With the above, ElixirScript can also compile your project's dependencies to JavaScript.

## ElixirScript-specific packages are now possible

This is one of the most exciting changes. It is now possible to create packages that ElixirScript code can use. This is thanks to the changes described above and the new FFI described later. These are normal mix projects which may or may not have FFI modules and some JavaScript files.

If creating a Hex package for ElixirScript, please prepend `elixir_script` to your package name. This is so that it is clear your package is to be use with ElixirScript. For example, a React package would be named `elixir_script_react`. [Here is an example](https://github.com/elixirscript/elixirscript_react).

## Foreign Function Interface (FFI)

JavaScript interoperability has changed in this version. ElixirScript now interacts with JavaScript using an FFI. This allows for a stricter barrier between Elixir code and JavaScript. It also allows for the ability to make packages. Another benefit is that there are far fewer warnings during compilation time. As an example let's create an FFI module for some HTTP specific functions we want to use from JavaScript. An FFI module has 2 parts: the Elixir module and the corresponding JavaScript module.

Our Elixir module, `MyApp.Http`
```elixir
defmodule MyApp.Http do
  use ElixirScript.FFI

  defexternal get(url)
  defexternal post(url, body)
end
```

Our JavaScript module
```javascript
function get(url) {
 // Implementation goes here
}
function post(url, body) {
 // Implementation goes here
}

export default {
 get,
 post
}
```

ElixirScript depends on the JavaScript module being at a certain path. It will look for the JavaScript module either at `priv/elixir_script/my_app/http.js` or `priv/elixir_script/my_app.http.js`. If your code uses the FFI module, during compilation ElixirScript copies the js file along side the compilation output. The generated output imports the js file.

With the above in place, you can call functions on `MyApp.Http` as you would any other Elixir module.

For more information, check the documentation for `ElixirScript.FFI`.

## Limitations

`receive` is still not supported. When ElixirScript encounters a `receive` call during compilation, the compiler will show a warning, but will continue. During runtime these calls will throw an error. With `receive` not yet supported, this also means most of OTP is not supported as well.

## A Call to Arms

Calls to Erlang functions that the Elixir standard library make have to be reimplemented in JavaScript. There are many that are implemented, but there are more that need to be. This effects the availability of some Elixir standard library calls. A major contribution to ElixirScript would be to help with an implementation of an Erlang function that is blocking the use of an Elixir function you want to use!

## Breaking Changes

ElixirScript now only works as a mix compiler. This means there will no longer be an escript for download and plugins for Brunch and Webpack will no longer work with this and future versions as well.

ElixirScript now only supports output of ES modules. This is mainly so that there is no confusion about how to implement the JavaScript portion of FFI modules.

## Conclusion

For more information regarding changes, please check the [changelog](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md). For more information about how to get started, the FFI, and other things, check out the [docs](https://hexdocs.pm/elixir_script)

Finally, I want to give a great big thanks to the Elixir Core Team! Thanks for making the updates in Erlang and Elixir that allowed ElixirScript's progress to continue. And thank you for being available to answer questions and to give great advice!
