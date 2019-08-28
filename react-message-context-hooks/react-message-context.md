<small></small>

## Using React Hooks for Fun and Profit

<em><br>
@eemeli_aro<br>
github.com/eemeli</em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">

---

<small>github.com/eemeli/react-message-context</small>

```js
import {
  getMessage,
  getMessageGetter,
  MessageContext,
  MessageProvider,
  Message,
  useLocales,
  useMessage,
  useMessageGetter
} from 'react-message-context'
```

---

<small>Context, briefly</small>

```js
import React from 'react'

const Context = React.createContext('default')

export const App = () => (
  <Context.Provider value="foo">
    <Example />
  </Context.Provider>
)

const Example = () => (
  <Context.Consumer>
    {value => (value === 'foo' && 'This is foo.')}
  </Context.Consumer>
)
```

---

<small>message-context.js</small>

```js
import React from 'react'

export const defaultValue = {
  locales: [],
  merge: Object.assign,
  messages: {},
  pathSep: '.'
}

export default React.createContext(defaultValue)
```

---

<small>message-provider.js</small>

```js
import React, { useContext, useMemo } from 'react'
import MessageContext, { defaultValue } from './message-context'

export default function MessageProvider({
  children, locale, merge, messages, pathSep
}) {
  const parent = useContext(MessageContext)
  const context = useMemo(
    () => ({
      locales: getLocales(parent, locale),
      merge: merge || parent.merge,
      messages: getMessages(parent, locale, messages),
      pathSep: getPathSep(parent, pathSep)
    }),
    [parent, locale, merge, messages, pathSep]
  )
  return (
    <MessageContext.Provider value={context}>
      {children}
    </MessageContext.Provider>
  )
}

function getLocales({ locales }, locale) {
  const fallback = locales.filter(fb => fb !== locale)
  return [locale].concat(fallback)
}

function getMessages({ merge, messages }, locale, lcMessages) {
  const res = Object.assign({}, messages)
  const prev = res[locale]
  res[locale] = prev ? merge({}, prev, lcMessages) : lcMessages
  return res
}

function getPathSep(context, pathSep) {
  return pathSep === null || typeof pathSep === 'string'
    ? pathSep
    : context.pathSep
}
```

---

<small>get-message.js</small>

```js
export function getMessage(context, id, locale) {
  const { locales, messages, pathSep } = context
  if (locale != null)
    locales = Array.isArray(locale) ? locale : [locale]
  const path = getPath(id, pathSep)
  for (const lc of locales) {
    const msg = getIn(messages[lc], path)
    if (msg !== undefined) return msg
  }
  return undefined
}

function getIn(messages, path) {
  if (!messages) return undefined
  let msg = messages
  for (const p of path) {
    msg = msg[p]
    if (msg === undefined) return undefined
  }
  return msg
}

function getPath(id, pathSep) {
  if (!id) return []
  if (Array.isArray(id)) return id
  return pathSep ? id.split(pathSep) : [id]
}
```

---

<small>getMessage() example</small>

```js
import React, { Component } from 'react'
import {
  getMessage, MessageContext, MessageProvider
} from 'react-message-context'

const example = { key: 'Your message here' }
export const App = () => (
  <MessageProvider messages={{ example }}>
    <Example />
  </MessageProvider>
)

class Example extends Component {
  render() {
    const message = getMessage(this.context, 'example.key')
    return <span>{message}</span> // 'Your message here'
  }
}
Example.contextType = MessageContext
```

---

<small>use-message.js</small>

```js
import { useContext } from 'react'
import { getMessage } from './get-message'
import MessageContext from './message-context'

export default function useMessage(id, locale) {
  const context = useContext(MessageContext)
  return getMessage(context, id, locale)
}
```

---

<small>use-locales.js</small>

```js
import { useContext } from 'react'

import MessageContext from './message-context'

export default function useLocales() {
  const { locales } = useContext(MessageContext)
  return locales.slice()
}
```

---

<small>useLocales() & useMessage() example</small>

```js
import React from 'react'
import {
  MessageProvider, useLocales, useMessage
} from 'react-message-context'

const en = { example: { key: 'Your message here' } }
const fi = { example: { key: 'Lisää viestisi tähän' } }

export const App = () => (
  <MessageProvider locale="en" messages={en}>
    <MessageProvider locale="fi" messages={fi}>
      <Example />
    </MessageProvider>
  </MessageProvider>
)

function Example() {
  const locales = useLocales() // ['fi', 'en']
  const lfOpt = { style: 'long', type: 'conjunction' }
  const lf = new Intl.ListFormat(locales, lfOpt)
  const lcMsg = lf.format(locales.map(JSON.stringify))
  const keyMsg = useMessage('example.key')
  return (
    <article>
      <h1>{lcMsg /* '"fi" ja "en"' */}</h1>
      <p>{keyMsg /* 'Lisää viestisi tähän' */}</p>
    </article>
  )
}
```

---

<small>get-message.js, continued</small>

```js
export function getMessageGetter(
  context, rootId, { baseParams, locale } = {}
) {
  const { pathSep } = context
  const pathPrefix = getPath(rootId, pathSep)
  return function message(id, params) {
    const path = pathPrefix.concat(getPath(id, pathSep))
    const msg = getMessage(context, path, locale)
    if (typeof msg !== 'function') return msg
    const msgParams = baseParams
      ? Object.assign({}, baseParams, params)
      : params
    return msg(msgParams)
  }
}
```

---

<small>use-message-getter.js</small>

```js
import { useContext } from 'react'
import { getMessageGetter } from './get-message'
import MessageContext from './message-context'

export default function useMessageGetter(rootId, opt) {
  const context = useContext(MessageContext)
  return getMessageGetter(context, rootId, opt)
}
```

---

<small>useMessageGetter() example</small>

```js
import React from 'react'
import {
  MessageProvider, useMessageGetter
} from 'react-message-context'

const example = {
  funMsg: ({ thing }) => `Your ${thing} here`,
  thing: 'message'
}

export const App = () => (
  <MessageProvider messages={{ example }}>
    <Example />
  </MessageProvider>
)

function Example() {
  const getMsg = useMessageGetter('example')
  const getThing = () => getMsg('thing')
  return getMsg('funMsg', { thing: getThing() })
} // 'Your message here'
```

---

<small>Dynamic loading example</small>

```js
import React, { useEffect, useState } from 'react'
import { MessageProvider } from 'react-message-context'
import { App } from './app'
import en from './messages.en.yaml'

export function AppWrapper() {
  const [locale, setLocale] = useState('en')
  const [messages, setMessages] = useState({ en })

  useEffect(() => {
    if (messages[locale]) return
    import(`./messages.${locale}.yaml`)
      .then(lcMessages => {
        const next = Object.assign({}, messages)
        next[locale] = lcMessages
        setMessages(next)
      })
      .catch(error => {
        console.error(error)
        if (locale !== 'en') setLocale('en')
      })
  }, [locale])

  return (
    <MessageProvider locale="en" messages={en}>
      <MessageProvider
        locale={locale}
        messages={messages[locale]}
      >
        <App setLocale={setLocale} />
      </MessageProvider>
    </MessageProvider>
  )
}
```

---

<small></small>

### npm i react-message-context<br><br>

<em><br>
@eemeli_aro<br>
github.com/eemeli</em>

<img src="vincit.svg" style="background:none; border:none; box-shadow:none; width:250px">
