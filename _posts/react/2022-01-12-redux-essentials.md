---
layout: post
title: "Redux Essentials"
description: "Understanding the essentials of Redux"
date: 2022-01-12
tags: [React]
comments: false
references: ["ReduxJS : https://redux.js.org/"]
---

As React applications grow, managing state across multiple components can become complex and difficult to maintain. This is where Redux comes in, providing a predictable state management solution that helps developers maintain consistency across their applications. In this post, we'll explore Redux, specifically using Redux Toolkit, to make our state management simpler and more efficient.  

## Why Redux ?

As you build larger applications in React, you'll find that managing state becomes increasingly challenging. You might start by passing state down from a parent component to child components through props, a technique known as "props drilling." While this works fine for small applications, it quickly becomes unmanageable as your app scales.

Consider a scenario where multiple components across different parts of your application need access to the same piece of state. Passing this state through several levels of the component hierarchy can be cumbersome, error-prone, and hard to debug. This is where Redux steps in.

Redux provides a centralized store for all your state, making state management predictable and easy to debug. With Redux, your application state is stored in a single location, allowing any component to access it directly. Redux's predictable nature means that the state changes only in response to dispatched actions, which makes the behavior of your application more consistent and easier to debug.  

In Redux, the entire state of an application is stored in a single JavaScript object called the “store.” This allows for a centralized and predictable state management system

## Key Concepts in Redux  

<img src='/images/2024-08-31-13-22-37.png' class='img-responsive'>


### Action
An action is a plain JavaScript object that describes what happened in the application. Every action has a type property, and it may also contain some data (payload).

```js
const incrementAction = {
  type: 'counter/increment',
  payload: 1,
};

```
Think of an action as a message describing something that happened based on a user action, like “Increment the counter by 1

An action is invoked or send using a special function called "Dispatch". 

### Reducer    
A reducer is a function that takes the current state and an action as arguments and returns a new state. Reducers specify how the application’s state changes in response to actions.  

```js 
function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'counter/increment':
      return { ...state, value: state.value + action.payload };
    default:
      return state;
  }
}

```
A reducer is like a rulebook that says, “When this action happens, here’s how to change the state.”  

### Store  
The store is the centralized place where the application state is stored. It brings together actions, reducers, and the current state.  

```js 
import { createStore } from 'redux';
const store = createStore(counterReducer);

```
The store is like a warehouse where all the application data is stored and managed.  

### Slice  
A slice is a portion of the Redux state and the logic related to it, usually created using Redux Toolkit’s createSlice function. Usually a slice is related to a feature in your application.  

```js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
  },
});

export const { increment } = counterSlice.actions;
export default counterSlice.reducer;

```  
As show slice is where we glue together our Initial State, Actions and its corresponding Reducers. In this slice there is only one Action and corresponding Reducer.

### Flow  
When something happens in the app (e.g., a button is clicked), an action is created and dispatched. The reducer receives this action and determines how to update the store. The store then holds the updated state, which the components can access. This flow ensures that the state management in your application is predictable, traceable, and centralized, making it easier to manage complex applications   

## Redux implementation using Redux tool kit  

While Redux is powerful, it can also be verbose and require a lot of boilerplate code. Redux Toolkit, a set of tools from the Redux team, addresses this by simplifying the setup and reducing the amount of code you need to write. It makes it easier to create slices of state, manage reducers, and handle asynchronous logic.  

Let’s dive into a simple example using Redux Toolkit with TypeScript. We'll create a counter application that allows users to increment, decrement, and set a custom value. 

### 1. Setting Up Your Project  
First, let's install Redux Toolkit, React-Redux, and TypeScript:

```bash
npm install @reduxjs/toolkit react-redux @types/react-redux typescript

```
Make sure your project is set up with TypeScript by having a tsconfig.json file. If you don’t have one yet, you can generate it with

```bash
npx tsc --init
```
## 2. Creating a Simple Counter Example

