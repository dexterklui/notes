---
date: 2023-10-20 (Fri)
---

# Basic SvelteKit

## Project Structure

| File / Dir          | Description          |
| ------------------- | -------------------- |
| `/package.json`     | List npm packages    |
| `/svelte.config.js` | Project config       |
| `/vite.config.js`   | Vite config          |
| `/src/`             | Contains source code |
| `/src/app.html`     | Page template        |
| `/src/routes/`      | Defines routes       |
| `/static/`          | Contains assets      |

## Routing

### Filesystem-based routing

- `+page.svelte` in `/src/routes/` is a page
- Routes follow the filesystem structure in `/src/routes/`
- You can create groups with **parentheses** `/src/routes/(auth)/login/`

### Dynamic content

Like single-page app, navigating to a different page and back updates the
contents. This can be configured.

### Layouts

`+layout.svelte` defines a layout component that applies to all routes in the
same directory. In the file, `<slot />` is where the content of the page will be
rendered.

You can nest layouts.

### Route parameters

Use **square brackets** around a valid variable name for a directory name to
specify a dynamic parameter (i.e. dynamic routing). E.g.
`/src/routes/blog/[slug]/+page.svelte`.

You can have **multiple** route parameter **within one segment**, as long as
they are separated by at least one static character. E.g.
`/src/routes/foo/[bar]x[baz]/+page.svelte`.

To access the parameters, you are using the `params` props for the `load` export
function in a `server.js` file.

```javascript
export function load({ params }) {
  const slug = params.slug;
}
```

## Loading Data

### Page data

You can export a `load` function in a `+page.server.js` file alongside the
`+page.svelte` file. This module runs on the server, including for client-side
navigations.

This `load` function should return an **object**, and this returned object can
be accessed in the page via the `data` prop:

```svelte
<script>
  export let data;
</script>

<ul>
  {#each data.summaries as { slug, title }}
    <li><a href="/blog/{slug}">{title}</a></li>
  {/each}
</ul>
```

### Layout data

`+layout.server.js` works the same as `+page.server.js`, except its scope is
similar to `+layout.svelte`, i.e. all pages under the current directory. So you
can load data for every child route.

### Server load function props

- `params`: route parameters
- `request`: HTTP request object

## Throw error

In `+page.server.js`:

```javascript
import { error } from "@sveltejs/kit";

error(404);
// throw error(404); // In SvelteKit 1.x, you need to `throw` the error yourself
```

## Forms

### Form actions

In `+page.svelte`, you define `method` and the API route with `action` property.

```svelte
<!-- Default action -->
<form method="POST">
  <!-- ... -->
</form>

<!-- Or named action "create" -->
<form method="POST" action="?/create">
  <!-- ... -->
</form>
```

You define server actions by exporting `actions` object in `+page.server.js`.
The `actions` object has a `default` action **or** any amount of **named**
actions, **but not both**. An action will receive `cookies` object and `request`
object as props.

```js
export const actions = {
  default: async ({ cookies, request }) => {
    const data = await request.formData();
    // ...
  },

  // exclusive or:

  // named action
  create: async ({ cookies, request }) => {
    // ...
  },

  // more named actions...
};
```

After the server responding to the request, the client will **refresh** the
page.

### Example: using form actions without form

```svelte
<ul class="todos">
  {#each data.todos as todo (todo.id)}
    <li>
      <form method="POST" action="?/delete">
        <input type="hidden" name="id" value={todo.id} />
        <span>{todo.description}</span>
        <button aria-label="Mark as complete" />
      </form>
    </li>
  {/each}
</ul>
```

### Validation

Built-in form validation can be useful as a first line defence.

- `required`
- `type`
- `minLength` and `maxLength`

But this doesn't remove the needs to do server-side validation.

### Error handling and custom status code

When not in dev mode, when error occurs on server side, SvelteKit hides the
error message in client side to prevent the lead of sensitive information. To
set a custom status code with custom response body, use `fail` function from
`@sveltejs/kit`:

```js
export const actions = {
  create: async ({ cookies, request }) => {
    const data = await request.formData();

    try {
      db.createTodo(cookies.get("userid"), data.get("description"));
    } catch (error) {
      return fail(422, {
        description: data.get("description"),
        error: error.message,
      });
    }
  },
};
```

Then in `+page.svelte`, you can access the return value of `fail` via the `form`
prop, which is only ever populated after a form submission.

```svelte
<script>
  export let form;
</script>

{#if form?.error}
  <p class="error">{form.error}</p>
{/if}

<form>
  <!-- keep the value when error occurs to let user fix it easily -->
  <input value={form?.description ?? ""} />
</form>
```

### Form prop from form actions

As said in
[Error handling and custom status code](#error-handling-and-custom-status-code),
`form` prop is populated with the return value of `fail` function after form
submission. But it also is populated with any return value from **form
actions**. So you can use the `form` prop to access that.

### Progressive enhancement

`import { enhance } from "$app/forms"` and use the enhance directive,
`<form use:enhance>`, to make the form enhance the behaviour and features with
client-side JavaScript when possible. See
[Svelte tutorial](https://learn.svelte.dev/tutorial/progressive-enhancement).

## Server-side Only Code

In the following, replace the last extension to `.ts` for TypeScript.

- `+page.server.js`
- `+layout.server.js`
- Any other modules ending with `.server.js`
- Any modules inside `src/lib/server/`

These modules only run on the server, and can be imported only by other server
modules.

## Environment Variables

### Modules that expose environment variables

- `$env/dynamic/private`
- `$env/dynamic/public`
- `$env/static/private`
- `$env/static/public`

**Private** variables are only exposed to the server. **Client** variables also
are exposed to the client. **Static** variables are known at build time and can
be **statically replaced** at build time. **Dynamic** variables are read at
runtime.

### Defining environment variables

You define them in `.env`, `.env.local`, `.env.<mode>`, or `.env.[mode].local`
files. See
[Vite documentation](https://vitejs.dev/guide/env-and-mode.html#env-files).

By default, environment variables are only available in server-side code. To
make them public, prefix the variable name with `PUBLIC_`.

### Importing environment variables

For static variables, you can import them directly:

```javascript
import { MY_API_KEY } from "$env/static/private";
```

For dynamic variables, you need to access through `env` object:

```javascript
import { env } from "$env/dynamic/private";
const { MY_VAR } = env;
```

## Page Metadata

```svelte
<svelte:head>
  <title>Todo List</title>
</svelte:head>
```

## API Routes

See this [SvelteKit Tutorial](https://kit.svelte.dev/docs/routing#server).

Defined with a `+server.js` file, usually in `/src/routes/api/`.

A `+server.js` file exports functions corresponding to HTTP verbs like `GET`,
`POST`, `PATCH`, `PUT`, `DELETE`, `OPTIONS`, and `HEAD` that take a
`RequestEvent` argument and return a `Response` object.

```ts
import { error } from "@sveltejs/kit";

export const GET({ url }) {
  const min = Number(url.searchParams.get("min") ?? "0");
  const max = Number(url.searchParams.get("max") ?? "1");
  const d = max - min;

  if (isNaN(d) || d < 0) {
    error(400, "min and max must be numbers, and min must be less than max");
  }

  const random = min + Math.random() * d;

  return new Response(String(random));
}
```

## 🔗 References

- [SvelteKit Tutorial - Basics](https://learn.svelte.dev/tutorial/introducing-sveltekit)

## 🧭 Navigation

- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
