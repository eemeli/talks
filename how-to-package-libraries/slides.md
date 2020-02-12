<small></small>

## How to Package Front-end JavaScript Libraries in 2020

<em><br>Eemeli Aro<br><code>npx eemeli</code></em>

---

<div style="position:relative">
<img src="vincit-at-work.jpg" style="border:none; box-shadow:none; display:block; margin: auto">
</div>

---

<br><br>

What happens to your code when you build it?

---

<br><br>

But isn't CommonJS good enough for everyone?

<small>Hint: not anymore.</small>

---

<br><br>

The browser is not the environment where your libraries are parsed.

<small>Think instead of Webpack, Rollup, and maybe Node.js <sup>\*</sup></small>

---

<br><br>

Tree shaking. It's a thing. An ES module thing.

<small>Unless you `import * as Foo from 'foo'`, so don't do that.</small>

---

<br><br>

But won't that break in Node?

<small>Technically no, but kinda yes, but we can fix that.</small>

---

<br><br>

If we need to care about Node, we can <span style="color:lime">hack it</span>.

---

<br><br>

Maybe `package.json` fields could help?

```js
{
  "type": "module",
  "main": "...",
  "module": "...",
  "browser": { ... }
}
```

---

<div style="position:relative">
<img src="grumpy-cat-no.jpg" style="border:none; box-shadow:none; display:block; margin: auto">
</div>

---

<div style="position:relative">
<img src="mjs.jpg" style="border:none; box-shadow:none; display:block; margin: auto; height: 531px">
</div>

---

<br><br>

Use the extension, Luke!

```js
{
  "main": "index"
}
```

```shell
$ ls index*
index.mjs
index.js
```

---

<br><br>

There, I fixed it.

<small>Of course there's a catch.</small>

---

<br>

* TypeScript **requires** a `main` field.<br><br>
* Jest implements its own `require()`, and needs a little configuration<br><br>
* Older Webpacks don't default to supporting `.mjs`

---

<small>Too long; didn't listen</small>

* If you can, you should only publish your code as ES modules.<br><br>
* If you need to, use/abuse the priority of `.mjs` over `.js`.<br><br>
* Ask me again about this later. Things <span style="text-decoration:line-through">_might_</span> will change.

---

<small></small>

## How to Package Front-end JavaScript Libraries in <span style="text-decoration:line-through">_2019_</span> 2020

<em><br>@eemeli_aro<br></em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">
