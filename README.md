# MUI Menu in an iframe scrolls the page around it

Live page: https://cmdcolin.github.io/mui-focus-repro/

- **Bug:** `@mui/material` 9.5.0 focuses a Menu's first row before Popover
  positions the menu, while the row still sits at the top-left of the viewport.
- **Effect:** Firefox scrolls every enclosing page to show a focused element, so
  a page embedding the Menu in an iframe jumps up to the iframe's top whenever
  that top is scrolled out of view. Chrome skips that scroll for an element
  inside a `position: fixed` container and never jumps.
- **Repro:** open the live page in Firefox and press **Run test**. The panel
  reports where MUI focused the row, whether the menu had a position yet, and
  how far the page moved. **With the workaround** runs the same test with
  [JBrowse's fix](https://github.com/GMOD/jbrowse-components/blob/main/packages/core/src/ui/usePlacedMenuFocus.ts).

`index.html` is the embedding page and `frame.html` the iframe, with React 19.2
and `@mui/material` 9.5.0 loaded from esm.sh. Nothing is built.

## Measured

Run test, iframe top 300 px above the window:

| Browser       | Mode           | Row focused at | Menu positioned | Page    |
| ------------- | -------------- | -------------- | --------------- | ------- |
| Firefox 151   | MUI default    | x=11, y=32     | no              | −596 px |
| Firefox 151   | workaround     | x=40, y=549    | yes             | stayed  |
| Chrome 155    | MUI default    | x=11, y=32     | no              | stayed  |
| Chrome 155    | workaround     | x=40, y=548    | yes             | stayed  |
