<small></small>

## The State of the Art in Localization

<em><br>
@eemeli_aro<br>
github.com/eemeli</em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">

---

<small></small>

<img src="cancel.png" style="width:50%" alt="https://help.peatix.com/customer/en/portal/articles/362169-how-to-respond-to-ticket-cancellation-requests">

---

<small></small>

### Toby’s photos

---

<small></small>

### Toby took 2566 photos

---

<small></small>

### Toby took 2,566 photos

---

<small></small>

### Toby took 2,566 photos
### on 10 July 2019

---

<small>Toby took <span style="color:white">2,566</span> photos on 10 July 2019</small>

### Intl.NumberFormat
```js
const nf = new Intl.NumberFormat('en')

nf.format(2566)
// '2,566'
```

---

<small>Toby took 2,566 photos on <span style="color:white">10 July 2019</span></small>

### Intl.DateTimeFormat
```js
const date = new Date(2019, 6, 10) // months are fun.
const opt = { day: 'numeric', month: 'long', year: 'numeric' }
const dtf = new Intl.DateTimeFormat('en-GB', opt)

dtf.format(date)
// '10 July 2019'
```

---

<small>Toby took 2,566 photos on <span style="color:white">July 10, 2019</span></small>

### Intl.DateTimeFormat
```js
const date = new Date(2019, 6, 10) // months are fun.
const opt = { day: 'numeric', month: 'long', year: 'numeric' }
const dtf = new Intl.DateTimeFormat('en-US', opt)

dtf.format(date)
// 'July 10, 2019'
```

---

<small>Toby took 2,566 photos <span style="color:white">2 days ago</span></small>

### Intl.RelativeTimeFormat
<small>Chrome 71 (2018 Dec), Firefox 65 (2019 Jan), Node 12 (2019 April)</small>

```js
const rtf = new Intl.RelativeTimeFormat('en')

rtf.format(-2, 'day')
// '2 days ago'
```

---

<small></small>

### Intl.ListFormat
<small>Chrome 72 (2019 Jan)</small>

```js
const vehicles = ['Motorcycle', 'Bus', 'Car']
const opt = { style: 'long', type: 'conjunction' }
const lf = new Intl.ListFormat('en', opt)

lf.format(vehicles)
// 'Motorcycle, Bus, and Car'
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Intl.MessageFormat?
<small>Not yet. :(</small>

---

<small>Toby took 2,566 <span style="color:white">photos</span> on 10 July 2019</small>

### Intl.PluralRules

```js
const pr = new Intl.PluralRules('en')

pr.select(1)    // 'one' → 'photo'
pr.select(2566) // 'other' → 'photos'
```

---

<small>zero / one / two / few / many / other</small>

### Intl.PluralRules

```js
const pr = new Intl.PluralRules('en', { type: 'ordinal' })

pr.select(1)    // 'one' → '1st'
pr.select(2)    // 'two' → '2nd'
pr.select(3)    // 'few' → '3rd'
pr.select(2566) // 'other' → '2566th'
pr.select(42)   // 'two' → '42nd'
```

---

<small>I maintain `messageformat`. It's great.</small>

### Message Formatting Libraries

- FormatJS _– Yahoo (2014)_
- I18next _– Locize (2012)_
- messageformat _– OpenJS Foundation (2012)_
- Project Fluent _– Mozilla (2017)_

---

<small></small>

### Front-end Environments

- React _– react-intl, react-i18next, @lingui/react, ..._
- Angular _– `<ng i18n>`, ngx-translate, ..._
- Vue _– vue-i18n, vue-i18next, vue-intl_
- Svelte _– svelte-i18n_

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### ICU MessageFormat

```sh
{name}’s photos
```

```
{name} took {numPhotos, plural,
  =0 {no photos}
  one {one photo}
  other {{numPhotos, number} photos}
} on {timestamp, date, long}
```

---

<small>Toby took 2566 photos on 10 July 2019</small>

### I18next Translations

```json
{
  "photos-title": "{{name}}’s photos",
  "photos": "one photo",
  "photos_plural": "{{count}} photos",
  "photos-taken":
    "{{name}} took $t(photos, {'count': {{numPhotos}} }) on {{timestamp}}"
}
```

_Alternatively, use `i18next-icu` for MessageFormat support_

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Project Fluent

```sh
photos-title = {$name}’s photos

photos-taken = {$name} took {$numPhotos ->
   [0] no photos
   [one] one photo
  *[other] {$numPhotos} photos
} on {DATETIME($timestamp,
  day: "numeric",
  month: "long",
  year: "numeric"
)}
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Project Fluent

```sh
photos-title = {$name}’s photos

long-date = {DATETIME($timestamp,
  day: "numeric",
  month: "long",
  year: "numeric"
)}

photos-taken = {$name} took {$numPhotos ->
   [0] no photos
   [one] one photo
  *[other] {$numPhotos} photos
} on {long-date}
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### ICU MessageFormat

```sh
{name}’s photos
```

```
{name} took {numPhotos, plural,
  =0 {no photos}
  one {one photo}
  other {{numPhotos, number} photos}
} on {timestamp, date, long}
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Messages in XLIFF

