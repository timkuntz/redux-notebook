## Bare Bones Redux

A basic Redux clone can be written in a few lines of code and packaged as single function. The following example is taken from an excellent article on [Idiomatic Redux](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/).

```javascript
function createStore(reducer) {
  var state;
  var listeners = []

  function getState() {
    return state
  }

  function subscribe(listener) {
    listeners.push(listener)
    return function unsubscribe() {
      var index = listeners.indexOf(listener)
      listeners.splice(index, 1)
    }
  }

  function dispatch(action) {
    state = reducer(state, action)
    listeners.forEach(listener => listener())
  }

  dispatch({})

  return { dispatch, subscribe, getState }
}
```

Using code from the [Vanilla Counter](https://raw.githubusercontent.com/reduxjs/redux/master/examples/counter-vanilla/index.html) Redux example, basic usage is as follows:

Create a reducer that takes current state and an action to perform on that state.
```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

Create a store to hold your state, passing in the reducer.
```javascript
var store = createStore(counter)
```

Dispatch actions to modify your state.
```javascript
store.dispatch({ type: 'INCREMENT' })
```

Subscribe to changes to render updates.
```javascript
function render() {
  valueEl.innerHTML = store.getState().toString()
}
store.subscribe(render)
```

Unsubscribe if handler may no longer be needed.
```javascript
unsubscribe = store.subscribe(render)
function componentWillUnmount() {
  unsubscribe()
}
```
