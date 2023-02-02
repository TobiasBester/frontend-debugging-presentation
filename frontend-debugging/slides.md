---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# Frontend Debugging

Tobias Bester | EPI-USE Labs | 2023 | https://frontend-debugging-presentation-sovp.vercel.app/

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Agenda

- `console` object
- Chrome DevTools
- Browser Plugins - Vue/React/Angular DevTools

---
layout: image-right
image: https://images.unsplash.com/photo-1518737743670-3f217c4def4a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2680&q=80
---

# Why would we debug browser code? 🤷‍♂️

#### _When we get unexpected results for a test case..._
  - 🗺 Incorrect page layout
  - 🖱 Button not working
  - ❓ Incorrect data

<br />

#### _...we debug to find and fix the cause._
  - 🎨 Inspect CSS and correct styling
  - 🚩 Check whether the click event is triggering the correct action
  - 📶 Start at network response and trace it back to view

<br />

<div style="font-size: 8px">
Photo by <a href="https://unsplash.com/@shocking57?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Stephen Hocking</a> on <a href="https://unsplash.com/photos/iDKN0kJHyts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
</div>
  
---

# The tools for the job 🔨

- #### Event handling issues - JS - Logging and stepping through code
- #### Visual inconsistencies and errors - HTML and CSS - DOM and CSS inspector
- #### Incorrect data - JS and API - Network and state inspector

<br />

<div v-click="1">

#### Find all these tools in one place...

</div>

<br />

<div v-click="2">

### ✨Your Favorite Browser! [F12]✨

<img src="/dog-piano.png" style="max-width: 30%;" alt="Dog playing piano" />

</div>

---

# Logging 🌳

- #### Easy
- #### Doesn't affect UI
- #### Inspect JS state
- #### Works on server and client side

---

# `console` object

- #### Prints to stdout or stderr
- #### [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/console)
- #### View logs in Dev Tools Console tab
- #### REPL

<br />

<img src="/console-dev-tools.png" alt="Dev Tools Console" style="max-width: 80%" />

---

# `console` methods

- #### `.log()`
- #### Accepts array of strings - concatenates with spaces

<br />

<div v-click="1">

```javascript
const myData = { answer: 'A' }
// Concatenation gets messy
console.log(`Answer at ${Date.now()} is ${myData.answer}`)

// Auto concatenation + variables aren't called with .toString()
console.log('Answer at', Date.now(), 'is', myData.answer)
```

</div>

---

# `console` methods

- #### `.info()`, `.warn()`, `.error()`, `.debug()`
- #### Same as `.log` but filterable
- #### Convey different meanings

<div v-click="1">

- #### `.warn()` and `.error()`
  - Print to stderr
  - Print stack trace
  - Don't use it for user communication

</div>

---

# `console` methods

- #### `.assert()`
- #### Avoid adding if-statement

<br />

```javascript
const withinLimit = false
console.assert(withinLimit, 'Is not within limit!')

const data = { name: 'Bot', id: null }
console.assert(data.id, 'ID does not exist')
```

---

- 1x Toothpaste
- 1l Milk
- 30 Spaghetti
- 2x Oros

---

# `console` methods

- #### `.count()`
- #### Avoid adding counter variable

<br />

```javascript
const makeCall = () => {
  fetch('http://localhost:3030')
  console.count()
}

makeCall()
console.countReset()
```

---

# `console` methods

- #### `.group()`
- #### Distinguish a set of logs via indentation

<br />

```javascript
console.log('Root level')

console.group('Second level start')
console.log('Second level')
console.log('Second level')

console.group('Third level start')
console.log('Third level')

console.groupEnd('Back to second level')
console.log('Second level')

console.groupEnd('Back to root level')
console.log('Root level')
```

---

# `console` methods

- #### `.table()`
- #### Display complex structures in a table

<br />

```javascript
const people = []
people.push({
  name: 'Bot',
  hobby: 'Soccer',
  address: '123 Washington Lane',
  salary: 150e5
})
people.push({
  name: 'Rob',
  hobby: 'BBall',
  address: '123 Abbey Road',
  salary: 149e4
})
console.table(people);
```

---

# `console` methods

- #### `.time()`
- #### Measure and log time

<br />

```javascript
const complexProcess = (n) => 1 + 1 * n
console.time()
complexProcess(1)
console.timeLog()
complexProcess(2)
console.timeEnd()
```

---

# Stepping through code 👢

- Sources Tab
- View all files sent to client
  - Regular HTML and JS
  - Bundled - sourcemaps

<div v-click="1">

- Add breakpoints and on pause:
  - Resume with F8
  - Step to next line with F10

</div>

<div v-click="2">

- Ctrl+O/Ctrl+P to file search
- Ctrl+Shift+F to global text search

</div>

<div v-click="3">

- `debugger`

</div>

---
layout: image-right
image: /dom.jpg
---

# Fixing visuals 👁‍🗨

- Elements Tab
- Inspect DOM and CSS
- Ctrl+Shift+C to target element
- For an element, you can:
  - Add and edit attributes
  - Duplicate element
  - Copy XPath, Selector
  - Edit element state (hidden, focus, hover)
  - Add a breakpoint

---

# Elements Tab 🎨

- Style Inspector
  - View classes applied and from which file they come from
  - Update styles
  - Add and remove classes from element
  - More element state options

<div v-click="1">

- Layout Inspector
  - Edit margin and padding - see applied styles
  - View and edit flexbox and grid

</div>

<div v-click="2">

- Element properties
- Accessibility

</div>

---

# Inspecting Network Activity 🕵️‍♂️

- Network Tab
- See all requests made by the browser
  - Waterfall: From DNS Lookup to Content Download
- Filter by [Fetch/XHR] for API calls
- Filter requests by name, sort by status, method, size, and time

<div v-click="1">

- For individual requests, you can:
  - View request and response headers
  - View request [Payload] and response [Preview] data
  - Replay request

</div>

<div v-click="2">

- Simulate slow/no internet
- Make requests from simulated device using custom user agent

</div>

---

# Debugging Application State 🧠

- Use combination of debugger and logging, or...

<div v-click="1">

- Use framework-specific extension

</div>

<div v-click="2">

- [Vue DevTools](https://devtools.vuejs.org/guide/installation.html)
  - Components Tab: View Virtual DOM tree, edit data
  - Timeline Tab: View component events to check what data your component emits
  - Routes Tab: Interactive version of `/router/index.ts`
  - Pinia/Vuex Tab: Inspect and edit store data

</div>

---

# Frontend can be frustrating 😡...

<div class="flex justify-between">
  <img src="/mario.png" alt="Mario" style="max-width: 40%" />
  <img src="/others-harm.png" alt="AmPsycho" style="max-width: 40%;" />
</div>

---

# But if you're not using the right tools, there's only one person to blame 😊

<div class="flex justify-center">
  <img src="/thee.png" alt="Thee" style="max-width: 40%;" />
</div>
