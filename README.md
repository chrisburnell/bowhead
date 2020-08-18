# Bowhead

## A small framework on which to build your design tokens in SCSS.

I wrote more about this here: [https://chrisburnell.com/bowhead](https://chrisburnell.com/bowhead/).

You’ll want to jump into `src/_config.scss` and make some changes: add your own tokens, etc.

```scss
body {
    @include v(background-color, desert);
    @include v(color, brick);
    border: v(measure, tiny) solid v(color, plankton);
    padding: v(measure, medium) v(measure, gigantic);
    @include v(z-index, above);
}
```

```css
body {
  background-color: tan;
  background-color: var(--color-desert);
  color: firebrick;
  color: var(--color-brick);
  border: var(--measure-tiny) solid var(--color-plankton);
  padding: var(--measure-medium) var(--measure-gigantic);
  z-index: 2;
  z-index: var(--z-index-above);
}
```
