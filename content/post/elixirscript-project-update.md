+++
date = "2017-07-10T19:55:17-05:00"
title = "elixirscript project update"
+++

A major change is happening to ElixirScript. The compiler has been rewritten to take advantage of features in Erlang 20 and the upcoming Elixir 1.5. The work for this started shortly after my presentation on ElixirScript at Lonestar Elixir. This post will go over the changes that are happening and why.

### The Old Compiler

ElixirScript's old compiler compiled Elixir source code files. It would take these files and turn the code into the Elixir Abstract Syntax Tree (AST). From there it would translate the Elixir AST to JavaScript AST and then to JavaScript code. This worked but there were some drawbacks. For one, the compiler had to have access to the source code. This meant that ElixirScript had to reimplement the Elixir standard library. It also made it harder to compile Elixir libraries to JavaScript. Another issue was that ElixirScript could not support private macros.

### The New Compiler

ElixirScript's new compiler targets Erlang 20 and Elixir 1.5. This is because it uses the debug info inside of beam files compiled with Erlang 20. This is the same info that powers `Exception.blame/3` in Elixir 1.5. The debug info contains an AST that has all macros expanded. Using this AST, the ElixirScript compiler is simpler and half the size of the old compiler. It also made it much easier to contribute to. Reading the debug info from the beam files removes the issues mentioned above. ElixirScript has access to the Elixir standard library and dependencies used in projects. Support for public and private macros comes for free since they are already expanded.

Reimplementing the Elixir standard library is no longer needed. ElixirScript does need an Erlang compatibility layer now. This is because Elixir function calls end up calling Erlang functions. This is implemented in JavaScript. Not all the Erlang standard library needs implementing. Only ones used in the Elixir standard library. There are 2 ways to help here. One is to implement these functions in JavaScript. The other is to contribute to Elixir by replacing as many Erlang function calls with Elixir function calls as possible.

Another thing to note is that this version only works in mix projects. This means there will no longer be an escript version available.

The new compiler tries its best to remove any unused functions and modules from the output. Protocols and their implementations are the exception. There is no way to tell which implementations are removable. ElixirScript replaces standard library modules with its own version in some cases. For instance, it uses its own version of the `String` module. This is because Elixir's version includes the entire unicode database.

### Progress

The version in master is about as usable as the old version. Being able to compile the Elixir standard library is a good test of actual Elixir code. It surfaced some bugs that were fixed and some that still need resolution. I managed to get the [Todo example app](https://github.com/elixirscript/todo-elixirscript) to work with the new version which is a good sign that things are getting closer.

### JavaScript Interoperability

The version in master still has the same JavaScript interop capabilities. But that will likely change. ElixirScript will try to have a stricter barrier between Elixir and JavaScript. The current idea is to add an [FFI layer](https://github.com/elixirscript/elixirscript/issues/317). I've been a fan of how [PureScript does FFI](https://leanpub.com/purescript/read#leanpub-auto-using-javascript-code-from-purescript) and will try to borrow the ideas used there.

### Receive

`receive` is still unsupported. I still want to support it.

Hopefully this post did a good job of explaining the changes happening to the project. These changes make it much easier to see a day when ElixirScript is production ready.