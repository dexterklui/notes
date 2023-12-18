---
title: Redux
date: 2023-08-31 (Thu)
---

# Brief Introduction

## Purpose

For state management, like `useContext` in React. It can avoid **_Prop
Drilling_**.

## Packages

| Package            | Description                |
| ------------------ | -------------------------- |
| `redux`            | For state management       |
| `redux-logger`     | Add logger middleware      |
| `react-redux`      | Integrate react and Redux  |
| `@reduxjs/toolkit` | Simplify Redux store setup |

It seems you don't need `redux` if you are using `react-redux`.

## Basic mechanism

Each application has a root **_store_** that stores all the states managed by
Redux.

To update the value of a state, we call the **_dispatcher_** function by passing
an **_action_** object as argument. An action has two properties, `type` and
`payload`, where `type` specifies what kind of action is used to update the
value and predefined by user arbitrarily, and `payload` is an optional argument
that fine tune how exactly the value of a state is changed.

Upon calling the dispatcher function, Redux will calls the **_root reducer_**
function that is responsible to update the value of states. A **_reducer_**
function accepts two argument, **previous state** and **action**.

Usually, root reducer function does not handle the updates by itself, but calls
other sub-reducer functions by forwarding the arguments.

## Store

Usually defined in `src/redux/store.js`.

```js
import { combineReducers, createStore } from "redux";
import { counterReducer } from "./counter/reducer";

const rootReducer = combineReducers({
  counterStore: counterReducer,
  // Other sub-reducers...
});

const store = createStore(rootReducer);

export default store;
```

Note that `createStore` is deprecated.

## Reducer

```js
import { INCREMENT, DECREMENT, INCREASE } from "./constants";

const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
  let count = state.count;
  switch (action.type) {
    case INCREMENT:
      count = state.count + 1;
      break;
    case DECREMENT:
      count = state.count - 1;
      break;
    case INCREASE:
      count = state.count + action.payload.delta;
      break;
    default:
      break;
  }
  // You cannot mutate state and return it, but must return a new state
  return { ...state, count };
}

export { counterReducer };
```

In `constants.js`:

```js
// You can use string. Symbol is to ensure no conflict of value
export const INCREMENT = Symbol("counterActionIncrement");
export const DECREMENT = Symbol("counterActionDecrement");
export const INCREASE = Symbol("counterActionIncrease");
```

## Accessing and updating a state

```js
import { useDispatch, useSelector } from "react-redux";
import { INCREMENT, DECREMENT, INCREASE } from "../redux/counter/constants";

function Counter() {
  const count = useSelector((store) => store.counterStore.count);
  const dispatch = useDispatch();

  function handleIncrementBtn() {
    dispatch({ type: INCREMENT });
  }

  function handleIncrementBtn() {
    dispatch({ type: DECREMENT });
  }

  function handleIncreaseBtn(n) {
    dispatch({ type: INCREASE, payload: n });
  }

  return (
    <div className="counter" style={counterStyle}>
      {count}
      <div className="counter__btns">
        <button onClick={handleIncrementBtn}>+</button>
        <button onClick={handleDecrementBtn}>-</button>
        <button onClick={() => handleIncreaseBtn(10)}>+10</button>
      </div>
    </div>
  );
}

export default Counter;
```

## Action creaters

Usually it is convenient to define **_action creaters_** to make calling
dispatcher to update states easier.

```js
export function incrementCount() {
  return { type: INCREMENT };
}

export function decrementCount() {
  return { type: DECREMENT };
}

export function increaseByN(n) {
  return {
    type: INCREASE,
    payload: { delta: n },
  };
}
```

A component can then import and use these action creaters.
`dispatcher(increaseByN(3))`.

# Thunk

A **_thunks_** is function that interact with a Redux store's `dispatch` and
`getState()`.

This is particularly useful for writing asynchronous functions.

## Example

