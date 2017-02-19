+++
date = "2017-02-19T11:28:48-06:00"
title = "Elixirscript 0.25.0 released"
+++

0.25.0 adds some big changes. For a full look at what's new, look at the [changelog.](https://github.com/elixirscript/elixirscript/blob/master/CHANGELOG.md) Below is a summary of the major changes.

## Select module format of output

Beginning with this release, the module format of generated JavaScript is selectable. The choices are:

*   es - ES Modules (default)

*   common - CommonJS modules

*   umd - UMD modules

The common options means you can use the output in node without the need for tools like babel. The same for UMD in modern browsers (if you use [requirejs](http://requirejs.org/) or add in each file in script tags). JS.import is also updated to generate the correct import code for the specified format.

## Dependencies support in mix projects

This release also brings support for dependencies in mix projects.

Your mix project must include the [elixirscript compiler in its list of compilers](https://github.com/elixirscript/elixirscript/blob/master/GettingStarted.md#mix-dependency). Dependencies you use must also have the elixirscript compiler in their mix compilers. When you run "mix compile", the compiler will compile the elixirscript code in the paths defined in those dependencies with your code.

## Default compiler parameters

The elixirscript mix compiler has the following compiler defaults:

*   input: "lib/elixirscript"

*   output: "priv/elixirscript"

*   format: ":es"

These are changeable by adding the "elixir_script" settings to your mix project.

## Other announcements

*   Elixirscript now has an [organization.](https://github.com/elixirscript) It has all elixirscript-related projects.

*   Elixirscript now has a [gitter](https://gitter.im/elixirscript/elixirscript) room. I am still reachable in the "elixirscript" slack channel on the elixir-lang slack. The gitter room makes sure that conversation history isn't lost allows for integrations.

