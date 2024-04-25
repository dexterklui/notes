---
date: 2023-10-18 (Wed)
---

# Basics

## Files

A component is defined in a file with the extension `.svelte`.

## Basic structure

```svelte
<script>
  let name = "world";
  let src = "/image.gif"
</script>

<h1>"Hello {name}"</h1>
<img {src} alt="A man dances" />

<style>
  h1 {
    color: smokewhite;
  }
</style>
```

## Shorthand attribute

You can omit the attribute name if the name and the identifier of the value are
the same: `src={src}` can be written as `{src}`.

## Styling

Write CSS in `<style>`. The rules are **scoped** to the component.

- `:global(...)` to apply a CSS selector global

## Importing components

Svelte automatically does default export for components defined in a `.svelte`
file. You can `import` them in `<script>` and then use them like React.

In `Nested.svelte`:

```svelte
<p>Another paragraph</p>
```

In `App.svelte`:

```svelte
<script>
  import Nested from "./Nested.svelte";
</script>

<p>This is a paragraph</p>
<Nested />
```

## Interpret HTML tags

Use `{@html ...}` to interpret HTML tags in a string. But note that Svelte
**doesn't** perform any **sanitization**, so be aware of XSS attacks when
serving untrusted content.

```svelte
<script>
  let string = `this string contains some <strong>HTML!!!</string>`;
</script>

<p>{@html string.length ? string : "<small>Nothing here</small>"}</p>
```

## Reactivity

### Assignments

Reactivity is updating DOM as the states change. In Svelte, reactivity is
triggered by assignments. Any components that reference the assigned variables
will be re-rendered.

```svelte
<script>
  let count = 0;

  function increment() {
    ++count;
  }
</script>

<button on:click={increment}>
  Clicked {count} {count === 1 ? "time" : "times"}
</button>
```

### Declarations

Sometimes certain values are derived from other reactive values, and need to be
updated when their dependency changes. Use `$: ...` to declare a reactive value
that depends on other reactive values.

```svelte
<script>
  let count = 0;
  $: doubled = count * 2;
  // Since "doubled" is undeclared, svelte will inject `let` declaration
</script>
```

Reactive declarations and statements will run **after** other script code.

### Statements

You can leverage reactive declarations to run statements when certain reactive
values change.

```svelte
<script>
  $: {
    console.log(`the count is ${count}`);
    console.log(`this will also be logged whenever count changes`);
  }

  $: if (count >= 10) {
    alert("Count is dangerously high!");
    count = 0;
  }
</script>
```

### Updating arrays and objects

Methods like `push()` for arrays aren't assignment and by default there won't be
any updates. To fix this:

- Use redundant assignment: `numbers = numbers`
- For `push()` case, use `numbers = [...numbers, numbers.length + 1]`
- `numbers[numbers.length] = numbers.length + 1`

Rule of thumb: name of the updated variable must appear on the left hand side of
the assignment. E.g. the following won't work:

```svelte
<script>
  const foo = obj.foo;
  foo.bar = "bax";
</script>
```

## Props

### Declaring props

In `Nested.svelte`:

```svelte
<script>
  export let answer = 0; // default value
</script>

<p>The answer is {answer}</p>
```

In `App.svelte`:

```svelte
<script>
  import Nested from "./Nested.svelte";
</script>

<Nested answer={42} />
```

### Spread props

```svelte
<script>
  const pkg = {
    name: "svelte",
    speed: "blazing",
    version: 4,
    website: "https://svelte.dev",
  };
</script>

<PackageInfo {...pkg} />
```

### Referencing all props

Alternatively, if you need to reference all the props that were passed into a
component, including ones that weren't declared with `export`, you can do so by
accessing `$$props` directly. It's not generally recommended, as it's difficult
for Svelte to optimise but it's useful in rare cases.

## Logic

### Logic blocks

Augment HTML to implement complex rendering logic, with traditional if-else and
for-each blocks.

Control statements of blocks are wrapped in curly braces `{}`. Opening control
statement (or _block opening_ tag) starts with `#`, and _closing_ tag starts
with `/`, while _continuation_ tag starts with `:`.

### If-else blocks

Just use `{#if ...}` and `{/if}`

```svelte
{#if count > 10}
  <p>{count} is greater than 10</p>
{:else if count > 5}
  <p>{count} is between 5 and 10</p>
{:else}
  <p>{count} is less than or equal to 5</p>
{/if}
```

### Each blocks

```svelte
{#each colors as color, idx}
  <button
    aria-current={selected === color}
    aria-label={color}
    style="background: {color}"
    on:click={() => selected = color}
  >
  </button>
{/each}
```

#### Keyed each blocks

