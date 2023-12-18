---
title: Next.Js
date: 2023-09-14 (Thu)
---

# Layout

Layout applies as default to every pages and subpages of its path.

The `children` is the sub-layout and page component to be loaded.

```js
export default function DashboardLayout({ children }) {
  return (
    <>
      <Navbar />
      {children}
    </>
  );
}
```

# Routing

## Routing paths

By default, routing paths mirror the **file structure** inside `app` directory.
Each `page.jsx` inside a subdirectory of `app` is a specific path. I.e.:

- `page.jsx` inside `app` is the page for path `/`
- `page.jsx` inside `app/dashboard` is the page for path `/dashboard`
- `page.jsx` inside `app/dashboard/projects` is the page for path
  `/dashboard/projects`

### Route groups

Use parentheses, e.g. `(auth)`, to name directories to group routes together.

- `app/(auth)/login/page.jsx` corresponds to the route `/login`
- `app/(auth)/signup/page.jsx` corresponds to the route `/signup`
- `app/(dashboard)/page.jsx` corresponds to the route `/`
  - You may want to put your homepage here, if you feel like it should be a
    dashboard.

Be ware of the route conflicts, e.g. in this case conflict if any of the
following file exists, respectively:

- `app/login/page.jsx`
- `app/signup/page.jsx`
- `app/page.jsx`

`layout.jsx` can be put into route groups to apply to all subdirectories.

## Dynamic segment

Enclose the name of a directory with square bracket `[]` for a **dynamic**
routing segment.

Say you name the directory as `[id]`, then you can get the dynamic routing path
from:

```js
export default async Ticket({ params }) {
  const id = params.id; // get the dynamic routing path
  // ...
}
```

## Nested dynamic segment

A directory named `[...catch-all]`, where `catch-all` can be any string, is like
a dynamic segment but can catch nested level of route.

Then the nested routes will be passed as a list to the `params`.

For example, for a route `/some/random/not/existing/route`, the page of
`app/[...catch-all]/page.jsx` with the component:

```js
export default CatchAll({ params }) {
  console.log(params)
  // {'catch-all': ['some', 'random', 'not', 'existing', route]}
}
```

## Static vs Dynamic Rendering

**_Static rendering_** pre-renders pages during build phase (not in dev
environment). **_Dynamic rendering_** renders pages on request.

### Generating static page at build phase

You need to export a function called `generateStaticParams()` that returns an
array of objects, each has one key-value pair where:

- Name of the key, which is the same for all objects in the array, is the
  identifier of the dynamic routing segment (key `id` for directory `[id]`)
- Value is the specific path for the dynamic routing segment

```js
export async function generateStaticParams() {
  const res = await fetch("http://localhost:4000/tickets");
  const tickets = await res.json();
  return tickets.map((ticket) => ({
    id: ticket.id,
  }));
}
```

