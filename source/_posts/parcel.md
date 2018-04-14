---
title: parcel
date: 2018-03-18 19:28:01
tags: bundler；parcel
---
That's no doubt that a lots of developer are tired of complex configuration of webpack. Now there is a new bundler -- parcel to make you easy.
Let me use it to create a react app.

```javscript
  // init project
  mkdir react-parcel <br>
  cd react-parcel <br>
  npm init -y
```

```javscript
  // install necessary dependencies

  npm install -S react react-dom
  npm install -D babel-preset-react babel-preset-env parcel-bundler
```
Next, pls create the .babelrc file. This file tells parcel that I are using ES6 and React JSX.

```javascript
  // .babelrc
  {
    "presets": ["env", "react"]
  }
```

Next create react app.

```javascript
  // index.js
  import React from 'react'
  import ReactDOM from 'react-dom'

  class HelloMessage extends React.Component {
    render() {
      return <div>Hello {this.props.name}</div>
    }
  }
  const mountNode = document.getElementById('app')
  ReactDOM.render(<HelloMessage name="Jelly" />, mountNode)
```

```html
  <!DOCTYPE html>
  <html>
    <head>
    <title>React starter app</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="index.js"></script>
  </body>
  </html>
```
Now need to add a script entry to package.json to be able to start app.

```json
  "script": {
    "start": "parcel index.html"
  }
```
Let's start app, then browser to `http://localhost:1234` to see app
```javascript
  npm start
```

## Reference
  * https://parceljs.org/recipes.html
  * http://blog.jakoblind.no/react-parcel/