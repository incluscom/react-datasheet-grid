# react-datasheet-grid

## Changes from main repo

### 1. Custom copy/paste handler support

The `<DataSheetGrid>` component now accepts optional `pasteHandler` and `copyHandler` props. These allow you to define custom logic for handling paste and copy events, respectively. If these props are not provided, the component will fall back to its default behavior. Both handlers should be memoized using e.g. `useCallback`.
- `pasteHandler`: A memoized function that takes a string (the pasted content) and returns a 2D array of strings representing the data to be pasted into the grid.
- `copyHandler`: A memoized function that takes a 2D array of strings (the data to be copied) writes it to the clipboard.

The handlers only deal with raw text data, and ignore any HTML versions in the clipboard. Setting the handlers will disable HTML support for copy/paste.

With the custom handlers, you can use, e.g., the reliable SheetClip library to handle clipboard operations. Example code:

```jsx
const handlePaste = useCallback((text) => {
  const sheetclip = new SheetClip();
  return sheetclip.parse(text);
}, []);

const handleCopy = useCallback((cellData) => {
  const sheetclip = new SheetClip();
  const output = sheetclip.stringify(cellData);
  if (navigator?.clipboard?.writeText) {
    navigator.clipboard.writeText(output);
  }
}, []);

return (
  <DataSheetGrid
    pasteHandler={handlePaste}
    copyHandler={handleCopy}
  />
);
```

## Versioning in this fork

We'll follow the same version number as the main number with `inclus.x` added for each change we make.

## Making a new release

We follow this process to make a new release:
1. Create a patch, and increase the version number in `package.json`. Make a PR to the `master` branch.
2. Merge the PR, and fork a new `release/<version>` branch from `master`.
3. In the `release/<version>` branch, run `npm run build` to create the production build.
4. Commit the `dist` folder to the `release/<version>` branch.
5. Push the `release/<version>` branch to GitHub, and make a release from it.

---


![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/nick-keller/react-datasheet-grid/tests.yml?branch=master)
[![Coveralls](https://img.shields.io/coveralls/github/nick-keller/react-datasheet-grid)](https://coveralls.io/github/nick-keller/react-datasheet-grid)
[![npm](https://img.shields.io/npm/dm/react-datasheet-grid)](https://www.npmjs.com/package/react-datasheet-grid)
[![GitHub last commit](https://img.shields.io/github/last-commit/nick-keller/react-datasheet-grid)](https://github.com/nick-keller/react-datasheet-grid)
![npm bundle size](https://img.shields.io/bundlephobia/min/react-datasheet-grid)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

View [demo and documentation](https://react-datasheet-grid.netlify.app/)

An Airtable-like / Excel-like component to create beautiful spreadsheets.

![Preview](./images/preview.png)

Feature rich:
- Dead simple to set up and to use
- Supports copy / pasting to and from Excel, Google-sheet...
- Keyboard navigation and shortcuts fully-supported
- Supports right-clicking and custom context menu
- Supports dragging corner to expand selection
- Easy to extend and implement custom widgets
- Blazing fast, optimized for speed, minimal renders count
- Smooth animations
- Virtualized rows and columns, supports hundreds of thousands of rows
- Extensively customizable, controllable behaviors
- Built with Typescript

## Install

```bash
npm i react-datasheet-grid
```

## Usage

```tsx
import {
  DataSheetGrid,
  checkboxColumn,
  textColumn,
  keyColumn,
} from 'react-datasheet-grid'

// Import the style only once in your app!
import 'react-datasheet-grid/dist/style.css'

const Example = () => {
  const [ data, setData ] = useState([
    { active: true, firstName: 'Elon', lastName: 'Musk' },
    { active: false, firstName: 'Jeff', lastName: 'Bezos' },
  ])

  const columns = [
    {
      ...keyColumn('active', checkboxColumn),
      title: 'Active',
    },
    {
      ...keyColumn('firstName', textColumn),
      title: 'First name',
    },
    {
      ...keyColumn('lastName', textColumn),
      title: 'Last name',
    },
  ]

  return (
    <DataSheetGrid
      value={data}
      onChange={setData}
      columns={columns}
    />
  )
}
```
