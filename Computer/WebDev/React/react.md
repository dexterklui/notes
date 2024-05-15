---
date: 2023-08-19 (Sat)
---

# React

## ⚙️ Installation and Sandbox

### Online Sandbox

Edit or fork a basic React sandbox from
[React - Try React](https://react.dev/learn/installation#try-react)

### Local Sandbox

Download this
[HTML](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html)
and start writing React code within the html file.

### Making a React app

- `npx create-react-app my-app`
- `yarn create react-app my-app`

`cra` is the alias people used for `create-react-app`.

To use TypeScript:

- `npx create-react-app my-app --template typescript`
- `yarn create react-app my-app --template typescript`

Also see [here](https://create-react-app.dev/docs/adding-typescript/) for adding
TypeScript to existing project.

### Using other frameworks

| Framework | Command               | Description                              |
| --------- | --------------------- | ---------------------------------------- |
| Next.js   | `npx create-next-app` | Full-stack framework                     |
| Remix     | `npx create-remix`    | Full-stack with nested routing           |
| Gatsby    | `npx create-gatsby`   | For fast CMS-backed websites             |
| Expo      | `npx create-expo-app` | For native apps (Android, iOS, web apps) |

See
[Production-grade React frameworks](https://react.dev/learn/start-a-new-react-project#production-grade-react-frameworks).

## 🌈 TypeScript

### Installation

See [Making a React app](#making-a-react-app)

### TypeScript files

For a TypeScript file containing JSX, use `.tsx` file extension.

### Type checking

See [React TypeScript](https://react.dev/learn/typescript).

#### Prop types

```ts
function MyButton({ title }: { title: string }) {
  return <button>{title}</button>;
}
```

```ts
interface MyButtonProps {
  title: string;
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return <button disabled={disabled}>{title}</button>;
}
```

#### Hook types

##### Types for useState

Can used inferred type mostly. For explicit type:

```ts
type Status = "idle" | "loading" | "success" | "errror";
const [status, setStatus] = useState<Status>("idle");
```

## 📖 React Basics

### File Structure

- Anything in `public` stores static assets for the index.html and can be
  accessed directly through URL.
  - `index.html` is the entry point of the website
  - `manifest.json` is for Progressive Web App
  - `robots.txt` tells search engine what files should be indexed and what
    shouldn't.
- `src`, standing for _source_, contains files for React Application
  - `index.js` is the entry point for the React Application

### JSX

`JSX` is a syntactic sugar to create HTML elements in JS file. The only
difference is you need to use **camelCase** for tag attributes.

| Attributes       | Description        |
| ---------------- | ------------------ |
| `className=""`   | Classes            |
| `style=(object)` | CSS inline styling |

- `style=({color: "red", backgroundColor: "blue"})`

Inside JSX, you can embed JavaScript **expressions** inside a pair of curly
braces `{}`. If you passed an array, React will render each element in the
array.

#### Creating element with JSX

```javascript
const myPara = <p>Hello</p>;
// equivalent to
const myPara = React.createElement("p", {}, "Hello");
```

### Components

#### Functional and class components

There are **_functional_** and **_class_** components.

A functional components used **PascalCase** naming and returns a JSX expression.
Note that a functional components must return only **one** JSX tag. You must
either wrap the return markup in a parent tag like `<div></div>` or in a **_JSX
fragment_** like `<></>`. Since it might not be supported without babel version
7, use `<React.Fragment></React.Fragment>` if there's an error.

```javascript
function Greeting() {
  return (
    <div>
      <p>Hello</p>
    </div>
  );
}
```

For class components, there is a `render()` method that returns a JSX expression
to be rendered.

#### Using components

It is conventional to **export** the main component of the JS file as default.
Then other files can import the component and use it to compose more complex
JSX.

```javascript
import Greeting from "./Greeting";

function App() {
  return (
    <div>
      <Greeting />
    </div>
  );
}
```

#### Props

**_Props_** stands for properties for components. When you specify HTML
attributes for a component, React **pass** those attributes in key-value pairs
as a JavaScript object as the first argument to a functional component.

```js
function Greeting(props) {
  console.log(props.firstName);
  return <p>Hello {props.firstName}</p>;
}
// Or using destructuring
function Greeting({ firstName }) {
  console.log(firstName);
  return <p>Hello {firstName}</p>;
}
```

To use a component with a given props:

```js
function App() {
  return <Greeting firstName="Tim" />;
}
```

For boolean attributes, you can simply declare it. I.e. `<div isWide>` is
equivalent to `<div isWide={true}>`.

##### Passing props with spread operator

```js
const greetingProps = {
  firstName: Tom,
  lastName: Riddle,
};

function Greeting(props) {
  return (
    <p>
      Hello {props.firstName} {props.lastName}
    </p>
  );
}

function App() {
  return <Gretting {...greetingProps} />;
}
```

##### Children

Anything within the tag is stored as the value of `props.children`.

```javascript
function Card(props) {
  return (
    <div>
      <h2>{props.subtitle}</h2>
      {props.children}
    </div>
  );
}
```

```javascript
function App() {
  return (
    <div>
      <Card subtitle="dog">
        <p>Content of the card</p>
      </Card>
    </div>
  );
}
```

#### States

See [useState](#usestate).

### Map array to create components

It is a common practice to use `map()` to iterate over an array and return JSX.

When using `map()` it is recommended to assign a reserved property `key` to JSX
for each iteration: `key=<unique-key>`, because it helps React to determine what
kinds of changes to the list was made, and this enhances performance.[^1] If you
don't assign explicitly, React uses array indices as the key, but it is
generally problematic when reordering, inserting and removing list items.

```javascript
const GROCERY = ["chocolate", "apple"];
function List() {
  return (
    <ul>
      {GROCERY.map((item, idx) => (
        <li key={idx}>{item}</li>
      ))}
    </ul>
  );
}
```

## 🪝 Hooks

Every function starting with `use` is a **_Hook_**, like the builtin
`useState()`.

You can only call Hooks at the **top** of your **functional** component (or
other Hooks). If you want to use Hooks in a condition or loop, extract a new
component and put it there.

### useState

#### Using states

**_States_** are internal states stored inside a component.

```javascript
import { useState } from "react";
function Greeting() {
  const [age, setAge] = useState(10); // initial age is 10
  return (
    <div
      onClick={() => {
        setAge((prev) => {
          return prev + 1;
        });
      }}
    >
      <p>age: {age}</p>
    </div>
  );
}
```

`setState()` can used to update the value of `state`. It can also receives a
callback function which will receive the current value of `state` as an argument
and return the new value.

Note that `setState()` is **asynchronous**. So you shouldn't run any code
synchronously after `setState()` that depends on the new state value. If you
really want to immediate run some code as soon as a state updates its value, use
`useEffect()`, or wrap those code in another Component.

Using `setState()` function will **re-render asynchronously** this components
and its children. During re-rendering, the body of the functional component runs
again, and any local variables will be set again, except React will handle
states updates automatically, so you don't need to worry states will be
re-initialized.

##### Sharing states

If you need to shares the state across multiple components, you need to _lift
the state up_ to a common ancestor component, and pass the state (and setState
function as a callback) to those components.

```javascript
function App() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <MyButton count={count} />
      <MyButton count={count} />
    </div>
  );
}
```

### useEffect

#### Fetching data

`useEffect()` is useful to fetch data when the component mount.

```js
export default function MyComponent() {
  const [rawData, setRawData] = useState();
  const [isFetching, setIsFetching] = useState(true);
  useEffect(() => {
    let active = true; // prevent race condition in rapid re-rendering
    fetch(url)
      .then((res) => res.json())
      .then((data) => {
        if (!active) return;
        setRawData(data);
        setIsFetching(false);
      });
    return () => {
      active = false;
    };
  }, []);

  if (isFetching) return <p>Fetching data...</p>;
  if (!rawData) return <p>Could not fetch data.</p>;

  return {
    /* component */
  };
}
```

Besides using `active` control variable, we can use `AbortController`.

```js
export default function MyComponent() {
  const [rawData, setRawData] = useState();
  const [isFetching, setIsFetching] = useState(true);
  useEffect(() => {
    const abortController = new AbortController();

    const fetchData = async () => {
      try {
        const res = await fetch(url, { signal: abortController.signal });
        const data = await res.json();
        setRawData(data);
        setIsFetching(false);
      } catch (err) {
        if (err.name === "AbortError") {
          return; // dismount throw abort signal and stop unfinished fetching
        }
        throw err; // handle other request errors
      }
    };

    fetchData();
    return () => {
      abortController.abort();
    };
  }, []);

  if (isFetching) return <p>Fetching data...</p>;
  if (!rawData) return <p>Could not fetch data.</p>;

  return {
    /* component */
  };
}
```

#### Better fetching data alternatives

But note that this is usually not the best way to fetch data. Because there's no
optimizations like caching and server-rendering, it's easy to cause "network
waterfalls", and it's not ergonomic. So it's better to use the framework's
fetching method if you are using a framework (e.g. _Next.js_), or a custom hook
or third-party library (e.g. `useSWR`).

### useCallback

You can pass a function and optionally an array of dependencies to
`useCallback()`. This is useful to prevent redefining an event callback function
every time the component re-renders.

```js
const handleMouseMove = useCallback(
  (e) => {
    const { clientX, clientY } = e;
    setMousePos({ x: clientX, y: clientY });
  },
  [setMousePos],
);
```

### useContext

#### Creating and using context

1. Create the context

   Create a js file to create a context, and export the context from it. E.g. in
   a `LevelContext.js`:

   ```js
   import { createContext } from "react";
   export const LevelContext = createContext(1); // default value is 1
   ```

2. Use the context

   Use `useContext` hook from react to use the context in your component. E.g.
   in `Heading.jsx`:

   ```js
   import { useContext } from "react";
   import { LevelContext } from "@/lib/LevelContext.js";

   export default function Heading({ children }) {
     const level = useContext(LevelContext);
     // ...
   }
   ```

   You no longer need to pass a prop `level` to the `Heading` component.
   Currently, since no context is provided by any ancestor component up the
   tree, React will use the **default** level, i.e. 1.

3. Provide the context

   Provide the context with an ancestor component. In the ancestor component,
   wrap the children with a **context provider**. E.g. in `Section.jsx`:

   ```js
   import { useContext } from "react";
   import { LevelContext } from "@/lib/LevelContext.js";

   export default function Section({ children }) {
     const level = useContext(LevelContext);
     return (
       <section className="section">
         <LevelContext.Provider value={level + 1}>
           {children}
         </LevelContext.Provider>
       </section>
     );
   }
   ```

   Any component using `LevelContext` will use the value of the **nearest**
   `<LevelContext.Provider>` in the tree above it.

#### Context

- The value can be any object.

### useRef

- To store information between re-renders (unlike regular variables, which reset
  on every render)
- Changing it doesn't trigger a re-render (unlike state variables)
- The information is local to each copy of your component (unlike variables
  outside)

`useRef` can be used as a replacement to `querySelector` to get a reference to
an element for manipulation.

```js
import { useRef } from "react";

function MyComponent() {
  const inputRef = useRef(null);
  return <input ref={inputRef} />;
}

function handleClick() {
  inputRef.current.focus();
}
```

### Custom hooks

Define a function whose identifier starts with `use`.

## 📦 React Packages

- `clsx`: convenient to add classes conditionally
- [`react-redux`](redux.md): manage and access states easily
- [`react-router-dom`](react-router-dom.md): use routing to simulate multi-page
  websites
- [`react-icons`](https://react-icons.github.io/react-icons/): can work with
  tailwind
- [`dnd-kit`](https://docs.dndkit.com) provides `@dnd-kit/core` to build custom
  drag and drop components, and an optional `@dnd-kit/sortable` package to
  provide presets to simplify building drag-and-drop sortable lists/tables.

## 🌐 Resources

- [HTML to JSX converter](https://transform.tools/html-to-jsx)

## 🧭 Navigation

- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)

## Footnote

[^1]:
    See
    [React: Picking a key](https://react.dev/learn/tutorial-tic-tac-toe#picking-a-key)
