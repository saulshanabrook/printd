# Printd [![npm](https://img.shields.io/npm/v/printd.svg)](https://www.npmjs.com/package/printd) [![npm](https://img.shields.io/npm/dt/printd.svg)](https://www.npmjs.com/package/printd) [![Build Status](https://travis-ci.org/joseluisq/printd.svg?branch=master)](https://travis-ci.org/joseluisq/printd) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

> [Print](https://developer.mozilla.org/en-US/docs/Web/API/Window/print) HTML elements in modern browsers. :printer:

Printd is a small (`618 bytes` gzipped) script to print HTMLElements. Printd opens [the Print Dialog](https://developer.mozilla.org/en-US/docs/Web/API/Window/print) to print elements inside a blank document. It also supports [CSS Text](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style) for custom styles.

- :rocket: [Live demo](https://codepen.io/joseluisq/full/VzRpGb/)
- :books: [Demo source code](https://github.com/joseluisq/printd-vue-component-example)

## Install

[Yarn](https://github.com/yarnpkg/)

```sh
yarn add printd
```

[NPM](https://www.npmjs.com/)

```sh
npm install printd --save
```

[UMD](https://github.com/umdjs/umd/) file is also available on [unpkg](https://unpkg.com):

```html
<script src="https://unpkg.com/printd/dist/printd.umd.min.js"></script>
```

You can use the library via `window.printd`.

## Usage

### ES6

```js
import { Printd } from 'printd'

// some styles for the element (optional)
const cssText = `
  table {
    font-size: 85%;
    font-family: sans-serif;
    border-spacing: 0;
    border-collapse: collapse;
  }
`

const d = new Printd()

// opens the "print dialog" of your browser to print the element
d.print( document.getElementById('mytable'), cssText )
```

### Typescript

```ts
import { Printd } from 'printd'

const cssText: string = `
  h1 {
    color: black;
    font-family: sans-serif;
  }
`

const d: Printd = new Printd()
d.print( document.getElementById('myelement'), cssText )
```

## API

### Methods

#### constructor

The constructor supports an optional parent element (`HTMLElement`) where the printable element will be appended. Default value is `window.document.body`.

Example:

```js
const d = new Printd( document.getElementById('myparent') )
```

#### print
Function to print the current `HTMLElement`.

Params:

- __el:__ The `HTMLElement` to print.
- __cssText:__ Optional [CSS Text](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style) to add custom styles to the current element.
- __callback:__ Optional callback function. _Inside the callback it's necessary to call `launchPrint(win)` to trigger the printing._
  - __win__: `Window` reference.
  - __doc__: `Document` reference.
  - __node__: `HTMLElement` reference.
  - __launchPrint(win)__: `Function` to trigger the printing on demand.

1. Basic example:

```js
const d = new Printd()
d.print( document.getElementById('h1'), `h1 { font-family: serif; }` )
```

2. Callback example:

```js
const d = new Printd()
const cssText = `
  .code {
    font-family: monospace;
  }
`

const callback = (win, doc, node, launchPrint) => {
  // trigger the printing
  launchPrint(win)
}

d.print(document.getElementById('mycode'), cssText, callback)
```

#### getIFrame

Returns the current `HTMLIFrameElement` reference.

### Events

```ts
const { contentWindow } = d.getIFrame()

contentWindow.addEventListener('beforeprint', () => console.log('before print!'))
contentWindow.addEventListener('afterprint', () => console.log('after print!'))
```

__Browser compatibility:__

- Chrome Desktop 63+
- Chrome for Android 63+
- Firefox 6+
- Edge
- Internet Explorer
- Opera Desktop 50+
- Opera for Android 50+

References:
- [Chrome Platform Status - beforeprint and afterprint events](https://www.chromestatus.com/features/5700595042222080)
- https://caniuse.com/#feat=beforeafterprint
- [PR: Update support for before/after print event handlers (Blink)](https://github.com/Fyrd/caniuse/pull/4086)
- https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeprint

__Webkit-based and old browsers__

For Webkit-based browsers, it can create an equivalent result using `window.matchMedia('print')`.

```ts
if (contentWindow.matchMedia) {
  const mediaQueryList = contentWindow.matchMedia('print')

  mediaQueryList.addListener((mql) => {
    if (mql.matches) {
      console.log('before print!')
    } else {
      console.log('after print!')
    }
  })
}
```

References:
- https://developer.mozilla.org/en-US/docs/Web/API/WindowEventHandlers/onbeforeprint
- https://www.tjvantoll.com/2012/06/15/detecting-print-requests-with-javascript/

## Contributions

Feel free to send some [Pull request](https://github.com/joseluisq/printd/pulls) or [issue](https://github.com/joseluisq/printd/issues).

## License
MIT license

© 2018 [José Luis Quintana](http://git.io/joseluisq)
