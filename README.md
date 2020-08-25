# Bowhead

> Memorable and maintainable design tokens in SCSS

## What?

**Bowhead** is a small SCSS framework on which to implement your design tokens, spitting out CSS Variables with optional fallbacks.

## Why?

Implementing a design system or even just a series of simple design tokens can come with some unpredictable mental overhead. **Bowhead** aims to reduce that mental overhead by abstracting the specifics of design tokens into human-sensible formats and nomenclature.

This has a positive effect that ranges from giving the colours in your design system fun and memorable names, to the time and effort saved when communicating about these colours with collaborators without getting bogged down by details, because let’s be real: you don’t want *or need* to memorise the six-character hex value for all your colours, nor does anyone else! Now imagine that scenario when applied to multiple times more design tokens.

## Installation

- **With npm:** `npm install @chrisburnell/bowhead`
- **Direct download:** [https://github.com/chrisburnell/bowhead/archive/master.zip](https://github.com/chrisburnell/bowhead/archive/master.zip)

## Usage

There are three main moving parts to this set-up and an optional fourth:

1. [`$bowhead-tokens`](#tokens)
2. [`$bowhead-show-fallback`](#fallback)
3. [`$bowhead-generate`](#generate)
4. [`$bowhead-property-map`](#property-map)

### Tokens

`$bowhead-tokens` expects an *SCSS* `map` of "types" of tokens. These types could be a *measure*, *color*, *opacity*, *z-index*, etc.:

```scss
$bowhead-tokens: (
    measure: (
        small:  0.5rem,
        medium:   1rem,
        large:    2rem,
    ),
    color: (
        brick:    #b22222,
        plankton: #3cb371,
        desert:   #d2b48c
    ),
    opacity: (
        alpha: 0.8,
        beta:  0.5,
        gamma: 0.2
    ),
    z-index: (
        below:  -1,
        root:    0,
        default: 1,
        above:   2
    )
);
```

### Fallback

`$bowhead-show-fallback` is either `true` *(default)* or `false` and determines whether or not **Bowhead** should print fallback values for browsers that do not support CSS Variables.

**`$bowhead-show-fallback: true;`**

```css
body {
    color: #b22222;
    color: var(--color-desert);
}
```

**`$bowhead-show-fallback: false;`**

```css
body {
    color: var(--color-desert);
}
```

### Generate

`$bowhead-generate` is either `true` *(default)* or `false` and determines whether or not **Bowhead** should print CSS Variables for you, like so:

```css
:root {
    --measure-small: 0.5rem;
    --measure-medium: 1rem;
    --measure-large: 2rem;
    --color-brick: #b22222;
    --color-plankton: #3cb371;
    --color-desert: #d2b48c;
    --opacity-alpha: 0.8;
    --opacity-beta: 0.5;
    --opacity-gamma: 0.2;
    --z-index-below: -1;
    --z-index-root: 0;
    --z-index-default: 1;
    --z-index-above: 2;
}
```

### Property Map

`$bowhead-property-map` is another `map` that contains mappings from CSS properties (`padding-left`, `border-bottom-right-radius`, etc.) to our defined design token "types" (`measure`, `color`, etc.), i.e.

```scss
(
    width: measure,
    min-width: measure,
    max-width: measure,
    height: measure,
    min-height: measure,
    max-height: measure,
    ...
)
```

If you wish, you can create new mappings or overwrite existing defaults by defining your own property map, e.g.

```scss
$bowhead-property-map: (
    vertical-align: alignments
);
```

Where `alignments` would be one of your design token "types", e.g.

```scss
$bowhead-tokens: (
    alignments: (
        default: baseline,
        alternate: middle
    ),
    ...
);
```

**Bowhead** will merge your defined map into its own defaults automatically!

--------

Then you’ll have to include **Bowhead** in your SCSS somehow. You could use *Webpack* or something like that, or if you’re using *npm*, the below code snippet should suffice.

Take note that you need to define any of your **Bowhead** variables (`$bowhead-tokens`, `$bowhead-show-fallback`, `$bowhead-generate`(, `$bowhead-property-map`)) before importing **Bowhead** into your SCSS!

```scss
$bowhead-tokens: (
    ...
);
$bowhead-show-fallback: true;
$bowhead-generate: true;
$bowhead-property-map: (
    ...
);

@import "node_modules/@chrisburnell/bowhead/bowhead";
```

Finally, you can use either **Bowhead's** `@v` function, `@v` mixin, both, or just the CSS Variables it can spit out. However you use it is totally up to you! :)

```scss
.thing {
    @include v(background-color, desert);
    @include v(color, brick);
    border: v(measure, small) solid v(color, plankton);
    padding: v(measure, medium) v(measure, large);
    @include v(z-index, above);
    opacity: var(--opacity-alpha);
    // 1. if you just want the raw value, this is not really recommended:
    text-decoration-color: map-get(map-get($bowhead-tokens, "color"), "brick");
    // 2. this does the same for you:
    text-decoration-color: v(color, brick, true);
    // 3. so does this, although it includes the CSS Variable too:
    @include v(text-decoration-color, brick, true);
}
```

will generate…

```css
.thing {
    background-color: #d2b48c;
    background-color: var(--color-desert);
    color: #b22222;
    color: var(--color-brick);
    border: var(--measure-small) solid var(--color-plankton);
    padding: var(--measure-medium) var(--measure-large);
    z-index: 2;
    z-index: var(--z-index-above);
    opacity: var(--opacity-alpha);
    /* 1 */
    text-decoration-color: #b22222;
    /* 2 */
    text-decoration-color: #b22222;
    /* 3 */
    text-decoration-color: #b22222;
    text-decoration-color: var(--color-brick);
}
```

## Learn more

I wrote more about **Bowhead** here: [https://chrisburnell.com/bowhead/](https://chrisburnell.com/bowhead/).

## Authors

So far, it’s just myself, [Chris Burnell](https://chrisburnell.com), but I welcome collaborators with ideas to bring to the table!

## License

This project is licensed under an MIT license.