```xml
<xliff xmlns="urn:oasis:names:tc:xliff:document:2.0"
  version="2.0" srcLang="en-GB" trgLang="en-US">
  <file id="namespace1">
    <unit id="key1">
      <segment>
        <source>{name}’s photos</source>
        <target>...</target>
      </segment>
    </unit>
    <unit id="key2">
      <segment>
        <source>
          {name} took {numPhotos, plural,
            =0 {no photos}
            one {one photo}
            other {{numPhotos, number} photos}
          } on {timestamp, date, long}
        </source>
        <target>
          ...
        </target>
      </segment>
    </unit>
  </file>
</xliff>
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Messages in JSON

```json
{
  "photos-title": "{name}’s photos",
  "photos-taken":
    "{name} took {numPhotos, plural, =0 {no photos} one {one photo} other {{numPhotos, number} photos}} on {timestamp, date, long}"
}
```

---

<small>Toby took 2,566 photos on 10 July 2019</small>

### Messages in YAML

```yaml
photos-title: "{name}’s photos"
photos-taken: |
  {name} took {numPhotos, plural,
    =0 {no photos}
    one {one photo}
    other {{numPhotos, number} photos}
  } on {timestamp, date, long}
```

---

<small></small>

### Translation Services

- locize.com
- lokalise.co
- phraseapp.com
- poeditor.com
- transifex.com
- weblate.org
- ...

---

<div style="position:relative; background:white">
<img src="all-the-things.png" style="border:none; box-shadow:none; display:block; margin-left: 120px; padding: 30px; width:750px">
<div style="background:white;color:black;font-family:'Comic Sans MS';font-size:60px;position:absolute;top:30px;left:70px">TRANSPILE</div>
</div>

---

<small>webpack.config.js</small>

```sh
npm i -D messageformat messageformat-loader
```

```js
module.exports = {
  ...,
  module: {
    rules: [
      {
        test: /\bmessages\.(json|ya?ml)$/,
        type: 'javascript/auto', // required to load JSON as JS
        loader: 'messageformat-loader',
        options: { locale: ['en-GB'] }
      }
    ]
  }
}
```

---

<small><span style="color:white">Toby took 2,566 photos on 10 July 2019</span></small>

```js
import messages from './messages.yaml'

const msg = messages['photos-taken']
const timestamp = new Date(2019, 6, 10)

msg({ name: 'Toby', numPhotos: 2566, timestamp })
// 'Toby took 2,566 photos on 10 July 2019'
```



<small></small>

```js
function(e, n) {
  var t = function(e, n) {
    var t = String(e).split("."),
        r = !t[1],
        o = Number(t[0]) == e,
        i = o && t[0].slice(-1),
        u = o && t[0].slice(-2);
    return n
      ? 1 == i && 11 != u ? "one"
        : 2 == i && 12 != u ? "two"
        : 3 == i && 13 != u ? "few"
        : "other"
      : 1 == e && r ? "one" : "other"
  },
  r = function(e, n, t) {
    var r = t && t.split(":") || [],
        o = {
          integer: { maximumFractionDigits: 0 },
          percent: { style: "percent" },
          currency: {
            style: "currency",
            currency: r[1] && r[1].trim() || "USD",
            minimumFractionDigits: 2,
            maximumFractionDigits: 2
          }
        };
    return new Intl.NumberFormat(n, o[r[0]] || {}).format(e)
  },
  o = function(e, n, t) {
    var r = { day: "numeric", month: "short", year: "numeric" };
    switch (t) {
      case "full": r.weekday = "long";
      case "long": r.month = "long"; break;
      case "short": r.month = "numeric"
    }
    return new Date(e).toLocaleDateString(n, r)
  };
  e.exports = {
    "photos-title": function(e) {
      return e.name + "’s photos"
    },
    "photos-taken": function(e) {
      return e.name +
        " took " +
        function(e, n, t, r, o) {
          if ({}.hasOwnProperty.call(r, e)) return r[e];
          n && (e -= n);
          var i = t(e, o);
          return i in r ? r[i] : r.other
        }(e.numPhotos, 0, t, {
          0: "no photos",
          one: "one photo",
          other: r(e.numPhotos, "en") + " photos"
        }) +
        " on " +
        o(e.timestamp, "en", " long".trim())
      }
  }
}
```

---

<small><span style="color:white">Toby took 2,566 photos on 10 July 2019</span></small>

```sh
npm i react-message-context
```

```js
import React from 'react'
import { Message, MessageProvider } from 'react-message-context'
import messages from './messages.yaml'

const Toby = ({ numPhotos, timestamp }) =>
  <Message id="photos-taken" name="Toby"
    numPhotos={numPhotos} timestamp={timestamp} />

export const TobyApp = () =>
  <MessageProvider messages={messages}>
    <Toby numPhotos={2566} timestamp={new Date(2019, 6, 10)} />
  </MessageProvider>

// 'Toby took 2,566 photos on 10 July 2019'
```

---

<small>To start, localization doesn’t have to make sense.</small>

```js
export function msg(strings, ...values) {
  if (typeof strings === 'string') return strings
  let res = ''
  for (let i = 0; i < values.length; ++i) {
    res += strings[i] + values[i]
  }
  return res + strings[strings.length - 1]
}

const name = 'Toby'
msg(name + '’s photos') // 'Toby’s photos'
msg`${name}’s photos`   // 'Toby’s photos'
```



<small>It can be just as dumb in React.</small>

```js
import React from 'react'

export const Msg = ({ children }) => children

function TobyTitle() {
  const name = 'Toby'
  return <Msg>{name}’s photos</Msg>
}
```

---

<small></small>

## Kotoistamisen huippuluokka

<em><br>
@eemeli_aro<br>
github.com/eemeli</em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">
