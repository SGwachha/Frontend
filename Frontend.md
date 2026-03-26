## React States

#### Built-in States

1. useReducer
2. context Api

#### External States

1. Redux/Zustand
2. React Query/Tanstack Query


## React Hooks

#### Helps to store and update state
#### Run side effects (useEffect)
#### Access Context

1. useState => Manage States to store the variable.
2. useEffect => Helps to run the side effect like data fetching, changing DOM.
3. useContext => Helps to pass the data in any level without prop drilling.
4. useReducer => Alternative to useState to manage more complex state logic similar to redux.
5. useMemo => Optimizes performance by storing the result and recalculate the result only when its dependencies changes.
6. useCallbacks => Optimizes performance by storing the function and recalculate the function only when its dependencies changes.
7. useRef => Store the value that does not cause re-render.
8. useEffectLayout => When Ui is having issues. it runs after the DOM has been mutated in the browsers.


## Zustand

A light and fast global state management, which can read and write from anywhere from the project. similar to shared google docs.

#### Ways to use

1. create a store

import { create } from "zustand";

const useStore = create((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
}));

2. Use it

function Counter() {
  const { count, increase } = useStore();

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increase}>Increase</button>
    </div>
  );
}


#### Store Slice

A slice is just a small part of global store. (looks code clean and will get to know which slice is where and where are the issues)

1. Seperate Slice for each parts.

// counterSlice.js
export const createCounterSlice = (set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
});


// userSlice.js
export const createUserSlice = (set) => ({
  user: null,
  login: (user) => set({ user }),
});

2. Combine all slice into one.

import { create } from "zustand";
import { createCounterSlice } from "./counterSlice";
import { createUserSlice } from "./userSlice";

const useStore = create((...a) => ({
  ...createCounterSlice(...a),
  ...createUserSlice(...a),
}));


## Middleware

Middleware allows to run the code before any request is completed. For example, if the user try to login then we can check if the user is valid or not or if the token is there in cookies(browser). we can rewrite, redirect or set headers and cookies. which can be used for authentication, i18n, testing, security. we can give similar to dependencies in useEffect if the matcher match the condition then middleware will run the condition.

#### For Example

// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token')?.value;

  if (request.nextUrl.pathname.startsWith('/protected') && !token) {
    return NextResponse.redirect(new URL('/login', request.url));
  }

  return NextResponse.next();
}

// Optionally, define a matcher to run the middleware only on specific paths
export const config = {
  matcher: ['/protected/:path*'],
};
