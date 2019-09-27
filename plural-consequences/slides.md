## A Story of Intended Consequences<br>and Plural Forms

_<br><br>
Eemeli Aro_

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">

---

<small></small>

## _This is_<br>one talk<br>_in a series of_<br>many talks

---

<small>ICU MessageFormat</small>

```text
{unread, plural,
    =0 {You've reached inbox zero!}
    one  {You have one unread email.}
    other  {You have # unread emails.}
}
```

---

<small>Project Fluent</small>

```text
{ $unread ->
    [0] You've reached inbox zero!
    [one] You have one unread email.
   *[other] You have { $unread } unread emails.
}
```

---

<small>Project Fluent</small>

```text
{ NUMBER($score, minimumFractionDigits: 1) ->
    [0.0]   You scored zero points. What happened?
    [one]   This should never happen.
   *[other] You scored {
              NUMBER($score, minimumFractionDigits: 1)
            } points.
}
```

---

<small>Project Fluent</small>

```text
{ NUMBER($score, minimumFractionDigits: 1) ->
    [0.0]   You scored zero points. What happened?
    [one]   This should never happen.
   *[other] You scored {
              NUMBER($score, minimumFractionDigits: 1)
            } points.
}


```

```js
scoreMessage({ score: 0 });
// 'You scored zero points. What happened?'

scoreMessage({ score: 1 });
// 'Your scored 1.0 points.'

scoreMessage({ score: 42 });
// 'Your scored 42.0 points.'
```

---

<small>Pop quiz!</small>

## How many plural forms are there in English?

---

<small></small>

## _Next up is the_<br>third break<br>_of the_<br>first PromptConf

---

<small>ICU MessageFormat</small>

```text
{pos, selectordinal,
   =1 {You finished first!}
   one  {You finished #st}
   two  {You finished #nd}
   few  {You finished #rd}
   other  {You finished #th}
}
```

---

<small>Project Fluent</small>

```text
{ NUMBER($pos, type: "ordinal") ->
   [1] You finished first!
   [one] You finished {$pos}st
   [two] You finished {$pos}nd
   [few] You finished {$pos}rd
  *[other] You finished {$pos}th
}
```

---

<small></small>

```text
NUMBER(42, minimumFractionDigits: 1)
```

```js
const nf = new Intl.NumberFormat('en', {
  minimumFractionDigits: 1
});

nf.format(42); // '42.0'
```

---

<small></small>

```text
NUMBER(42, minimumFractionDigits: 1, type: "ordinal")
```

```js
const nf = new Intl.NumberFormat('en', {
  minimumFractionDigits: 1,
  type: 'ordinal'
});

nf.format(42); // '42.0'
```

---

<small>JavaScript</small>

```js
export class FluentNumber {
  constructor(value, opts) {
    this.value = value;
    this.opts = opts;
  }

  toString() {
    const nf = new Intl.NumberFormat('en', this.opts);
    return nf.format(this.value);
  }
}

export const NUMBER = (arg, opts) => (
  new FluentNumber(Number(arg), opts)
);
```

---

<small>JavaScript</small>

```js
const selector = NUMBER(42);

function match(selector, key) {
  if (key === selector) return true;
  if (selector instanceof FluentNumber) {
    const { value, opts } = selector;
    if (key instanceof FluentNumber) {
      if (key.value === value) return true;
    } else {
      const pr = new Intl.PluralRules('en', opts);
      const category = pr.select(value); // 'other'
      if (key === category) return true;
    }
  }
  return false;
}
```

---

<small>JavaScript</small>

```js
const selector = NUMBER(42, { type: 'ordinal' });

function match(selector, key) {
  if (key === selector) return true;
  if (selector instanceof FluentNumber) {
    const { value, opts } = selector;
    if (key instanceof FluentNumber) {
      if (key.value === value) return true;
    } else {
      const pr = new Intl.PluralRules('en', opts);
      const category = pr.select(value); // 'two'
      if (key === category) return true;
    }
  }
  return false;
}
```

---

<small></small>

### Why did that work?

---

<small></small>

### Why didn't this fail?

```text

NUMBER(42, minimumFractionDigits: 1, type: "ordinal")
```

```js
const nf = new Intl.NumberFormat('en', {
  minimumFractionDigits: 1,
  type: 'ordinal' // ⬅ not a NumberFormat option!
});

nf.format(42); // '42.0'
```

---

<p class="next-left"></p>

A conforming implementation _of the ECMAScript 2020 Internationalization API Specification is permitted to provide additional objects, properties, and functions beyond those described in this specification. In particular, a conforming implementation of the ECMAScript 2020 Internationalization API Specification_ is permitted to provide properties not described in this specification, and values for those properties, for objects that are described in this specification.

<small>ecma-international.org/ecma-402/#conformance</small>

---

### How I Added Ordinal Plural Support to Mozilla's Fluent Project by Pre-emptively Fixing the JS Intl Specification

_eemeli.org/talks/plural-consequences<br><br>
@eemeli_aro<br>
eemeli&#64;gmail.com_

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">
