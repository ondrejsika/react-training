# React Training

    2019 Ondrej Sika <ondrej@ondrejsika.com>
    https://github.com/ondrejsika/react-training


## Install Next.js & React

```
yarn add next react react-dom
```

## Create a First Page

File `./pages/index.js`

```js
function Home() {
  return <div>Welcome to Next.js!</div>;
}

export default Home;
```

## Add scripts to `package.json`

```json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

## Run App

```
yarn dev
```

See <http://127.0.0.1:3000>
