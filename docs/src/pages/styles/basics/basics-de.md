# @material-ui/styles

<p class="description">You can use Material-UI's styling solution in your app, whether or not you are using Material-UI components.</p>

Material-UI aims to provide a strong foundation for building dynamic UIs. For the sake of simplicity, **we expose the styling solution used in Material-UI components** as the `@material-ui/styles` package. You can use it, but you don't have to, since Material-UI is also [interoperable with](/guides/interoperability/) all the other major styling solutions.

## Why use Material-UI's styling solution?

In previous versions, Material-UI has used LESS, then a custom inline-style solution to write the component styles, but these approaches have proven to be limited. We have [adopted a *CSS-in-JS* solution](https://github.com/oliviertassinari/a-journey-toward-better-style). Es ** schaltet viele großartige Funktionen frei ** (Verschachtelung von Themen, dynamische Stile, Selbstunterstützung usw.). Wir denken, das ist die Zukunft:

- [Eine vereinheitlichte Styling-Sprache](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660)
- [SCSS (Sass) in CSS-in-JS umwandeln](https://egghead.io/courses/convert-scss-sass-to-css-in-js)

Material-UI's styling solution is inspired by many other styling libraries such as [styled-components](https://www.styled-components.com/) and [emotion](https://emotion.sh/).

- 💅 Sie können [die gleichen Vorteile](https://www.styled-components.com/docs/basics#motivation) wie bei styled-components erwarten.
- 🚀 Es ist [blitzschnell](https://github.com/mui-org/material-ui/blob/next/packages/material-ui-benchmark/README.md#material-uistyles).
- 🧩 It's extensible via a [plugin](https://github.com/cssinjs/jss/blob/next/docs/plugins.md) API.
- ⚡️ It uses [JSS](https://github.com/cssinjs/jss) at its core – a [high performance](https://github.com/cssinjs/jss/blob/next/docs/performance.md) JavaScript to CSS compiler which works at runtime and server-side.
- 📦 Less than [15 KB gzipped](https://bundlephobia.com/result?p=@material-ui/styles); and no bundle size increase if used alongside Material-UI.

## Installation

Um die Abhängigkeit zu ihrer `package.json` hinzuzufügen, führen Sie folgenden Befehl aus:

```sh
// mit npm
npm install @material-ui/styles

// mit yarn
yarn add @material-ui/styles
```

## Erste Schritte

We provide 3 different APIs to generate and apply styles, however they all share the same underlying logic.

### Hook API

```jsx
import React from 'react';
import { makeStyles } from '@material-ui/styles';
import Button from '@material-ui/core/Button';

const useStyles = makeStyles({
  root: {
    background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
    border: 0,
    borderRadius: 3,
    boxShadow: '0 3px 5px 2px rgba(255, 105, 135, .3)',
    color: 'white',
    height: 48,
    padding: '0 30px',
  },
});

export default function Hook() {
  const classes = useStyles();
  return <Button className={classes.root}>Hook</Button>;
}
```

{{"demo": "pages/styles/basics/Hook.js"}}

### Styled components API

Note: this only applies to the calling syntax – style definitions still use a JSS object. You can also [change this behavior](/styles/advanced/#string-templates), with some limitations.

```jsx
import React from 'react';
import { styled } from '@material-ui/styles';
import Button from '@material-ui/core/Button';

const MyButton = styled(Button)({
  background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
  border: 0,
  borderRadius: 3,
  boxShadow: '0 3px 5px 2px rgba(255, 105, 135, .3)',
  color: 'white',
  height: 48,
  padding: '0 30px',
});

export default function StyledComponents() {
  return <MyButton>Styled Components</MyButton>;
}
```

{{"demo": "pages/styles/basics/StyledComponents.js"}}

### Higher-order component API

```jsx
import React from 'react';
import PropTypes from 'prop-types';
import { withStyles } from '@material-ui/styles';
import Button from '@material-ui/core/Button';

const styles = {
  root: {
    background: 'linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)',
    border: 0,
    borderRadius: 3,
    boxShadow: '0 3px 5px 2px rgba(255, 105, 135, .3)',
    color: 'white',
    height: 48,
    padding: '0 30px',
  },
};

function HigherOrderComponent(props) {
  const { classes } = props;
  return <Button className={classes.root}>Higher-order component</Button>;
}

HigherOrderComponent.propTypes = {
  classes: PropTypes.object.isRequired,
};

export default withStyles(styles)(HigherOrderComponent);
```

{{"demo": "pages/styles/basics/HigherOrderComponent.js"}}

## Verschachteln von Selektoren

Sie können Selektoren verschachteln, um Elemente innerhalb der aktuellen Klasse oder Komponente anzuvisieren. The following example uses the Hook API, but it works the same way with the other APIs.

```js
const useStyles = makeStyles({
  root: {
    padding: 16,
    color: 'red',
    '& p': {
      color: 'green',
      '& span': {
        color: 'blue'
      }
    }
  },
});
```

{{"demo": "pages/styles/basics/NestedStylesHook.js"}}

## Anpassung basierend auf Eigenschaften

You can pass a function to `makeStyles` ("interpolation") in order to adapt the generated value based on the component's props. The function can be provided at the style rule level, or at the CSS property level:

```jsx
const useStyles = makeStyles({
  // style rule
  foo: props => ({
    backgroundColor: props.backgroundColor,
  }),
  bar: {
    // CSS property
    color: props => props.color,
  },
});

function MyComponent() {
  // Simulated props for the purpose of the example
  const props = { backgroundColor: 'black', color: 'white' };
  // Pass the props as the first argument of useStyles()
  const classes = useStyles(props);

  return <div className={`${classes.foo} ${classes.bar}`} />
}
```

Diese Buttonkomponente hat eine Farbeigenschaft, die ihre Farbe ändert:

### Adapting the hook API

{{"demo": "pages/styles/basics/AdaptingHook.js", "react":"next"}}

### Adapting the styled components API

{{"demo": "pages/styles/basics/AdaptingStyledComponents.js"}}

### Adapting the higher-order component API

{{"demo": "pages/styles/basics/AdaptingHOC.js"}}

## Stresstest

Im folgenden Stresstest können Sie die *Themefarbe* und *background-color property* live aktualisieren:

```js
const useStyles = makeStyles(theme => ({
  root: props => ({
    backgroundColor: props.backgroundColor,
    color: theme.color,
  }),
}));
```

{{"demo": "pages/styles/basics/StressTest.js"}}