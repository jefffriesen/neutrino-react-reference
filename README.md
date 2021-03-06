# Neutrino React Reference

### But neutrino is about making it so simple that you don't need a boilerplate. Why?!?
Because it's still easier to just clone this repo and go.

Neutrino is easy and it's a welcome relief to babel and webpack configs. And the documentation gets you a basic app fast. But inevitably you will have to do simple or advanced customizations and it's nice to have a reference implementation on top of the documentation.

I've created 2 repos to serve as these reference implementations. They are a work in progress but they should get your up and running with Neutrino.js version 4 quickly (version 5 isn't officially released yet).

#### 1. [neutrino-react-reference](https://github.com/jefffriesen/neutrino-react-reference)
Simple React app. Uses Mobx as a state container mainly just to show use of decorators. It's easy to replace Mobx with Redux or whatever you use. The functionality here
intentionally minimal, but it comes with a scaffolding generator to build out
features quickly.
  * [Mobx](https://mobx.js.org) with colocated stores
  * [Class properties](https://github.com/jefffriesen/neutrino-preset-class-properties)
  * [Decorators](https://github.com/jefffriesen/neutrino-preset-decorators)
  * [Typechecking with tcomb](https://github.com/gcanti/babel-plugin-tcomb) (It's [Flow](https://flowtype.org) compatible but has some advantages over Flow)
  * Linting (not yet using a linting preset)
  * [Jest](https://facebook.github.io/jest/) with tests colocated with feature components
  * [Bootstrap](http://getbootstrap.com)
  * [React-Bootstrap](https://react-bootstrap.github.io)
  * [React Router 4](https://reacttraining.com/react-router/web/guides/quick-start)
  * [Scaffolding generator (Plop)](https://plopjs.com) to easily create feature-first components
  * [Remotedev](https://github.com/zalmoxisus/mobx-remotedev) Remote debugging for Mobx with [Redux Devtools extension](https://github.com/zalmoxisus/redux-devtools-extension)

#### 2. [neutrino-electron-reference](https://github.com/jefffriesen/neutrino-electron-reference)
Same app as above but uses Electron for publishing into a distributable app on Mac, Windows and Linux.


## To Run
You can either use `yarn` or `npm`
```
yarn install    // Load dependencies and create yarn.lock file
yarn start      // Run locally (or `npm start`)
yarn build      // Build for deployment
yarn test       // Run all tests found in the codebase
yarn coverage   // See test coverage with Jest
yarn scaffold   // CLI to generate the scaffolding for a component, tests & styling
```
--------------------------------------------------------------------------------

## Writing Tests
You can create app features in their own directory. Normally for neutrino.js, Jest will only look in the test/ folder (https://neutrino.js.org/presets/neutrino-preset-jest/). Because of an override in neutrino-custom.js, Jest will look for and run any test file as long as it follows one of these naming patterns:
```
*.test.js
*_.test.js
*.spec.js
*_spec.js
.jsx
```

dependencies: `neutrino-preset-jest`, `enzyme`, `react-addons-test-utils`

--------------------------------------------------------------------------------

## Scaffolding Generator
Quickly build a feature component but running `yarn scaffold` or `npm run scaffold`. For example, you want a Navbar component:
```
yarn scaffold  // follow instructions. You will be able to create a React function, class and optionally hook it up to a store.
```
Creating a feature called `ClockFeature` with a component called `Clock` scaffolds out:
```
- src
  - clock-feature
    - components
      - clock.css       // CSS Module for the React component
      - clock.jsx       // React component
      - clock.text.jsx  // Test scaffold with a 'will it render' sanity check
    - clock-feature.store.js
    - index.js
```

Make sure to import the new store and add it to the `<Provider` component inside index.js:

```js
import {clockFeatureStore} from './clock-feature';

render(
  <Provider store={store} clockFeatureStore={clockFeatureStore}>
    <App />
  </Provider>,
  document.getElementById('root'),
);
```
This scaffolding is an example of [feature-first architecture](https://medium.com/front-end-hacking/the-secret-to-organization-in-functional-programming-913484e85fc9#.4zpdahe2f) that colocates all code for a feature together instead of putting all of the view components in a folder, the stores in another and tests in another folder.

dependencies: `plop`, `inquirer-directory`

--------------------------------------------------------------------------------

## Linting
I know you are suppose to be able to write a dynamic `.eslintrc.js` file with Neutrino. I kept getting errors so I replaced it with a static `.eslintrc`. I'm waiting until neutrino 5 to see if I can get it working again. Currently I use [prettier-atom](https://atom.io/packages/prettier-atom) to reformat my code on save. So far no conflicts between my lint settings and `prettier`.

dependencies: `eslint`, `eslint-plugin-react`

--------------------------------------------------------------------------------

## CSS Modules
Write modern CSS and import them like you do JS modules. All imported styles are considered global unless it is locally scoped.
https://github.com/webpack-contrib/css-loader

```css
:local .counter {
  font-weight: 900;
  font-size: 1.2em;
}

:local .counterValue {
  composes: counter;
  color: blue;
}
```

Importing and exporting variables:
```css
/* palettes.css */
@value ORANGE: #FFBD41;
@value BROWN: #302D2B;
```

Inside another css file:
```css
@value palettes: "../shared/styles/palettes.css";
@value BROWN, ORANGE from palettes;

body {
  background-color: ORANGE
}
```

Global styles like Bootstrap or Foundation can just be imported without assignment. Assign local styles to a variable and use them in the `className` prop:
```js
import React from 'react';
import './bootstrap.css' // global styles
import styles from './counter.css'; // local component styles

export default class Table extends React.Component {
  render () {
    return <span className={styles.counterValue}> {counter}</span>
}
```

Working with `:local` styles combined with global styles is also possible. For example, you can override a Bootstrap nav element like this:
```css
:local :global(.nav > li > a).listItem {
  color: tomato;
}
```


--------------------------------------------------------------------------------

#### TODO:
* Wait until neutrino v5 lands and then update these dependencies with neutrino-react-reference as the upstream.
* Add current route to store (Link to mwestrate's blog post)

#### Documentation
* tcomb & Flowtypes use
* mobx remotedev
