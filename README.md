# Redux Fundamentals
https://redux.js.org/tutorials/fundamentals/part-1-overview

## Part1: Redux Overview
### What is Redux?
- a pattern and library for managing and updating application state, using events called 'actions'
- a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be udpated in a predictable fashion

### Why use Redux?
- to manage 'global state' - state that is needed across many parts of your application

### When use Redux?
- you have large amounts of application state that are needed in many places in the app
- the app state is updated frequently over time
- the logic to update that state may be complex
- the app has a medium or large-sized codebase(the complete body of source code for a given software programm or application), and might be worked on by many people 

### Redux Libraries and Tools
#### Redux 
- a small standalone JS library, commonly used with several other packages

#### React-Redux
- an official package that lets your React components interact with a Redux store by reading pieces of state and dispatching actions to update the store

#### Redux Toolkit
- a toolkit that contains packages and functions that are essential for building a Redux app (a recommended approach by the official)

#### Redux DevTools Extension
- the extension that shows a history of the changes to the state in your Redux store over time
- allows you to debug your applications effectively, including using techniques like 'time-travel debugging'


### Redux Basics
#### The Redux __Store__
- a container taht holds your application's global state 
- a JavaScript objecct with a few special functions and abilities that are different than a plain glboal object:
    * never directly modify or change the state that is kept inside the Redux store
    * update the state by creating a plain __action__ object that describes 'something that happened in the application', and then __dispatch__ the action to the store to tell it what happened
    * when an action is dispatched, the store runs the root __reducer__ function, and lets it calculate the new state based on the old state and the action
    * finally, the store notifies __subscribers__ that the state has been updated so the UI can be updated with the new data

##### State, Actions, and Reducers (examples)
1. Define an initial __state__ value to describe the application:

```JavaScript
const initialState = {
    value: 0
}
```

2. Define a __reducer__ function which receives two arguments: the current state and an action object describing what happened.

```JavaScript
function counterReducer(state = initialState, action) {
    switch(action.type) {
        case 'counter/incremeneted':
            return { ...state, value: state.value + 1 }
        case 'counter/decremented':
            return { ...state, value: state.value - 1 }
        default:
            return state;
    }
}
```

- action object always have a __type__ field, which is a string that acts as a unique name for the action
- the __type__ should be a readable name
- based on the type of the action, return a new object to be the new __state__ result, or return the existing __state__ object if nothing should change
- the state is updated immutably by copying the existing state and updating the copy, instead of modifying the original object directly

##### Store
- create a __store__ instance by calling the Redux library __createStore__ API
- pass the reducer function to __createState__, which uses the reducer function to generate the initial state, and to calculate any future updates

```JavaScript
const store = Redux.createStore(counterReducer);
```

##### UI
- when a reducer and store are ready, integrate them with UI

```JavaScript
const valueEl = document.getElementById('value');

function render() {
    // when the store state changes, update the state and the UI 
    const state = store.getState();
    valueEl.innerHTML = state.value.toString();
}

// Update the UI with the initial data
render();
// And subscribe to redraw whenever the data changes
store.subscribe(render);
```

##### Dispatching Actions
- respond to user input by creating action objects that describe what happened, and dispatching them to the store
- call __store.dispatch(action)__, and then the store runs the reducer, calculates the updated state, and runs the subscribers to update the UI

```JavaScript
// handle user input by dispatching action objects, which should describe what happened in the app

// a simple implementation of dispatch
document.getElementById('incremenet').addEventListener('click', () => {
    store.dispatch({ type: 'counter/incremented' })
});

document.getElementById('decrement').addEventListener('click', () => {
    store.dispatch({ type: 'counter/decremented' })
});

// The below are examples of dispatching an action under certain conditions
document
  .getElementById('incremenetIfOdd')
  .addEventListener('click', () => {
    if(store.getState().value % 2 !== 0){
      store.dispatch({ type: 'counter/incremented' });
    }
})

document
  .getElementById('incremenetAsync')
  .addEventListener('click', () => {
    setTimeout(() => {
      store.dispatch({ type: 'counter/incremented' })
    }, 1000);
})

```

##### Data Flow
1. UI receives input by a user
2. Event Handler dispatches an action to the reducer and return a new value
3. state is updated by the store with the new value
4. UI is updated


## Part2: Redux Concepts and Data Flow


## Part3: State, Actions, Reducers