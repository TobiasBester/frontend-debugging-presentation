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

Tobias Bester | EPI-USE Labs | 2025 | frontend-debugging-presentation-sovp.vercel.app/

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

# Why would we debug browser code? 🤔

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

<div v-click="1">

- #### Event handling issues - JS - Logging and stepping through code

</div>

<br />

<div v-click="2">

- #### Visual inconsistencies and errors - HTML and CSS - DOM and CSS inspector

</div>

<br />

<div v-click="3">

- #### Incorrect data - JS and API - Network and state inspector

</div>

<br />

<div v-click="4">

#### Find all these tools in one place...

</div>

<br />

<div v-click="5">

### ✨Your Favorite Browser! [F12]✨

<img src="/browser.png" style="max-width: 30%;" alt="Dog playing piano" />

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
- #### Accepts string substitutions

<br />

<div v-click="1">

```javascript
const myData = { answer: 'A' }
// Concatenation gets messy
console.log(`Answer at ${Date.now()} is ${myData.answer}`)

// Auto concatenation + variables aren't called with .toString()
console.log('Answer at', Date.now(), 'is', myData.answer)

// Substitution-based string can be useful if parameter identifiers are long
console.log('Answer at %d is %s', Date.now(), myData.answer)

// Use %c to apply CSS to text appearing after the symbol
console.log('Answer at %d is %cERROR', Date.now(), 'color: red')

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

<div v-click="2">

```javascript
console.log('Default')
console.info('Info')
console.warn('Warn')
console.debug('Debug')
console.error('Error')
```

</div>

---

# `console` methods

- #### `.assert()`
- #### Avoid adding if-statement
- #### Like a conditional breakpoint

<br />

```javascript
const sumsIncorrectly = 5 + 3 === 10
const sumsCorrectly = 5 + 5 === 10
console.assert(sumsIncorrectly, 'Sum is not correct!')
console.assert(sumsCorrectly, 'Sum is not correct!')

const data = { name: 'Bot', id: null }
console.assert(data.id, 'ID does not exist')
```

---

# `console` methods

- #### `.trace()`
- #### Logs the stack trace from where the method is called

```javascript
const functionA = () => {
  functionB()
}
const functionB = () => {
  functionC()
}
const functionC = () => {
  console.trace('I am inside functionC')
}
const main = () => {
  functionA()
}

main()
```

---

# `console` methods

- #### `.count()`
- #### Avoid adding counter variable
- #### Use label parameter to manage multiple counters

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
const request = async () => await fetch('https://qa.logbox.co.za/lookup/countries')
console.time()
complexProcess(1)
console.timeLog()
await request()
complexProcess(2)
console.timeEnd()   // logs again
```

---

# `console` methods

- #### `.profile()`
- #### Activate browser profiler to measure performance metrics
- #### Use Performance tab to see detailed recording of memory, loading times, and call stack

<br />

```javascript
const complexProcess = (n) => 1 + 1 * n
const request = async () => await fetch('https://qa.logbox.co.za/lookup/countries')
console.profile('complex steps')
complexProcess(1)
await request()
complexProcess(2)
console.profileEnd('complex steps')
```

---

# Progress report

- #### Event handling issues - JS
  - Logging [✅]
  - Stepping through code
- #### Visual inconsistencies and errors - HTML and CSS
  - DOM inspector
  - CSS inspector
- #### Incorrect data - JS and API
  - Network inspector
  - State inspector

---

# Stepping through code 👢

- Sources Tab
- View all files sent to client
  - Regular HTML and JS
  - Bundled - sourcemaps

<div v-click="1">

- Ctrl+O/Ctrl+P to file search
- Ctrl+Shift+F to global text search
</div>

<div v-click="2">

- Add breakpoints and on pause:
  - Resume with F8
  - Step to next line with F10

</div>

<div v-click="3">

- `debugger`

</div>

---

# Progress report

- #### Event handling issues - JS
  - Logging [✅]
  - Stepping through code [✅]
- #### Visual inconsistencies and errors - HTML and CSS
  - DOM inspector
  - CSS inspector
- #### Incorrect data - JS and API
  - Network inspector
  - State inspector

---
layout: image-right
image: /dom.jpg
---

# Fixing visuals 👁‍🗨

- Elements Tab
- Inspect DOM and CSS

<div v-click="1">

- Ctrl+Shift+C to target element
- For an element, you can:
  - Add and edit attributes
  - Duplicate element
  - Copy XPath, Selector
  - Edit element state (hidden, focus, hover)
  - Add a breakpoint

</div>

---

# Elements Tab 🎨

- Style Inspector
  - View classes applied and from which file they come from
  - Update styles
  - Add and remove classes from element
  - More element state options
  - View and edit flexbox and grid

<div v-click="1">

- Other Element tabs
  - Computed properties
  - Flexbox and grid layout properties
  - Event listeners on a DOM object
  - Javascript properties on a DOM object
  - Accessibility: Properties important for screen readers

</div>

---

# Progress report

- #### Event handling issues - JS
  - Logging [✅]
  - Stepping through code [✅]
- #### Visual inconsistencies and errors - HTML and CSS
  - DOM inspector [✅]
  - CSS inspector [✅]
- #### Incorrect data - JS and API
  - Network inspector
  - State inspector

---

# Inspecting Network Activity 🕵️‍♂️

- Network Tab
- See all requests made by the browser
  - Waterfall: From DNS Lookup to Content Download
- Filter by [Fetch/XHR] for API calls
- Filter requests by name, sort by status, method, size, and time

<div v-click="1">

- For individual requests, you can:
  - View request [Payload] and response [Preview] data and [Headers]
  - Replay requests
  - See what initiated a request

</div>

<div v-click="2">

- Add URL-based breakpoint in Sources tab

</div>

<div v-click="3">

- Simulate slow/no internet
- Make requests from simulated device using custom user agent...

</div>

<div v-click="4">

- Or just simulate a device's screen size by using the device toolbar

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

- [React DevTools](https://react.dev/learn/react-developer-tools)

</div>

---

# Final Words

- Don't worry about learning these tools. You won't need them, you perfect coders 🙏
