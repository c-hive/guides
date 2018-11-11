# Styling and CSS

*How should we style our React app?*

This is a huge and controversial topic so we need to be a bit more chatty. It's not obvious that there is (or should be) a one-for-all solution. First we set needs and propose options. Then we break down the evaluation into 3 major topics usually groupped together as "style":
- [**Behaviour**](#behaviour) (SMACSS state)
- [**Component appearence**](#component-appearence) (SMACSS module)
- [**Cross-component appearence**](#cross-component-appearence) (SMACSS base, layout, theme)


## Needs

#### Looking for (in order of importance)
- all the features of modern css and preprocessors (e.g. media queries)
- none of the issues of regular css (e.g. global namespaces)
- clear best practices and conventions
- styles not breaking our functionality
- the more foolproof the better
- active community, good documentation
- simplicity for small projects but scalability for big ones
- low cost of entry
- framework agnostic
- server side and react native support is a plus

#### _Not_ looking for
- Tier 1 performance

#### Don't want
- mixing component logic with styles
- having to use conventions we cannot test or enforce
- big cost of entry


## Options

#### Preprocessed CSS modules

Such as SASS, LESS, SCSS.

Pros
- feature rich
- solves issues of regular CSS
- faster
- widely used and known
- framework agnostic, easily portable

Cons
- technical concerns rest on you
- conventions, best practices not enforced
- requires conventions to be viable for bigger projects, there're many of them with no clear winner (e.g. [BEM](http://getbem.com/introduction/), OOCSS, SMACSS, SUITCSS, Atomic, [SASS-Guidelines](https://sass-guidelin.es/))
- conventions are completely separate from other project conventions (e.g. naming, structure) and have similar complexity, high cost of entry

#### CSS in JS

Library comparisons:
- [Generations / evolution of libs](https://github.com/streamich/freestyler/blob/master/docs/en/generations.md)
- [Comparison of libs by Michele Bertoli](https://github.com/MicheleBertoli/css-in-js)
- [Comparison of libs by Radium](https://github.com/FormidableLabs/radium/tree/905227c122b1775775cf8d82c508cce4179cff08/docs/comparison)
- [troch/choosing-a-css-in-js-library](https://gist.github.com/troch/c27c6a8cc47b76755d848c6d1204fdaf)
- [CSS in JS comparison with benchmarks, articles, other resources](https://github.com/tuchk4/awesome-css-in-js)

Out of the many libs the best choice currently seems to be `styled-components` which is also clearly shown in usage trends. Pros and cons address this in particular but most apply accros many.

Pros
- feature rich (complete SASS features for JS with [polished](https://github.com/styled-components/polished))
- no issues of regular CSS
- best practices and conventions come with the lib
- JS for logic (e.g. `:nth-child(even)` vs `i % 2`)
- critical parts can be unit tested
- technical concerns are abstracted away (e.g. CSS injection, object creation and structure)
- very simple for small projects
- needs structure / conventions for big projects, but those align with project conventions as a whole (great articles on the topic: [Structuring our Styled Components | Part I](https://tech.decisiv.com/structuring-our-styled-components-part-i-2bf21fa64b28), [Structuring our Styled Components | Part II](https://tech.decisiv.com/structuring-our-styled-components-part-ii-99c336a3748f))
- doesn't exclude other ways of styling
- good documentation, active community
- low cost of entry also for those only familiar with CSS
- server side and react native support

Cons
- not framework agnostic (but uses CSS syntax which makes it somewhat portable)
- ability to mix component logic with styles
- security issue with interpolation ([article describing the vulnerability](https://reactarmory.com/answers/how-can-i-use-css-in-js-securely), [GitHub issue, currently open](https://github.com/styled-components/styled-components/issues/1105))


## Decision

Let's address the concerns separately. [There's also a great post about this on StackOverflow](https://stackoverflow.com/a/31638988/2771889).

#### Behaviour

We want our state and behaviour to be defined in one place and to be unit tested. As mentioned before we don't want styles to break our app functionality. CSS in JS has the benefit on keeping state-related code in one place and allowing to test critical parts of the style instead of only testing CSS classes (e.g. disabled button, crossed out text). This talk does a great explanation on it: [Michael Chan - Inline Styles, 9:39 - 15:55](https://youtu.be/ERB1TJBn32c?t=579).

Use **CSS in JS**.

#### Component appearence

CSS still presents a big brawback on the convention and entry cost part.  CSS in JS handles the conventions of component appearence well. It has multiple applicable cons, but they can be addressed (see [Conventions](#conventions)).

Use **CSS in JS**.

#### Cross-component appearence

Most benfits of CSS-in-JS don't apply here. Testing these is not in the scope of unit testing. For layouts best practices are estiablished and there're many libraries abstracting away most of the CSS work. For global styles scoping is not an issue, they tend to be more straightforward and wouldn't make use of the conventions of CSS-in-JS. CSS-in-JS can be used to keep the way of styling unified, but Preprocessed CSS modules and sometimes even regular CSS can meet all needs.

Use either **Preprocessed CSS modules**, **CSS in JS** or **Regular CSS**.


## Conventions

Despite our efforts there're still some custom conventions we have establish but only a small number of straightforward ones.

#### Never interpolate user input with `styled-components`

- [Article describing the vulnerability](https://reactarmory.com/answers/how-can-i-use-css-in-js-securely)
- [GitHub issue, currently open](https://github.com/styled-components/styled-components/issues/1105)

#### Store CSS-in-JS styles in separate files

It is very often the case that CSS-in-JS is presented with examples where it is part and parcel of the components. This gives the wrong idea and (understandably) scraes off poeple. Separating the concerns of style and business logic is encouraged on many levels. Starting on component level (see: ["Presentational and Container Components" by Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)) reaching down to `styled-components` specifics (see: ["Separate your code with Styled Components" by Sara Vieira](https://blog.cloudboost.io/separate-your-code-with-styled-components-ec4fd1ee3ef8), ["Get Organized with Styled-Components" by Jeremy Davis](https://www.toptal.com/javascript/styled-components-library)).

Our approach is inspired by said articles, aiming to provide a clear and generic separation between styles and components. The main difference being that we don't allow styled components to be defined in style files. This in turn:
- makes the separation abundantly clear
- makes the use of styles explicit in the component files
- gets rid of the differentiation between the props and non-props versions

Component styles are declared in a separate `.styles.js` file using the [`css` helper function](https://www.styled-components.com/docs/api#css).

#### Use SMACSS categories for cross-component styles

[SMACSS categories](https://smacss.com/book/categorizing) provide a practical separation of cocnerns. Module and state categories are covered by the `component.style.js` pattern, however styles that reach accross components (whether because they are global or just affect many components) need a proper place. Place them in `src/common_styles` and use SMACSS catogires for grouping. Applies both to CSS and CSS-in-JS.


## CSS-in-JS example

```
.
└── src
    ├── common_styles
    │   └── layout.scss
    └── components
        └── Header
            ├── Header.js
            └── Header.style.js
```

Header.js
```js
import styled from 'styled-components';

import HeaderStyles from './style.js';

const Header = () => {
  // ...
};

export default styled(Header)`${HeaderStyles};`;
```

Header.style.js
```js
import { css } from 'styled-components';

const HeaderStyles = css`
  margin: 0.5rem 1rem;
  width: 11rem;
  background: transparent;
  color: white;

  ${p => p && p.secondary`
    background: white;
    color: palevioletred;
  `}
`;

export default HeaderStyles;
```

## Further reading

- ["Why We Use Styled Components at Decisiv"](https://medium.com/@_alanbsmith/why-we-use-styled-components-at-decisiv-a8ac6e1507ac)
- [React.js inline style best practices](https://stackoverflow.com/questions/26882177/react-js-inline-style-best-practices/31638988#31638988)
- [The ultimate CSS battle: Grid vs Flexbox](https://hackernoon.com/the-ultimate-css-battle-grid-vs-flexbox-d40da0449faf)
- [Comparison os styling variants](https://github.com/styled-components/comparison)

Things to keep an eye on:
- https://github.com/styled-components/styled-components/issues/1209
- https://github.com/Decisiv/styled-components-modifiers
