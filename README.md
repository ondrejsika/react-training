# React Training

    2019 Ondrej Sika <ondrej@ondrejsika.com>
    https://github.com/ondrejsika/react-training


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
