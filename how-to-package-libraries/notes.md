# How to Package Front-end JavaScript Libraries in 2020

https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md
https://github.com/defunctzombie/package-browser-field-spec
https://github.com/jkrems/proposal-pkg-exports

https://2ality.com/2019/04/nodejs-esm-impl.html
https://nodejs.org/api/esm.html
https://github.com/nodejs/modules/blob/master/doc/plan-for-new-modules-implementation.md

https://github.com/microsoft/TypeScript/issues/21423 - Resolving multiple package.json "main" fields

https://github.com/facebook/jest/issues/2702 - package.module
https://github.com/facebook/jest/issues/4637 - .mjs
https://github.com/facebook/jest/blob/master/e2e/transform/ecmascript-modules-support/package.json

Packaging JavaScript libraries is nominally pretty easy, but it's a bit tricky to do just right. This article is about the latter part, and looking under the hood of what actually goes on once someone starts using your library. In particular, it's about publishing your code as ES modules (using `import` and `export`) rather than CommonJS (using `module.exports` and `require()`).

The really important question here is, which environments will your code need to work in? You're certainly familiar with Node.js and the various browsers that'll run your front-end code, but for the packaging, you should also consider bundlers like Webpack and Rollup, Typescript users, and Jest testers. All of those support ES modules... sort of. And differently.

You might not have realised that browsers already have remarkably [wide support](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Browser_compatibility) for `import`ing JavaScript, and it's supported in the latest Node.js as well, albeit behind an `--experimental-modules` flag. But this doesn't actually matter at all yet for library developers, because all of them require file extensions to be used, and browsers have no analogue for `node_modules` from which to search for packages. The likeliest fix for that will be [import maps](https://wicg.github.io/import-maps/), but that's still being specified and the current draft is only supported by [SystemJS](https://github.com/systemjs/systemjs).

Modern bundlers all support ES modules, either natively or via their plugin systems. Webpack and Rollup in particular are more efficient when handling ES modules rather than CommonJS; other bundlers' internal representations of modules are closer to CommonJS. To be clear, I'm not advocating that you should bundle your own library, but that you should be clear that most of your users who are not directly targeting Node.js will be bundling it as a part of their own application.

A rather relevant question that now arises is, how do these bundlers find libraries' ES code? The key tools here are the `main`, `module` and `browser` fields in the libraries' package.json files, which are used to determine the file path that a bare import of the library's name will resolve to. `main` is the only one of these recgnised by Node.js, and it canonically tends to point to a CommonJS endpoint. `module` is recognised and used by most bundlers as a corresponding ES6 endpoint, but it doesn't actually have any specification to support it. `browser` is [a bit more complicated](https://github.com/defunctzombie/package-browser-field-spec), as it points to code specifically meant for use in browser environments, does not specify a module type, and may map multiple endpoints. As an alternative or addition to these, the `.mjs` file extension is widely supported for JS files that use ES modules.

Besides browsers, bundlers and Node.js, there are two other specific tools that require some consideration: TypeScript and Jest. Even if you don't write TypeScript yourself, many others do, and TypeScript unfortunately requires that each imported package defines a `main` endpoint, even if the application's bundler. Jest also requires special consideration, as it will use its own `require()` implementation while running tests, and that needs a little configuration to handle ES modules in libraries. Other mocking tools such as [mockery]() and [proxyquire](https://github.com/thlorenz/proxyquire)

So, putting all that together,

---

On one level, my personal reasons for writing and publishing libraries are pretty selfish: Once I can package some functionality that my application/service/whatever needs as a distinct module, I find it's easiest to write some API docs for it, and to publish it as open source. At that point, it becomes a black box from the point of view of the rest of the application, and I no longer need to think of it.

## TL;DR

1. Write your code as ES modules, using `.mjs` as the file extension
2. Make sure to leave out `.js` and `.mjs` extensions from all paths in your code & config (including `package.main`!)
3. Publish transpiled ES modules as `.mjs` files, using `babel --keep-file-extension` & `modules: false` in your preset-env config
4. Publish transpiled CommonJS code as `.js`, with `modules: 'auto'` in your preset-env config
5. Profit!

## Why I Think I Know Stuff

One part of the work that I do for a client is to develop tools and guides for their teams of developers to use and follow. In other words, I'm paid hit my head at annoying problems so that they don't have to, and to tell them how to avoid the problems that I've found for myself. This is surprisingly rewarding work, provided that you do manage to find a decent working solution. Sometimes, this means writing new tools to do the job; sometimes, it means patching existing libraries with generic solutions to solve specific problems. Often, it just means that I need to write documentation and suggest specific tools to use.

## Why This Stuff is Important

As always, we live in interesting times. Among other interesting things, the module system of published JavaScript is finally transitioning from CommonJS to ES modules. To be clear, many of us have been using ES modules in the code we write already for many years, but the change that's now happening is that more and more _published_ code is also packaged as ES modules. For front-end development this matters for two reasons:

1. ES modules are smaller, and can be tree-shaken. Not only is there a small per-module size cost associated with the ES-to-CommonJS transpilation, but ES modules can be tree-shaken to leave out unused parts of the code from the final production build. For a real-life examples, take these two apparently equivalent imports of [make-plural](https://www.npmjs.com/package/make-plural), a library that I maintain:

```js
import { en } from "make-plural/plurals";
const { en } = require("make-plural/plurals");
```

Both of these fetch the same plural-category determining function for English. The first `import` will add 520 bytes of gzipped minified size to your bundle, but the second `require()` will add 3284, as Terser isn't able to drop the unused code for all the other languages' functions. Sure, 2.5k by itself might not be that much to care about, but those sizes balloon across your whole codebase.

2. The more code we publish as ES modules, the closer we get to a world where we don't have to _also_ publish it as CommonJS. Paradigm transitions are always painful, and it's in all our interests to get this one over and done with.

## Why Now Is the Time to Do Stuff

Node.js 12 was released in April 2019, and it now supports ES modules via the `--experimental-modules` flag and the `.mjs` file extension. The spec for this has been under development for a number of years, but it's finally getting to be rather stable. Webpack 4 and other packagers have also recently followed suit and implemented support for `.mjs` files.

One way to interpret this is that there's yet another way of telling packagers about a different export, in addition to the various package.json fields that are already supported (in particular `package.browser` and `package.module`, with slightly variable support in different packagers). But trust me, this one's better.

First of all, with the module system being defined by file extension, your package can easily have more than one entry point available from the same path. This isn't possible using `package.module` (which requires a string), and the [key-value form](https://github.com/defunctzombie/package-browser-field-spec#replace-specific-files---advanced) of `package.browser` isn't necessarily supported everywhere. With `.mjs`, you can publish the ES and CommonJS variants next to each other, such that `foo/bar` can either resolve to `foo/bar.mjs` or `foo/bar.js`, depending on what's supported by the runtime environment.

The other benefit is that from `.mjs` files, `require()` is not available. This is actually a good thing