See
[documentation](https://nextjs.org/docs/app/api-reference/functions/generate-static-params).

### Handling not pre-rendered page

If you pre-render static page for a dynamic routing section, there are specific
paths that aren't pre-render. You can tweak how Next.Js handle this situation.

Do `export const dynamicParams = false` to display a **404** page. Otherwise,
default is `true` and Next.Js will try to dynamically render the page and then
add the rendered page as a static page in the server.

When `dyanmicParams = true`, you may want to manually display **404** page when
fetch is unsuccessful. You can use the function `notFound()` from
`"next/navigation"` inside a _server component_ to set the status code to 404.

## Custom 404 page

Make a file `not-found.jsx` that returns a component called `NotFound`. It
becomes the 404 page for the current route segment, and sub-segments until being
overridden by other `not-found.jsx`.

## Using Router

```js
import { useRouter } from "next/navigation";
// ... Then inside a component...
const router = useRouter();

router.refresh(); // This tells the router to refresh current route, so will
// fetch updated data, also refresh the page pushed by the router after this
router.push("/"); // This function redirects current page to path "/"
router.replace("/"); // Similar to push(), but replace most recent path history
```

## Server-side refresh

On the server side, you can `revalidatePath(path)` to refresh a route of a given
path.

# Client vs Server-side Rendering

You need to decide which should be **_client components_** vs **_server
components_**. Both client and server components are rendered on the server, but
only client components require additional JS sent to the browser to be
**_hydrated_** (to have **interactivity**).

By **default**, all components are server components. See
[Server and Client Composition Patterns](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns)
to see when to use client vs server components.

## Making a client component

In the jsx file, add the line `"use client"` as the first line.

## Disable server-side rendering

```js
import dynamic from "next/dynamic";

const SimpleBarChartWithoutSsr = dynamic(
  import("../components/rechartsCharts/SimpleBar"),
  { ssr: false },
);
```

SSR means server-side rendering.

## Incremental server-side rendering

**_Incremental server-side render_** (**_ISR_**) means the server re-renders a
static pages periodically at a time interval.

## Server actions

### What's a server action?

A **_server action_** is a function that runs only on the server. It should have
serializable arguments and serializable return value.

### Defining a server action

To tell Next.js that a function is a server action, you can either:

- Define the function in a server component, and then add the line
  `"use server"` at the top of the body.
- Define the function in a file `action.js`. Then add the line `"use server"` at
  the top. Now every export in this file is a server action.

### Using a server action

You can import a server action, or pass it as a prop to a component. Then to
invoke it, you can:

- Use `action` props of a `<form>` element.
- Use `formAction` props on `<button>`, `<input type="submit">` and
  `<input type="image">` elements in a `<form>`.

### Form action

Server action is useful for form submission. In a client form component, you can
pass a server action to its `action` attribute. On submit, the form data will be
passed to the server action as the argument `formData`, with the type
[`FormData`](https://developer.mozilla.org/en-US/docs/Web/API/FormData), and the
function will run on the server.

# Static vs Dynamic Rendering

```js
export const dynamic = "auto"; // | "force-dynamic" | "error" | "force-static"
```

In `"auto"`, Next.js opts in dynamic rendering when there are dynamic functions:

- headers
- cookies
- searchParams (query string in URL)
  - When a client component uses `useSearchParams()`, the whole section enclosed
    by the closest `<Suspense>` boundary containing this component will render
    dynamically. So it is a good idea to wrap this kind of client components in
    `<Suspense>`.

# Async Server Components for Fetching

## Usage

Just use `async` for the functional component

```js
async function getTickets() {
  const res = await fetch("http://localhost:4000/tickets");
  return res.json();
}

export default async function TicketList() {
  const tickets = await getTickets();
  return; // map tickets to components
}
```

## Fetch and cache behaviour

See
[Caching in Next.js](https://nextjs.org/docs/app/building-your-application/caching).
Really a good read, with diagrams.

By default, _Next.Js_ would have following behaviour:

- **Caches** the response so a new request to the same page (e.g. leaving the
  page and coming back) won't require a new fetch.
- **Memoize** fetched data in one render pass, so only fetch **once** from the
  same source even if the same code appears multiple times in different places.
  - This is a _React_ feature.
  - This happens no matter data is cached or not.

### Tweaking cache

To opt-out cached response completely, pass `{cache: "no-store"}` to `fetch()`
as a second argument.

To tell _Next.Js_ to revalidate cached response after a period of time, pass
`{next: {revalidate: 30}}` instead.

```js
fetch(`https://...`, {
  next: {
    revalidate: 30,
    // Revalidate data again after 30 second since last fetch
  },
});
```

For `revalidate: 0`, then Next.Js will always re-fetch data.

## Fetching local files from server side

Note using `fetch()` to fetch local data in the server from the server side
requires an absolute path.

E.g. to fetch the local file `/public/data.csv`.

- `fetch("/data.csv")` throws an error about can't resolve URL
- `fetch("http://localhost:3000/data.csv")` works (replace your own port number)

# Streaming and Suspense

- Stream content to the browser using Suspense boundaries
- Load rest of the page where it is ready first, and show loading status only in
  subcomponents that waiting for the fetched data.

## Making a loading placeholder component

Make a file `loading.jsx` that returns a component called `Loading`. The
component is automatically used as a placeholder for **pages** that hasn't be
rendered (e.g. one of the sub-components is waiting for fetch to complete).

## Specifying boundary for loading placeholder

If you don't want the whole page to be replaced by `<Loading />` placeholder,
you can specify a custom boundary by wrapping the part to be replaced with
`<Suspense>`.

By default, `<Suspense>` doesn't use anything as the placeholder, i.e. it would
just be blank while loading. But you can specify a placeholder with `fallback`
attribute.

```js
<Suspense fallback={<Loading />}>
  <Page />
</Suspense>
```

# Error page

Make a file `error.jsx` that export a default **client** component. The
component is rendered as the error page when error occurs. The component
receives two props, `error` and `reset` from _Next.js_.

`error` props is the object thrown in the error, whereas `reset` is a function
that resets the error and gets back to the previous page.

# Page metadata

## Defining static metadata

In `layout.jsx` or `page.jsx`

```js
export const metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};
```

`metadata` exported in descendent layouts and pages is merged with and
**update** the parent metadata.

## Defining dynamic metadata

```js
// Use async function if you need to fetch
export function generateMetadata({ params }) {
  // params is same as the params prop for the component

  return {
    title: `${params.title}`,
  };
}
```

# Font

```js
import { Rubik } from "next/font/google";

