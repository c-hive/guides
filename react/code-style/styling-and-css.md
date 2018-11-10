# Styling and CSS

This is a huge and controversial topic so we need to be a bit more chatty. It's not obvious that there is (or should be) a one-for-all solution. First we set needs and propose options. Then we break down the evaluation into 3 major topics usually groupped together as 'style': **layout, appearence, behaviour**. 

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
- conventions are completely separate from other project conventions (e.g. naming, structure) and have similar complexity (relatively high cost of entry)

#### CSS in JS

Sources:
- [Generations / evolution of libs]((https://github.com/streamich/freestyler/blob/master/docs/en/generations.md))
- [Comparison of libs by Michele Bertoli](https://github.com/MicheleBertoli/css-in-js)
- [Comparison of libs by Radium](https://github.com/FormidableLabs/radium/tree/905227c122b1775775cf8d82c508cce4179cff08/docs/comparison)
- [troch/choosing-a-css-in-js-library](https://gist.github.com/troch/c27c6a8cc47b76755d848c6d1204fdaf)

Out of the many libs the best choice currently seems to be `styled-components`. Pros and cons address this in particular but most apply accros many.

Pros
- feature rich (complete SASS features for JS with [polished](https://github.com/styled-components/polished))
- no issues of regular CSS
- best practices and conventions come with the lib
- JS for logic (e.g. `:nth-child(even)` vs `i % 2`)
- critical parts can be unit tested
- technical concerns are abstracted away (e.g. CSS injection, object creation and structure)
- very simple for small projects
- needs structure / conventions for big projects, but those with project needs as a whole (great articles on the topic: [Structuring our Styled Components | Part I](https://tech.decisiv.com/structuring-our-styled-components-part-i-2bf21fa64b28), [Structuring our Styled Components | Part II](https://tech.decisiv.com/structuring-our-styled-components-part-ii-99c336a3748f))
- good documentation, active community
- low cost of entry also for those only familiar with CSS
- server side and react native support

Cons
- not framework agnostic (but uses CSS syntax which makes it somewhat portable)
- ability to mix component logic with styles
  - solution: store styles in separate files (e.g. [Separate your code with Styled Components](https://blog.cloudboost.io/separate-your-code-with-styled-components-ec4fd1ee3ef8), [`styled-components` Tips and Tricks example](https://github.com/styled-components/styled-components/blob/master/docs/tips-and-tricks.md#using-javascript-to-our-advantage)
- security issue with interpolation ([source](https://reactarmory.com/answers/how-can-i-use-css-in-js-securely), [GitHub issue, currently open](https://github.com/styled-components/styled-components/issues/1105))
  - solution: avoid interpolation!

## Decision

Let's address the concerns separately. [Great discussion about this on StackOverflow](https://stackoverflow.com/a/31638988/2771889).

#### Behaviour

We want our state and behaviour to be defined in one place and to be unit tested. As mentioned before we don't want styles to break our app functionality. **CSS in JS** clearly is superior here, having the benefit on keeping state-related code in one place and allowing to test critical parts of the style instead of only testing CSS classes (e.g. disabled button, crossed out text). This talk does a great explanation on it: [Michael Chan - Inline Styles, 9:39 - 15:55](https://youtu.be/ERB1TJBn32c?t=579).

#### Appearence

CSS still present a brawback on the convention and entry cost part. **CSS in JS** also has multiple issues in this area, but they can be addressed more easily (see [CSS in JS conventions](#css-in-js-conventions)).

#### Layout

The best practices are estiablished with [Grid](https://www.w3schools.com/css/css_grid.asp) and [Flexbox](https://www.w3schools.com/css/css3_flexbox.asp) and there're many libs utilizing these

There're many libraries out there that already solve most issues and you only need to use them.

https://css-tricks.com/guides/layout/

## CSS in JS conventions


## Further sources

- ["Why We Use Styled Components at Decisiv"](https://medium.com/@_alanbsmith/why-we-use-styled-components-at-decisiv-a8ac6e1507ac)
- [React.js inline style best practices](https://stackoverflow.com/questions/26882177/react-js-inline-style-best-practices/31638988#31638988)
