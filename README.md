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

### Summary
#### - Redux: a library for managing global application state with several types of code
- _Actions_: plain objects with a type field, and describe 'what happened' in the app
- _Reducers_: functions that calculate a new state value based on previous state + an action
- _Store_: runs the root reducer whenever an action is _dispatched_


## Part2: Redux Concepts and Data Flow
https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow


### Background Concepts
#### State Management

```JavaScript
// State: a counter value
function Counter() {
  const [counter, setCounter] = useState(0);

// Action: code that causes an update to the state when something happnes
  const increment = () => {
    setCounter(prevCounter => prevCounter + 1);
  }

// View: the UI definition
  return (
    <div>
      Value: {counter} 
      <button onClick={increment} > Increment </button>
    </div>
  )
}
```

It is a self-contained app with the following parts:
- State: the source that drives the app
- View: a declarative description of the UI based on the current state
- Actions: the events that occur in the app based on user input, and trigger updates in the state

This is __'one-way data flow'__:
- State describes the conditon of the app at a specific point in time
- The UI is rendered based on that state
- When something happens (such as a user clicking a button), the state is updated based on what occurred
- The UI re-renders based on the new state
-> this simplicity can break down when multiple components need to share and use the same state, and they may be located in different parts of the application

Solution? one way to solve this is to extract the shared state from the components, and put it into a centralized location outside the component tree. With this, the component tree becomes a big 'view', and any component can access the state or trigger actions, no matter where they are in the tree.
-> this is the basic idea behind Redux: a single centralized place to contain the global state in the application, and specific patterns to follow when updating that state to make the code predictable. 

#### Immutability
Redux expects that all state updates are done __immutably__. So update will be done by copying the existing object and modifying the copied one. 

### Redux Terminology
#### Actions
- a plain JavaScript object that has a _type_ field
- described as an event that describes something that happened in the application
- the __type__ field should be a string that gives this action a descriptive name, like _todos/todoAdded_ or the convention below
  * featureOrCategory/specificThingThatHappened
- can have other fields with additional information about what happened, called __payload__

```JavaScript
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```

#### Reducers
- a function that receives the current __state__ and an __action__ object, decices how to update the state if necessary, and returns the new state
- described as an event listener which handles events based on the received action (event) type
- specific rules that reducers must follow:
  * only calculate the new state value based on the _state_ and _action_ arguments
  * not allowed to modify the existing state. Instead, make immutable updates by copying the existing _state_ and making changes to the copied values
  * must not do any asynchronous logic, calculate random values, or cause other side effects
- the flow of the logic inside reducer functions
  * check to see if the reducer is responsible for a certain action
    * if so, make a copy of the state, update the copy with new values, and return it
    * otherwise, return the existing state unchanged

```JavaScript
// initializing a state
const initialState = { value: 0 };

// a reducer that receives state and action
// passing state assigned as initialState (setting its defualt value)
function counterReducer(state = initialState, action) {
  // if action type is counter/incremented, increment the state by 1 and return the new state
  if(action.type === 'counter/incremented') {
    return {
      ...state,
      value: state.value + 1
    };
  };

  // otherwise return the existing state unchanged
  return state;
}
```

#### Store
- the current Redux application state lives in an object called the __store__
- created by passing in a reducer, and has a method called __getState__ that returns the current state value:

```JavaScript
import {configureStore } from '@reduxjs/toolkit';

const store = configureStore({ reducer: counterReducer });
console.log(store.getState());
```

#### Dispatch
- a method that the Redux store has
- the only way to update the state: calling store.dipatch() and pass in an action object
- runs its reducer function and save the new state value inside, and we can call getState() to retrieve the updated value:

```JavaScript
// passing in an action object to the dispatch method
store.dispatch({ type: 'counter/incremented' });
console.log(store.getState());
```

#### Selectors
- functions that know how to extract specific pieces of information from s store state value
- when reading the same data several times in the application, you can define this selector and reuse it where you want to, instead of writing the same code to retrieve the data repeatedly

```JavaScript
const selectCounterValue = state => state.value;

const currentValue = selectCounterValue(store.getState());
console.log(currentValue);
```

### Core Concepts and Principlestion
#### Single Source of Truth
- the global state of the application is stored in a single __store__
- the store only exists in one location, rather than being duplicated in many places
-> makes it easier to debug and inspect the state as things change
-> BUT this does not mean every piece of state in the application should go into the Redux store. you should decide whether a piece of state belongs in Redux or your UI components, based on where its needed.

#### State is Read-Only
- dispatching an action is the only way to change the state

#### Changes are Made with Pure Reducer Functions
- define reducer functions to update the state based on actions

### Redux Application Data Flow
- initial setup:
  * a Redux store is created using a root reducer function (configureStore({ reducer: reducerName }))
  * the store calls the root reducer once, and saves the return value as its initial state (func reducerName(state = initialState, action) )
  * When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render
- updates:
  * when something happenes like a buttn got clicked, the application dispatches an action to the Redux store, like dispatch({ type: counter/incremented })
  the store runs the reducer function again with the previous state and the current action, and saves the return value as the new state
  * the store notifes all parts of the UI that are subscribed that the sate has been updated
  * each UI component that needs data from the store checkes to see if the parts of the state they need have changed
  * each component that sees its data has changed forces a re-render with the new data, so it can update what's sown on the screen

### Summary
- Redux's intent in three principles
  * global app state kept in a single store
  * read-only to the rest of the app
  * reducer functions to update the state in response to actions
- 'one-way data flow' app structure
  * state -> something happened -> dispatches an action -> store runs reducers & update the state -> notifies the UI that the state has changed -> UIs re-rendered based on the new state
  