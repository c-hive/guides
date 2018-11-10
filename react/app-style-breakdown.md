# App style

todo: checkout old repo state and check lagging

General sources

- [Comparison os styling variants](https://github.com/styled-components/comparison)
- ["Why We Use Styled Components at Decisiv"](https://medium.com/@_alanbsmith/why-we-use-styled-components-at-decisiv-a8ac6e1507ac)
- [troch/choosing-a-css-in-js-library](https://gist.github.com/troch/c27c6a8cc47b76755d848c6d1204fdaf)
- [CSS in JS comparison with benchmarks, articles, other resources](https://github.com/tuchk4/awesome-css-in-js)

## Inline styling

#### Pros

State changes are UI changes. Think about classes like `is-complete`, `is-open`, `is-selected`. Moving state-related styling allows to unit test it and makes app functionality independent from css. This removes the state from CSS.

Extended CSS functionality (e.g. SCSS, SASS) equivalents in js:
- variables: js variables
- pseudo-classes: js logic, e.g. `:nth-child(even)` vs `i % 2`
- pseudo-elements: regular elements, e.g. `::before` vs `<div style={before} />`
- harder stuff, e.g. hover or media queries

Sources:
- [Michael Chan - Inline Styles: themes, media queries, contexts, & when it's best to use CSS](https://www.youtube.com/watch?v=ERB1TJBn32c)

#### Cons

- File size
- Lack of caching

## CSS Modules

Obvious choice over simple stylesheets because of local scoping.

## Styled-components

## Goals

Looking for:
- foolproof
  - don't want styles to break our app functionality (great talk on this: [Michael Chan - Inline Styles 9:39-](https://youtu.be/ERB1TJBn32c?t=579))
  - enforced conventions
- all the features of modern css and preprocessors
- separation of concerns (i.e. layout, appearence, behaviour)
- simplicity
- predictability
- framework agnostic
- widely used, battle tested solution
- server side and react native support is a plus

_Not_ looking for:
- Winning the race of miliseconds

Don't want:
- mix business logic with styles
- using conventions we cannot enforce
- big cost of entry


Separating styles:
https://blog.cloudboost.io/separate-your-code-with-styled-components-ec4fd1ee3ef8
https://github.com/styled-components/styled-components/blob/master/docs/tips-and-tricks.md#using-javascript-to-our-advantage

Structures/naming
[BEM](http://getbem.com/introduction/), OOCSS, SMACSS, SUITCSS, Atomic, [SASS-Guidelines](https://sass-guidelin.es/)
- https://tech.decisiv.com/structuring-our-styled-components-part-i-2bf21fa64b28
- https://tech.decisiv.com/structuring-our-styled-components-part-ii-99c336a3748f


Layout

https://css-tricks.com/guides/layout/