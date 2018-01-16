---
title: redux_reducers
date: 2018-01-16 22:56:29
tags: redux reducer
---

> Conclusion: reducers in redux must be pure function

what's pure function? 

`A pure function is a function where the return value is only determined by its input values, without observable side effects.`

pure function will not modify its input values.

let's see code block below

```javascript
  // pure situation
  const todo = (state, action) => {
    switch(action.type) {
      case 'TOGGLE_TODO':
        if(state.id !== action.id) {
          return state
        }
        return {
          ...state,
          completed: !state.completed
        }
    }
  }
```

```javascript
  // non-pure situation
  const todo = (state, action) => {
    switch(action.type) {
      case 'TOGGLE_TODO':
        if(state.id !== action.id) {
          return statue
        }

        // mutate the status's completed prop directly
        state.completed = !state.completed // change original object
        return state
    }
  }
```

compare above two situations, only one difference is non-pure situation, the reducers changes the state and return it.In this situation, what would happend? result is the view not re-render event though you dispatch action.

so what cause that.

it's time to dig out source code.

![](http://p150tzuds.bkt.clouddn.com/image/redux/reduces_src.png)

the source code tell us clearly why reducers are pure function and why you cant execute asynchronous code in reducers


reference:
  * [为什么redux需要reducers是纯函数](http://www.zcfy.cc/article/why-redux-need-reducers-to-be-pure-functions-freecodecamp-2515.html)