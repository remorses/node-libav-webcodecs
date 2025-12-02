this library is for implementing webcodecs in node.js using node-av

notice that all browser APIs referenced in this project are ALWAYS polyfills unless they are not built into node.js directly. this project will run in Node.js always.

format and align markdown tables in your message output

you should write your code in zig inside native directory or typescript inside src or test.

for validating something works vitest tests are preferred over temporary scripts

NEVER import from .js files in your typescript files. NEVER write js files. use typescript

the original codebase was for a libav.js based webcodecs polyfill. some codecs are missing because of this: av1 and h264 are missing because of licensing reasons. now we can add those back

## goal of this project

The overall goal of this project is to provide access to advanced media processing and codec functionality—similar to what [libav.js](https://github.com/Yahweasel/libav.js) offers—but directly from Node.js, efficiently and natively. Rather than relying on wrappers like node-av (which may differ from the official C API), or using WASM-based solutions, we aim to implement a native Node.js binding to the Libav libraries using modern tooling.

To achieve this, we will:

- Develop a native extension using Zig, inside the `native` directory, exposing an API that closely mirrors [libav.js](https://github.com/Yahweasel/libav.js), but with bindings created via [Node-API (NAPI)](https://nodejs.org/api/n-api.html) for seamless integration with Node.js.
- Dynamically load Libav libraries, which you can install via Homebrew (`brew`), keeping the build setup (`build.zig`) straightforward.
- Use [napigen](https://github.com/cztomsik/napigen) to generate NAPI bindings efficiently from Zig code (see its README and examples for guidance).
- Refer to the source code of libav.js on GitHub to identify the functions and types that need to be implemented; start with those necessary for the project’s core functionality and maintain a to-do list as you go.
- Write TypeScript or Zig code following the project's conventions, and use [Vitest](https://vitest.dev/) to create unit tests that validate each exposed native function. Prefer isolated, well-defined tests.
- Once the native NAPI bindings are in place, update or replace the existing `@src/node-av-adapter.ts` module with a minimal stub that simply calls the new NAPI functions. TypeScript typing is not a priority at this stage, but manage memory carefully throughout.

By following this approach, the project will provide a modern, efficient, and maintainable interface to Libav functionalities from Node.js, leveraging open source tools and best practices. For detailed code examples, see the repositories:
- [libav.js](https://github.com/Yahweasel/libav.js)
- [napigen](https://github.com/cztomsik/napigen)

## canvas

for canvas we use the library https://github.com/Brooooooklyn/canvas

read the repo https://github.com/Brooooooklyn/canvas/blob/main/README.md to see how to use it

## audio

audio is also impemented with some web audio api APIs

these are added by using https://github.com/ircam-ismm/node-web-audio-api

read its readme and examples to understand how to use it

## changes to node-av-adapter.ts

before doing any changes to node-av-adapter.ts first see the corresponding function implmentation in https://github.com/Yahweasel/libav.js repository. this will help you have a starting point. that codebase use the preferred values for our use cases. You just need to apply modifications assuming NAPI instead of WASM.

IMPORTANT you MUST copy exactly what https://github.com/Yahweasel/libav.js does. If you see utils functions in their implementation for a function you MUST copy also those! You cannot assume anything. a bug in this code will be very costly!

## zig guide lines

- always use c allocator so it is the same as libav
- if there are bugs in the native part try to reproduce them in zig tests so that they can be debugged more easily and have more stack trace info

## reading files on github

do `curl gitchamber.com` to see how to search, list and read files on github repos

ALWAYS run this command to read files on github instead of alternatives

## testing

tests are implemented using vitest. use `vitest ` to run all tests or `vitest pathtofile` to run one or `-t` to run a specific test in a file

running specific test files is preferred so other test files will not pollute your results and output is smaller

## running scripts

to run typescript scripts use `pnpm tsx scriptpath` so that you have support for typescript and esm without extensions

## diffusionstudio

diffusionstudio is a library that let you edit and compose video in the web using webcodecs, canvas and other web APIs.

we are implementing webcodecs and others polyfills in this codebase so that we can use that library in the server and node.js

to know how to use diffusionstudio fetch the github repo files with gitchamber: https://github.com/diffusionstudio/core

this codebase has docs and examples

to debug or modify the diffusionstudio source code, edit `src/diffusionstudio.js` (copied from core.es.js). Types are re-exported from @diffusionstudio/core in `src/diffusionstudio.d.ts`

## adding a new features in src

if you are implementing a feature in src (basically modifying our webcodecs implementation based on libav) you will then need to first read all typescript files in src, so that you actually understand how it works

you will need to do the same when adding code to .zig files
