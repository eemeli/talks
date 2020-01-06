<small></small>

## The State of the Art in Localization

<em><code><br>npx eemeli</code></em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">

---

<small></small>

<img src="cancel.png" style="width:50%" alt="https://help.peatix.com/customer/en/portal/articles/362169-how-to-respond-to-ticket-cancellation-requests">

---

<small></small>

### Mark’s photos

---

<small></small>

### Mark took 1246 photos

---

<small></small>

### Mark took 1,246 photos

---

<small></small>

### Mark took 1,246 photos
### on 11 December 2019

---

<small>Mark took <span style="color:white">1,246</span> photos on 11 December 2019</small>

### Intl.NumberFormat
```js
const nf = new Intl.NumberFormat('en')

nf.format(1246)
// '1,246'
```

---

<small>Mark took 1,246 photos on <span style="color:white">11 December 2019</span></small>

### Intl.DateTimeFormat
```js
const date = new Date(2019, 11, 11) // months are fun.
const opt = { day: 'numeric', month: 'long', year: 'numeric' }
const dtf = new Intl.DateTimeFormat('en-GB', opt)

dtf.format(date)
// '11 December 2019'
```

---

<small>Mark took 1,246 photos on <span style="color:white">December 11, 2019</span></small>

### Intl.DateTimeFormat
```js
const date = new Date(2019, 11, 11) // months are fun.
const opt = { day: 'numeric', month: 'long', year: 'numeric' }
const dtf = new Intl.DateTimeFormat('en-US', opt)

dtf.format(date)
// 'December 11, 2019'
```

---

<small>Mark took 1,246 photos <span style="color:white">2 days ago</span></small>

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
<small>Chrome 72 (2019 Jan), Node 12 (2019 April)</small>

```js
const vehicles = ['Motorcycle', 'Bus', 'Car']
const opt = { style: 'long', type: 'conjunction' }
const lf = new Intl.ListFormat('en', opt)

lf.format(vehicles)
// 'Motorcycle, Bus, and Car'
```

---

<small>Mark took 1,246 photos on 11 December 2019</small>

### Intl.MessageFormat?
<small>Not yet. :(</small>

---

<small>Mark took 1,246 <span style="color:white">photos</span> on 11 December 2019</small>

### Intl.PluralRules

```js
const pr = new Intl.PluralRules('en')

pr.select(1)    // 'one' → 'photo'
pr.select(1246) // 'other' → 'photos'
```

---

<small>zero / one / two / few / many / other</small>

### Intl.PluralRules

```js
const pr = new Intl.PluralRules('en', { type: 'ordinal' })

pr.select(1)    // 'one' → '1st'
pr.select(2)    // 'two' → '2nd'
pr.select(3)    // 'few' → '3rd'
pr.select(1246) // 'other' → '1246th'
pr.select(42)   // 'two' → '42nd'
```

---

<small>I maintain `messageformat`. It's great.</small>

### Message Formatting Libraries

- FormatJS _– Yahoo (2014)_
- Globalize _– OpenJS Foundation (2010)_
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

<small>Mark took 1,246 photos on 11 December 2019</small>

### ICU MessageFormat

```sh
{name}’s photos
```

```js
{name} took {numPhotos, plural,
  =0 {no photos}
  one {one photo}
  other {# photos}
} on {timestamp, date, long}
```

---

<small>Mark took 1246 photos on 11 December 2019</small>

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



<small>Mark took 1246 photos on 11 December 2019</small>

### Polyglot.js Translations

```json
{
  "photos-title": "%{name}’s photos",
  "photos": "one photo |||| %{numPhotos} photos",
  "photos-taken":
    "%{name} took %{photos_as_string} on %{timestamp_as_string}"
}
```

---

<small>Mark took 1,246 photos on 11 December 2019</small>

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

<small>Mark took 1,246 photos on 11 December 2019</small>

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

<small>Mark took 1,246 photos on 11 December 2019</small>

### ICU MessageFormat

```sh
{name}’s photos
```

```
{name} took {numPhotos, plural,
  =0 {no photos}
  one {one photo}
  other {# photos}
} on {timestamp, date, long}
```

---

<small>Mark took 1,246 photos on 11 December 2019</small>

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
            other {# photos}
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



<small>Mark took 1,246 photos on 11 December 2019</small>

### Messages in .properties

```js
photos-title = {name}’s photos
photos-taken = \
  {name} took {numPhotos, plural, \
    =0 {no photos} \
    one {one photo} \
    other {# photos} \
  } on {timestamp, date, long}
```

---

<small>Mark took 1,246 photos on 11 December 2019</small>

### Messages in JSON

```json
{
  "photos-title": "{name}’s photos",
  "photos-taken":
    "{name} took {numPhotos, plural, =0 {no photos} one {one photo} other {{numPhotos, number} photos}} on {timestamp, date, long}"
}
```

---

<small>Mark took 1,246 photos on 11 December 2019</small>

### Messages in YAML

```yaml
photos-title: '{name}’s photos'
photos-taken: |
  {name} took {numPhotos, plural,
    =0 {no photos}
    one {one photo}
    other {{numPhotos, number} photos}
  } on {timestamp, date, long}
```

---

<div style="position:relative; background:white">
<img src="all-the-things.png" style="border:none; box-shadow:none; display:block; margin-left: 120px; padding: 30px; width:750px">
<div style="background:white;color:black;font-family:'Comic Sans MS';font-size:60px;position:absolute;top:30px;left:70px">TRANSPILE</div>
</div>

---

<small>webpack.config.js</small>

```sh
npm i -D messageformat@next messageformat-loader@next
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

<small><span style="color:white">Mark took 1,246 photos on 11 December 2019</span></small>

```js
import messages from './messages.yaml'

const msg = messages['photos-taken']
const timestamp = new Date(2019, 11, 11)

msg({ name: 'Mark', numPhotos: 1246, timestamp })
// 'Mark took 1,246 photos on 11 December 2019'
```

---

<small><span style="color:white">Mark took 1,246 photos on 11 December 2019</span></small>

```sh
npm i react-message-context
```

```js
import React from 'react'
import { MessageProvider, useMessage } from 'react-message-context'
import messages from './messages.yaml'

function Mark({ numPhotos, timestamp }) {
  const msg = useMessage('photos-taken')
  return msg({ name: 'Mark', numPhotos, timestamp })
}

export const MarkApp = () =>
  <MessageProvider messages={messages}>
    <Mark numPhotos={1246} timestamp={new Date(2019, 11, 11)} />
  </MessageProvider>

// 'Mark took 1,246 photos on 11 December 2019'
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

const name = 'Mark'
msg(name + '’s photos') // 'Mark’s photos'
msg`${name}’s photos`   // 'Mark’s photos'
```



<small>It can be just as dumb in React.</small>

```js
import React from 'react'

export const Msg = ({ children }) => children

function MarkTitle() {
  const name = 'Mark'
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
