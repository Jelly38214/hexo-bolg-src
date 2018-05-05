---
title: React
date: 2018-05-05 09:19:35
tags: React
---

> what different between Reactv15 and Reactv16

Since v16, React use Fiber structure to update React componet, that process is asynchronous instead of synchronous in v15.

Update process in Fiber has two phrase
1.  reconciliation phrase 
2.  commit phrase 

In reconciliation phrase, React find DOM needed to be updated. This phrase cant be interrupted if React found there are some more priority task needed to be executed.

In commit phrase, React update the DOM and wouldn't be interrupted.

In reconciliation phrase, these component recycle will be called, so they are be affected. Ther will be called several times properly.
* componentWillMount (caution: avoid execute login much times and you just want to execute one time)
* componentWillUpdate (caution)
* componentWillReceiveProps (called when parent component props update. no big matter)
* shouldComponentUpdate (just return true or false. no big matter)

In commit phrase
* componentDidMount
* componentDidUpdate
* componentWillUnmount

---

> setState Api feature

For React, `UI = f(state)`, function `f` is React and the code we wrote.

There are some features about `setState`
1. setState wouldn't change value of state in React Component `immediately`.
2. setState make component to update for rerendering UI(shouldComponentUpdate => componentWillUpdate => render => componentDidUpdate)
3. React will merge setState when it called `serveral times` in one queue or one loop.

You can change this.state's value because this.state just a object. But that's no any means, in this condition React wont rerender UI.

Only one way you can use to change this.state and make React to rerender is to use setState function.

setState wouldn't change this.state instanlly,So, when React change this.state's value. The answer is in `render`.

Let's which happended after calling setState. four hook called.

1. shouldComponentUpdate => `return true ? no update : update`(this function return false will stop executeting the follow function, But React will change this.state's value. This condition is so special )
2. componentWillUpdate => no update
3. render => update
4. componentDidUpdate => updated

In order to keep false and not waste performance, React will merge setState.

```javascript
  function updateName() {
    this.setState({FirstName: 'Morgan'})
    this.setState({LastName: 'cheng'})
  }

  // equal to

  function updateName() {
    this.setState({FirstName:'Morgan',LastName:'cheng'})
  }

````

`function usage of setState`: setState not only take a object as parameter but also a function.

This function will receive two params: `state and props`(caution: `state` params is not `this.state`, state has the same value of this.state. but they are not the same object.)

```javascript
  function increment(state,props) {
    return {count:state.count + 1}
  }

  //example
  constructor() {
    super();
    this.state =  {
      count: 0
    }
  }
  function incrementMultiple(state, props) {
    this.setState(increment) // => state.count = 1 this.state.count = 0
    this.setState(increment) // => state.count = 2 this.state.count = 0
    this.setState(increment) // => state.count = 3 this.state.count = 0
  }
```



