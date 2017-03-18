+++
date = "2017-03-17T20:05:52-05:00"
title = "elixirscript 0.27.0 released"
+++

For a full list of changes, check out out the [changelog](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md).

Here are the major changes in this release:

## Super

The `super` special form has been implemented and with it, `defoverridable`

## Global JavaScript Interop

Any JavaScript function, property or module in the global namespace can be accessed by using the `JS` module

```elixir
JS.alert("hello")

JS.console.log("hello")

JS.Date.now()
```