const rubik = Rubik({ subsets: ["latin"] });
// ...
<body className={rubik.className}>
```

# Route Handlers

## API Routes

**_Route Handlers_** is for building API endpoints. The endpoints is defined by
a file `route.js`. It's route is defined by the file path under `app/`
directory, like a `page.js`. So you should prevent _route conflicts_ with a
page.

## Example

### GET handler

```typescript
export async function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const dbName = searchParams.get("dbName");
  const collectionName = searchParams.get("collectionName");
  const pipeline = searchParams.get("pipeline");

  if (!dbName || !collectionName || !pipeline)
    return NextResponse.json(
      { error: "Invalid dbName or collectionName" },
      { status: 400 },
    );

  try {
    const mongoClient = await clientPromise;
    const data = await mongoClient
      .db(dbName!)
      .collection(collectionName!)
      .aggregate(JSON.parse(pipeline))
      .toArray();

    return NextResponse.json({ data });
  } catch (error) {
    if (error instanceof Error)
      return NextResponse.json({ error: error.message }, { status: 500 });
    throw error;
  }
}
```

### POST handler

```typescript
export async function POST(request) {
  const data = await request.json();
}
```

## Static vs dynamic route handlers

By default, **_GET_** route handlers are static. That means the response of the
route is fixed at compile time. To make a route handler dynamic, either you
provide the init object `{ next: { revalidate: 0 } }` to the `fetch()` function
in your route to make that fetch dynamic. Or you can add the following line to
`route.js`, making all route handlers defined in this file dynamic.

```typescript
export const dynamic = "force-dynamic";
```

## Dynamic routes for route handlers

```typescript
export async function GET(_, { params }) {
  const id = params.id;
}
```

Since the first argument is a `NextRequest` object, we use `_` to ignore it.

# Builtin Components

## Image

`<Image>` has certain properties, here are some useful ones:

| Attribute           | Description                                               |
| ------------------- | --------------------------------------------------------- |
| `src`               | `src` for `<img>`                                         |
| `alt`               | `alt` for `<img>`                                         |
| `height={50}`       | Set height to 50px                                        |
| `width={70}`        | Set width to 70px                                         |
| `placeholder`       | Placeholder while image is loading: `'empty'` or `'blur'` |
| `onLoadingComplete` | Callback when image is loaded                             |

## Link

`<Link>` acts like `<a>`. Attribute `href` specifies the URL.

# Building Application

Run `npm run build`.

You will see a list of routes / pages with a little leading icon:

- A empty circle `○` means a static page without any initial props.
- A filled circle `●` means a static page with initial props.
- A lambda `λ` means a dynamically rendered page.

# Tailwind CSS

You can create more CSS files, without the boilerplate with `@tailwind`
directive, and import the CSS files into component files.

In the CSS files, you should mostly use `@apply`, to apply styling defined by
Tailwind CSS classes.

For `d3.css`:

```css
.d3-tick line {
  @apply stroke-sky-200;
}

.d3-tick text {
  @apply fill-sky-600;
}
```

Then for `D3Plot.jsx`:

```js
import "./d3.css";
```

# Debug

## Console log

Be aware of where `console.log()` is done, server or client side.

To log at the client side, for better logging output in browser, I tried to pass
the things to be logged to a client component and render it.

```js
"use client";

export default function ConsoleLog({ logArgs }) {
  console.log(logArgs);
  return <div>See console for log output</div>;
}
```

## Errors

### Hydration failed initial UI does not match

#### Error

`Error: Hydration failed because the initial UI does not match what was rendered on the server.`

#### Problem

At server side, Next.js generates an HTML document (_pre-render_) and send it to
the client. At the client side, React hydrates the documents, and generates its
own _initial rendering_ so that it can attach event handlers. When the DOM model
of the two renders from two sides mismatch, this error occurs.

#### Scenarios

- Using browser APIs to conditionally render the component/page. The conditions
  differ between sever and client side.
- HTML syntax is not correct. E.g. `<p><div>Don't do this</div></p>`. `<p>`
  should not enclose `<div>`.
- Some browser extensions change the HTML on the client side.
- Using components from Recharts library with Next.Js. (Dunno why)

#### Solution

1.  Reconfigure Logic with `useEffect()`, which only runs on the client side and
    has access to all browser APIs.

2.  Disable SSR (server side rendering) on some components that may differ on
    the client side. Either one of the two ways:

    - Export dynamically:

      ```js
      import dynamic from "next/dynamic";
      // define your component
      export default dynamic(() => Promise.resolve(YourComponent), {
        ssr: false,
      });
      ```

    - Import dynamically:

      ```js
      import dynamic from "next/dynamic";
      const YourComponent = dynamic(() => import("./YourComponent"), {
        ssr: false,
      });
      ```

3.  In case of timestamps that always differ,suppress the warning by using
    `suppressHydrationWarning={true}` in the element

#### Link

[How to Solve Hydration Error in Next.js](https://chirag-gupta.hashnode.dev/how-to-solve-hydration-error-in-nextjs)

### Cannot pass functions from server to client component

Dunno if this works. Define the function in a separate js file, and import it
from the client component file.

# Bugs

## Uncaught ChunkLoadError

For error such as
`Uncaught ChunkLoadError: Loading chunk app-pages-internals failed.`, it's
possibly an internal error from Next.js caching. Try to remove the `.next/`
directory and refresh the page to remove the cache.

For reference, see this
[stackoverflow question](https://stackoverflow.com/questions/67652612/chunkloaderror-loading-chunk-node-modules-next-dist-client-dev-noop-js-failed).

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../../../index.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
