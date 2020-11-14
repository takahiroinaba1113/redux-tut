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
#### The Redux **Store**
- a container taht holds your application's global state 
- a JavaScript objecct with a few special functions and abilities that are different than a plain glboal object:
    * never directly modify or change the state that is kept inside the Redux store
    * update the state by creating a plain _action_ object that describes 'something that happened in the application', and then _dispatch_ the action to the store to tell it what happened
    * when an action is dispatched, the store runs the root _reducer_ function, and lets it calculate the new state based on the old state and the action

## Part2: Redux Concepts and Data Flow


## Part3: State, Actions, Reducers