```js
async function loginThunk(dispatch, email, password) {
  fetch("https://httpbin.org/post", {
    method: "POST",
    body: JSON.stringify({
      email,
      password,
    }),
    headers: {
      "content-Type": "application/json",
    },
  })
    .then((res) => res.json())
    .then((data) => {
      const jwtToken = data.token;
      if (jwtToken) {
        localStorage.setItem("token", jwtToken);
        dispatch(authActions.login());
      }
    })
    .catch((err) => {
      console.log(err);
    });
}

export const authActions = { ...authSlice.actions, loginThunk };
```

# Reduxjs Toolkit

## Purpose

To simplify the process of setting up stores, states, reducers and action
creaters. With `@reduxjs/toolkit`, we define a **_slice_** for each store.

## Root store

To setup the root store, in `store.js`:

```js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./counter/counterSlice";
const store = configureStore({
  reducer: {
    // property name is the store name, value is the reducer of that slice:
    counterStore: counterReducer,
  },
});
```

## Creating slices

### createSlice

Use `createSlice()` from the package and pass an object to it
(`mySlice = createSlice({...})`) with following properties:

| Property       | Description                          |
| -------------- | ------------------------------------ |
| `name`         | The name for this slice              |
| `initialState` | An object representing initial state |
| `reducers`     | An object of reducer functions       |

Toolkit **automatically generates action creater** for each reducer function
defined. The generated action creater accepts a single argument for the
**payload** of the action.

Each created slice is an object with its own reducers and action creaters:

| Instance Property | Description                             |
| ----------------- | --------------------------------------- |
| `reducer`         | An object with reducer functions        |
| `actions`         | An object with action creater functions |

### Exporting a slice

Either you export a slice, or you export the reducers and actions:

```js
export default mySlice;
export const mySliceReducer = mySlice.reducer;
export const mySliceActions = mySlice.actions;
```

### Example

#### Creating a slice

```js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  count: 0,
};

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    incrementCount: (state) => {
      ++state.count;
    },
    decrementCount: (state) => {
      --state.count;
    },
    increaseCountByN: (state, action) => {
      state.count += action.payload;
    },
  },
});

export const counterActions = counterSlice.actions;
export const counterReducer = counterSlice.reducer;
export default counterReducer;
```

#### Using a slice

```js
import { useDispatch, useSelector } from "react-redux";
import { counterActions } from "../redux/counter/slices";

const counterStyle = {
  display: "grid",
  gridTemplateColumns: "auto auto",
  gridAutoFlow: "dense",
};

function Counter() {
  const count = useSelector((store) => store.counterStore.count);
  const dispatch = useDispatch();

  function handleIncrementBtn() {
    dispatch(counterActions.incrementCount());
  }

  function handleDecrementBtn() {
    dispatch(counterActions.decrementCount());
  }

  function handleIncreaseBtn(n) {
    dispatch(counterActions.increaseCountByN(n));
  }

  function handleChangeRand(min, max) {
    counterActions.changeCountByRandomNumberThunk(dispatch, min, max);
  }

  return (
    <div className="counter" style={counterStyle}>
      {count}
      <div className="counter__btns">
        <button onClick={handleIncrementBtn}>+</button>
        <button onClick={handleDecrementBtn}>-</button>
        <button onClick={() => handleIncreaseBtn(10)}>+10</button>
        <button onClick={() => handleIncreaseBtn(-10)}>-10</button>
        <button onClick={() => handleChangeRand(0, 10)}>+ n ∈ [0, 10]</button>
        <button onClick={() => handleChangeRand(-10, 0)}>- n ∈ [0, 10]</button>
      </div>
    </div>
  );
}

export default Counter;
```

## Specifying middleware

In `store.js`, for the object passed to `configureStore()`, you can specifies
middleware with the property `middleware`.

```js
const store = configureStore({
  reducer: {
  middleware: [ /* list of middleware */ ],
  // Or to extend a default list of middleware:
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
}
```

# Middleware

## Logger

For each update of state values, log the details in the console.

Simply use middleware `logger` from package `redux-logger`.

# Redux DevTools

You can install `Redux DevTools` browser extensions.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
