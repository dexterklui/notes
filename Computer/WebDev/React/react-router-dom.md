---
date: 2023-09-02 (Sat)
---

# React Router Dom

## Installation

`npm install react-router-dom`

## Basic Usage

In `index.js`:

```js
import { BrowserRouter } from "react-router-dom";

// ...

root.render = (
  // Other enclosing tags
  <BrowserRouter>
    <App />
  </BrowserRouter>
  // ...
);
```

In `App.js`:

```js
import { Link, Route, Routes } from "react-router-dom";
// Other imports

function App() {
  const isAuthenticated = useSelector(
    (store) => store.authStore.isAuthenticated,
  );
  return (
    <div className="app">
      <nav>
        <Link to="/">Home</Link>
        <Link to="/my-profile">Profile</Link>
        {isAuthenticated ? <></> : <Link to="/login">Login</Link>}
      </nav>
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route
          path="/my-profile"
          element={
            <RequiredAuth>
              <MyProfilePage />
            </RequiredAuth>
          }
        />
        <Route path="/login" element={<LoginPage />} />
      </Routes>
      {isAuthenticated ? <LogoutButton /> : <></>}
    </div>
  );
}
```

Actually, you can leave `index.js` unchanged and use `<BrowserRouter>` in
`App.js` instead.

## Pages

Each page is just a JSX. It is conventional to put all js files for pages into a
folder, e.g. `src/pages` or `src/screens`.

To include a page in `App.js`, add a `<Route>` tag in `<Routes>` tag:

```js
<Routes>
  <Route path="/" element={<HomePage />} />
  <Route
    path="/my-profile"
    element={
      <RequiredAuth>
        <MyProfilePage />
      </RequiredAuth>
    }
  />
  <Route path="/login" element={<LoginPage />} />
</Routes>
```

- React will replace `<Routes>` tag by the target page JSX in the final product.
- Each `<Route>` is a single page with a given path. When browser user input a
  path, React will search for the first page with matching path to render.
  - Property `path` specifies the path of the page
  - Property `element` specifies the JSX to render for this path.

## Components

### Links

`<Link to="/">Home</Link>` gives you an HTML anchor to the target path.

### Navigate

When React encounters a `<Navigate to="/" />` while rendering, React will
redirect the browser to the target path instead and render it.

If React encounters multiple `<Navigate>` tags, then react will keep redirecting
one after another as it encounters those tags.

Or you can use the `useNavigate` hook from `react-router-dom`:

```javascript
const navigate = useNavigate();
// ...
navigate("/");
```

## Hooks

### useLocation

```js
const location = useLocation();
```

Will give you info about current path and route: `location.pathname` contains
the current path.

You can also access states passed to the current page if current page is
redirected by `<Navigate>`

```js
// in original page return:
<Navigate to="/redirected-page" state={{ key1: "a string", key2: myVar }} />;
// then access the state in redirected-page:
const state = useLocation().state;
```

### useParams

The hook `useParams` is used to access the parameters passed to the current page

## Catch All Route

You can handle unfound path handling. E.g. create a `Error.jsx` component, and
use it in catch-all `<Route>`:

```javascript
<Route path="*" element={<Error />} />
```

## 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](react.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
