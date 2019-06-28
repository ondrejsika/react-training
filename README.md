# React Training

    2019 Ondrej Sika <ondrej@ondrejsika.com>
    https://github.com/ondrejsika/react-training


Example repository to this training is [ondrejsika/react-example](https://github.com/ondrejsika/react-example)


## Install Node.js & Yarn

### Mac

```
brew install node
brew install yarn
```


## Install Next.js & React

```
yarn add next react react-dom
```

## Add Next.js scripts to `package.json`

```json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```


## Create a First Page

File `./pages/index.js`

```js
function Home() {
  return <div>Welcome to Next.js!</div>;
}

export default Home;
```


## Run App

```
yarn dev
```

See <http://127.0.0.1:3000>

Keep running, it will be updated automatically. Try it, change the `index.js`!

## Create Simple Component

Create file `./components/Hello.js`

```js
let Hello = (props) => {
  return <>
    <h1>Hello {props.name}</h1>
  </>
}

export default Hello;
```

Use it in `index.js`

```js
import Hello from '../components/Hello'

function Home() {
  return <>
    <Hello name='Zuz' />
  </>;
}

export default Home;
```

and check it out <http://127.0.0.1:3000>


## Add Styles to Component

Update our hello component like that:

```js
let Hello = (props) => {
  return <>
    <h1 style={{
      color: 'green',
      backgroundColor: 'lightblue',
    }}>Hello {props.name}</h1>
  </>
}
```

You can add styles using `style` property. Styles must be object, where key is camel case name of css property and value is string with its value.

Check it out <http://127.0.0.1:3000>


## Add Primer Design System

Primer is design system (like Bootstrap) by Github. See <https://primer.style>


### Install Dependencies

```
yarn add styled-components @primer/components @primer/next-pages
```

### Create Next.js Config

Create a file `next.config.js`:

``` js
module.exports = {};

const withPages = require('@primer/next-pages/plugin')
module.exports = withPages(module.exports)
```

We have to use primer plugin for Next.js

### Use Primer Components

Import Primer Components to `pages/index.js`:

``` js
import { Box, BaseStyles } from "@primer/components";
```

Use components

```js
function Home() {
  return <>
    <BaseStyles>
      <Box width={[1/2]}>
        <Hello name="Zuz" />
      </Box>
    </BaseStyles>
  </>;
}
```

Restart the server (because Next.js doesn't watch for changes is `next.config.js`) and see <http://127.0.0.1:3000>


## Static Rendering

You need to add pages you want to render statically to `next.config.js`:

```js
module.exports = {
  exportPathMap: async function(defaultPathMap) {
    return {
      '/': { page: '/' },
    };
  }
};
```

Add static build script to `package.json`:

```
{
  "scripts": {
    "static": "yarn run build && next static",
  }
}
```

Now you can build site using:

```
yarn build static
```

And see your statically builded website in `out/`.

Dont forget to add the `out` to `.gitignore`.


### Primer Components & Static Render

If you're using Primer Components, you have to statically render CSS from primer components.

Create file `pages/_document.js` which define low level HTML document behavior in Next.js. This file is taken drectly from Primer Repository https://github.com/primer/components/blob/master/pages/_document.js>.

```js
import Document from 'next/document'
import { ServerStyleSheet } from 'styled-components'


export default class MyDocument extends Document {
  static async getInitialProps (ctx) {
    const sheet = new ServerStyleSheet()
    const originalRenderPage = ctx.renderPage

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: App => props => sheet.collectStyles(<App {...props} />)
        })

      const initialProps = await Document.getInitialProps(ctx)
      return {
        ...initialProps,
        styles: (
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>
        )
      }
    } finally {
      sheet.seal()
    }
  }
}
```

Now, your pages are built properly. Try build again.

```
yarn run static
```


## Second Page

If you want create another page, you can create `pages/about-me.js`

```js
import { Box, BaseStyles } from "@primer/components";
import Hello from '../components/Hello'

export default () => {
  return <>
    <BaseStyles>
      <Box width={[1/2]}>
        <h1>About me</h1>
      </Box>
    </BaseStyles>
  </>;
}
```

You have to add that page also to `next.config.js`:

```js
module.exports = {
  exportPathMap: async function(defaultPathMap) {
    return {
      '/': { page: '/' },
      '/about-me': { page: '/about-me' },
    };
  }
};
```

Because you've edited Next.js config, you have to restart server.

Then, check it out <http://127.0.0.1:3000/about-me>. You can also build static site.

## Layout in `pages/_document.js` - Don't Repeat Yourself (DRY)

Create method `MyDocument.render` in `_pages/_document.js`:

Add imports

```js
import { Html, Head, Main, NextScript } from 'next/document';
import { BaseStyles } from "@primer/components";
```

and method render, where you can define your layout code. Like that menu or `<BaseStyles>`.

```js
render() {
  return (
    <Html>
      <Head>
      </Head>
      <body>
        <BaseStyles>
          <p>
            <a href="/">Index</a> ~ <a href="/about-me">About me</a>
          </p>
          <Main />
        </BaseStyles>
        <NextScript />
      </body>
    </Html>
  );
}
```

Then, you can remove base styles from your pages.

Check it out <http://127.0.0.1:3000>


## Deploy to zeit.co

### Install `now`

```
yarn global add now
```

### Create Now Config

Create `now.json`:

```json
{
  "version": 2,
  "name": "react-example",
  "builds": [
    {
      "src": "package.json",
      "use": "@now/static-build",
      "config": { "distDir": "out" }
    }
  ]
}
```

and add script to `package.json`:

```
{
  "scripts": {
    "now-build": "yarn run static"
  }
}
```

### Deploy it

Just call

```
now
```

See your deployment on Zeit, eg.: <https://react-example.ondrejsika.now.sh>