When the value of an each block is updated, Svelte won't re-render the whole
block, but add and remove items at the **end** of the block, then update any
values that have changed. That might not be what you want.

See [keyed-each-blocks](https://learn.svelte.dev/tutorial/keyed-each-blocks).
The key tells Svelte how to figure out which DOM node to change when there's
update.

```svelte
{#each things as thing (thing.id)}
  <Thing name={thing.name} />
{/each}
```

You can use **any object** as the key. But generally primitive type is safer
because identity persists even when referential equality breaks after updating
with fresh data from API.

### Await blocks

```svelte
<script>
	import { getRandomNumber } from './utils.js';

	let promise = getRandomNumber();

	function handleClick() {
		promise = getRandomNumber();
	}
</script>

<button on:click={handleClick}>
  generate random number
</button>

{#await promise}
  <p>...waiting</p>
{:then number}
  <p>The number is {number}</p>
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}
```

Only the most recent `promise` is considered, so no need to worry about race
condition.

You can skip first block and catch block.

```svelte
{#await promise then number}
  <p>The number is {number}</p>
</await>
```

## Events

### Event directive

Use `on:` directive in attribute to listen to any DOM event on an element.

You can pass in a reference to a handler, or define it in place. Both are the
same performance-wise.

### Event modifiers

```svelte
<button on:click|once={() => alert("clicked")}>
  Click Me
</button>
```

Full list of modifiers:

- `preventDefault`
- `stopPropagation`
- `passive`: improves scrolling performance on touch/wheel events (Svelte will
  add it automatically where it's safe to do so)
- `nonpassive`: explicitly set `passive: false`
- `capture`: fires during capture phase
- `once`: remove handler after the first time it runs
- `self`: only trigger if `event.target` is the element itself
- `trusted`: only trigger if `event.isTrusted` is `true`.

You can **chain** modifiers together, e.g. `on:click|once|capture={...}`.

### Component events

In `Inner.svelte`, you must call `createEventDispatcher()` to create a
dispatcher when the component is first instantiated.

```svelte
<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function sayHello() {
    dispatch("message", { // dispatch a custom event called "message"
      text: "Hello!", // the payload of the event
    });
  }
</script>

<button on:click={sayHello}>
  Click to say hello
</button>
```

In `App.svelte`:

```svelte
<script>
  import Inner from "./Inner.svelte";
</script>

<Inner on:message={e => alert(e.detail.text)} />
```

### Event forwarding

Component events **don't bubble**. You must **forward** event manually. To
forward an event, simply write the `on:` directive without the need to pass a
forwarding handler. You can forward DOM events too.

#### Forwarding component event

```svelte
<script>
  import Inner from "./Inner.svelte";
</script>

<Inner on:message />
```

#### Forwarding DOM event

`BigRedButton.svelte`:

```svelte
<button on:click></button>
<style>
  /* ... */
</style>
```

In `App.svelte`:

```svelte
<script>
  import BigRedButton from "./BigRedButton.svelte";
  import horn from "./horn.mp3";

  const audio = new Audio();
  audio.src = horn;
</script>

<BigRedButton on:click={() => audio.play()} />
```

### This in event handlers

In this [tutorial](https://learn.svelte.dev/tutorial/tick), it seems you can use
`this` in a event handler to refer to the element that the handler is attached
to. This can easily access the element's properties and methods.

## Bindings

### Binding attribute to variable

Binding connects a variable with an attribute of an element.

```svelte
<input bind:value={name} />
<!-- Same as : -->
<input value={name} on:input={e => name = e.target.value} />
```

### Automatic type conversion

Binding automatically handles different types of input elements.

```svelte
<input type="number" bind:value={a} min="0" max="10" />
<p>3 * {a} = {3 * a}</p>
<input type="checkbox" bind:checked={isHappy} />
```

### Multiple radio and checkboxes

Using `bind:group={myGroup}` along side with `value={myValue}` attrbute binds a
group of elements to the same group: changing the same state together.

If it's a group of **radio** buttons, their value are mutually exclusive. If
they are checkboxes, they form an array of selected values.

```svelte
{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
	<label>
		<input
			type="checkbox"
			name="flavours"
			value={flavour}
			bind:group={flavours}
		/>

		{flavour}
	</label>
{/each}
```

### Shorthand form

In case the name of the variable and the attribute matches, you can use
shorthand form: `bind:value` is the same as `bind:value={value}`.

### Bind this

You can get a reference to an element by using `bind:this`. This achieves
something like `useRef` in React.

```svelte
<script>
  let elem;
</script>

<div bind:this={elem}>
</div>
```

## Lifecycle

### onMount

You pass a function to `onMount()` and it's body will run when the component is
mounted. The function can return a cleanup function that will run when the
component is unmounted.

```svelte
<script>
  import { onMount } from "svelte";
  import { paint } from "./gradient.js";

  onMount(() => {
    const canvas = document.querySelector("canvas");
    const ctx = canvas.getContext("2d");

    let frame = requestAnimationFrame(function loop(t) {
      frame = requestAnimationFrame(loop);
      paint(ctx, t);
    })

    return () => {
      cancelAnimationFrame(frame);
    }
  })
</script>

<canvas width={32} height={32} />
```

### onDestroy

`onDestroy` is similar to `onMount`, but runs just before the component is
umnounted.

NOTE that this has nothing to do with the DOM. Being destroyed is not the same
as being removed from the DOM. It only means Svelte is about to stop managing
the component.

### beforeUpdate and afterUpdate

Callbacks passed to them fires before DOM is updated and after DOM is in sync
with the state, respectively.

### Tick

`tick()` returns a promise that resolves after the next DOM update. It's useful
when you want to wait for DOM to be in sync with state changes before doing
something.

In the handler, you can use `await tick()` to wait until after the next DOM
update.

## Stores

### What's a store

Solves the problem of accessing a state from multiple components. A store is
simply an object with a `subscribe` method that allows interested parties to be
notified whenever the store value changes. Writable stores additionally have
`update` and `set` methods for changing the value.

### Creating stores

#### Readable store

When it doesn't make sense to update a store value from outside. Use a
`readable` store.

`readable` function returns a readable store. It can takes two arguments:

1.  Initial value
2.  `start` function that takes a `set` callback and returns a `stop` function:
    - `start` function is called when the store gets its first subscriber.
    - `stop` is called when the last subscriber unsubscribes.

```javascript
import { readable } from "svelte/store";

export const time = readable(new Date(), function start(set) {
  const interval = setInterval(() => {
    set(new Date());
  }, 1000);

  return function stop() {
    clearInterval(interval);
  };
});
```

#### Writable store

Creating a `writable` store is similar to creating a readable store. A writable
store additionally provides `set` and `update` methods for changing the value.

#### Derived store

A `derived` store is a readable store whose value is **derived** from one or
more other stores.

```javascript
export const elapsed = derived(time, ($time) =>
  Math.round(($time - start) / 1000),
);
```

See this [tutorial](https://learn.svelte.dev/tutorial/derived-stores) for more
info, and for links to documentation describing asynchronous derived stores
using `set` explicitly.

#### Custom store

As long as an object correctly implements the `subscribe` method, it's a store.
Beyond that, anything goes. You can implement you own methods and avoid exposing
`set` and `update` methods.

```javascript
function createCount() {
  const { subscribe, set, update } = writable(0);

  return {
    subscribe,
    increment: () => update((n) => n + 1),
    decrement: () => update((n) => n - 1),
    reset: () => set(0),
  };
}
```

### Subscribing a store

`subscribe` method accepts a callback, and when the store value changes, it
calls the callback with the new value. It **returns** a function that can be
used to unsubscribe. This is important to prevent memory leak when the component
gets destroyed and created again.

```svelte
<script>
  import { count } from "./stores.js";
  import { onDestroy } from "svelte";

  let countValue;
  const unsubscribe = count.subscribe((value) => {
    countValue = value;
  })

  onDestroy(unsubscribe);
</script>

<h1>The count is {countValue}</h1>
```

#### Autosubscription

Autosubscription is a succinct way to subscribe, unsubscribe and use a store
value. It is by referencing the store variable with a `$` prefix.

Note that it only works with store variables that are declared or imported at
the top-level scope of a component.

Also any name with `$` prefix is **reserved** for autosubscription to refer to a
store value.

```svelte
<script>
  import { count } from "./stores.js";

  // You can reference $count directly in script
  if ($count > 10) {
    // ...
  }
</script>

<h1>The count is {$count}</h1>
```

### Setting value of a writable store

`set` method accepts a new value and updates the store value.

```svelte
<script>
  import { count } from "./stores.js";

  function handleReset() {
    count.set(0);
  }
</script>
```

### Updating value of a writable store

`update` method accepts a callback that takes the current value and returns the
new value.

```svelte
<script>
  import { count } from "./stores.js";

  function handleIncrement() {
    count.update((n) => n + 1);
  }
</script>
```

### Getting value of a store

```javascript
import { get } from "svelte/store";
import { myStore } from "$lib/stores/my-store.js";

const storeValue = get(myStore);
```

### Store bindings

If a store is writable, i.e. it has a `set` method - you can
[**bind**](#bindings) to its value.

And `$name += "!"` is equivalent to `name.set($name + "!")`.

## 🔗 References

- [Svelte Tutorial - Basics](https://learn.svelte.dev/tutorial/welcome-to-svelte)

## 🧭 Navigation

- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