### Setting Up the Provider

Before we dive into creating our counter example, it's crucial to set up the Redux provider in your React application. The Provider component from react-redux makes the Redux store available to any nested components that need to access the Redux state or dispatch actions.  


```js
//Index.tsx or App.tsx 
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```
In this code:  

We import the Provider component from react-redux.We wrap our App component with the Provider and pass the store as a prop.This setup ensures that any component within App can connect to the Redux store using hooks like useSelector and useDispatch.
Now, with the provider in place, your React components are ready to interact with the Redux store.   

### Store Setup

In your Redux setup, the store is the centralized place where the state lives. Here’s how you can set up a store in TypeScript: 


```js
//Store.ts  
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

export default store;

```  
This code configures the Redux store using configureStore from Redux Toolkit and adds the counter reducer to it. We also define RootState and AppDispatch types, which will help us type the selectors and dispatch functions in our components. Not RootState and AppDispatch types are specifically for Typescript.  

### Creating a Slice  
A slice in Redux Toolkit is a collection of reducer logic and actions for a single feature of your app. Let’s create a slice for our counter with TypeScript.

```js
//counterSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
  value: number;
}

const initialState: CounterState = {
  value: 0,
};

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;

```
In this example, we define a CounterState interface to describe the state structure. The PayloadAction<number> type is used for actions that carry a payload, ensuring type safety

### Connecting Redux to React

Next, let’s create a React component that interacts with our Redux store:  

```js
// Counter.tsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { RootState, AppDispatch } from './store';
import { increment, decrement, incrementByAmount } from './counterSlice';

const Counter: React.FC = () => {
  const count = useSelector((state: RootState) => state.counter.value);
  const dispatch: AppDispatch = useDispatch();

  return (
    <div>
      <div>
        <button onClick={() => dispatch(decrement())}>-</button>
        <span>{count}</span>
        <button onClick={() => dispatch(increment())}>+</button>
      </div>
      <button onClick={() => dispatch(incrementByAmount(5))}>Increase by 5</button>
    </div>
  );
};

export default Counter;

```  
In this component, useSelector is typed with RootState to ensure type safety when accessing the state, and useDispatch is typed with AppDispatch to ensure that only valid actions can be dispatched.  

## Handling Asynchronous Actions  
Real-world applications often need to deal with asynchronous data, such as fetching data from an API. Redux Toolkit provides a powerful utility, createAsyncThunk, to handle these asynchronous actions.  

Handling asynchronous actions in Redux can be tricky. You need to manage different states: loading, success, and error. Traditionally, this involves writing a lot of boilerplate code, but Redux Toolkit simplifies this with `createAsyncThunk`.  

Let’s create an example where we fetch user data from an API using TypeScript. 

```ts
// userSlice.ts 
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';
import axios from 'axios';

interface User {
  id: number;
  name: string;
}

interface UserState {
  entity: User | null;
  loading: 'idle' | 'loading' | 'failed';
}

const initialState: UserState = {
  entity: null,
  loading: 'idle',
};

export const fetchUser = createAsyncThunk<User, number>(
  'user/fetchUser',
  async (userId) => {
    const response = await axios.get(`/api/user/${userId}`);
    return response.data;
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState,
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = 'loading';
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = 'idle';
        state.entity = action.payload;
      })
      .addCase(fetchUser.rejected, (state) => {
        state.loading = 'failed';
      });
  },
});

export default userSlice.reducer;

```  
In this code:

* We define a UserState interface to represent the state structure.  
* We use createAsyncThunk with TypeScript’s generic types to specify the return type (User) and argument type (number).
* The extraReducers property handles different states of the async action: pending, fulfilled, and rejected.  

The action passes through a pending state before either being fulfilled or rejected. The state is then updated in the store accordingly.   

In the next post we will discuss how we can use Redux Saga's to handle the Asynchronous 
calls.

