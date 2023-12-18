---
title: Advanced
date: 2023-10-20 (Fri)
---

# Motion

## What's a motion

**_Motion_** is a **store** but with animated transition when store value
changes. To be precise, when you call `set` or `update` to change the store
value, the actual store value undergoes a series of transition. This is useful
for making animation.

## Creating a motion

You create a motion by importing a motion creator from `"svelte/motion"`. Each
type of creator can accept two arguments:

- Initial state value
- Motion option object

```svelte
<script>
  import { spring } from "svelte/motion";
  import { cubicOut } from "svelte/easing";

  const progress = tweened(0, {
    duration: 400,
    easing: cubicOut,
  })
</script>
```

## Types of motions

- `tweened`: `delay`, `duration`, `easing`, `interpolate`, ...
- `spring`: `stiffness`, `damping`, ...

# Transitions

## Transition, in and out directives

`transition` directive applies transition effect when an element enters into and
out of the DOM.

```svelte
<script>
  import { fade } from "svelte/transition";
</script>

<p transition:fade>...</p>
```

Transition is reversible. That means when a component is added again before
completing previous transition out of the DOM, the transition will be reversed
at the current transition state.

However, if you use `in` and `out` directives separately, which applies the
into-DOM transition and out-of DOM transition, respectively, the transition will
not be reversed.

## Transition parameters

You can pass an option object with transition parameters to the transition
directive.

```svelte
<p transition:fly={{ y: 200, duration: 2000 }}>...</p>
```

## Custom CSS transitions

You define a function that takes two arguments `node` and `params`, and returns
a transition object, which can have the following properties.

```svelte
<script>
  function fade(node, { delay = 0, duration = 400 }) {
    const o = +getComputedStyle(node).opacity;

    return {
      delay,
      duration,
      css: (t) => `opacity: ${t * o}`
    };
  }
</script>
```

- `delay` — milliseconds before the transition begins
- `duration` — length of the transition in milliseconds
- `easing` — a `p => t` easing function (see the chapter on tweening)
- `css` — a `(t, u) => css` function, where `u === 1 - t`
- `tick` — a `(t, u) => {...}` function that has some effect on the node

`t` represents the linear progress of the transition from 0 to 1.

## Custom JS transitions

```javascript
function typewriter(node, { speed = 1 }) {
  const valid =
    node.childNodes.length === 1 &&
    node.childNodes[0].nodeType === Node.TEXT_NODE;

  if (!valid) {
    throw new Error(
      "This transition only works on elements with a single text node child",
    );
  }

  const text = node.textContent;
  const duration = text.length / (speed * 0.01);

  return {
    duration,
    tick: (t) => {
      const i = Math.trunc(text.length * t);
      node.textContent = text.slice(0. i);
    }
  }
}
```

## Transition events

Svelte transition provides four events:

```svelte
<p
  transition:typewriter={{ speed: 5 }}
  on:introstart={() => status = "intro started"}
  on:outrostart={() => status = "outro started"}
  on:introend={() => status = "intro ended"}
  on:outroend={() => status = "outro ended"}
>
  Type in and out
</p>
```

## Global transition

Originally, Ordinarily, transitions will only play on elements when their direct
containing block is added or destroyed. To make its transition play when the
parent is added or destroyed, we use a `global` transition:

```svelte
<div transition:slide|global>{item}</div>
```

## Key blocks

A _Key block_ is a block that destroys and recreates their contents when the
value of an expression changes.

To make a svelte transition play not only when an element enters or leaves DOM,
but also when the content changes, we can wrap the element inside a Key block.

```svelte
{#key i}
  <p in:typewriter={{ speed: 10 }}>{messages[i] || ''}</p>
{/key}
```

## Deferred transitions

Like a FLIP effect, a deferred transitions is coordinated between multiple
elements.

# Animate directive

You can apply FLIP to create transition effect for an element. Here's what I
think the requirement:

- The element is not destroyed and recreated. What I mean is it remains in the
  same parent element from start to end.
- Only applies to transform transition and perhaps color transition
- It is useful for, e.g. translating an element to a new position.

```svelte
<script>
  import { flip } from "svelte/animate";
</script>

<li
  in:receive={{ key: todo.id }}
  out:send={{ key: todo.id }}
  animate:flip={{ duration: 200 }}
></li>
```

# Actions

Actions are functions that are called when an element is created. An action's
first parameter must be a DOM node. You can optionally define a second
parameter. When using an action with the `use` directive, you can optionally
pass the parameter to the action.

## Use directive

```svelte
<script>
  function fadeIn(node, options) {
    const defaultOptions = { duration: 300, delay: 100 };
    options = {...defaultOptions, options};
    node.animate(() => node.animate([{ opacity: 0 }, { opacity: 1 }], options));
  }
</script>

<img src="./ghost.jpg" alt="ghost fading in" use:fadeIn={{ delay: 500 }} />
```

## ActionReturn

They can optionally return an object with two optional methods:

- `destroy`: called when the element is unmounted
- `update`: it can have a parameter. Called when that parameter changes,
  immediately after Svelte has applied updates to the markup
  - Note `ActionReturn` and `ActionReturn<undefined>` both mean that the action
    accepts no parameters.

# Classes and Styles

## Class directive

```svelte
<button
  class="card {flipped ? 'flipped' : ''}"
  on:click={() => flipped = !flipped}
></button>

<button
  class="card"
  class:flipped={flipped}
  on:click={() => flipped = !flipped}
></button>
```

## Shorthand class directive

When the name of the class is the same as the name of the value it depends on:

```svelte
<button
  class="card"
  class:flipped
  on:click={() => flipped = !flipped}
></button>
```

## Style directive

```svelte
<button
  class="card"
  style="transform: {flipped ? 'rotateY(180deg)' : ''}; --bg-1='goldenrod'"
  on:click={() => flipped = !flipped}
></button>

<button
  class="card"
  style:transform={flipped ? "rotateY(0)" : ""}
  style:--bg-1="goldenrod"
  on:click={() => flipped = !flipped}
></button>
```

## Global CSS

In `<style>`, you can use `:global` CSS modifier to indiscriminately target
elements inside other components. See
[tutorial](https://learn.svelte.dev/tutorial/component-styles). But you should
avoid this approach whenever possible, for it breaks the encapsulation of your
components.

## Changing component style from outside

Other than using [Global CSS](#global-css), the best way to allow parent to
control the style of children is to use CSS variables.

In `Box.svelte`:

```svelte
<style>
  .box {
    background-color: var(--color, #ddd);
  }
</style>
```

In `App.svelte`, you can set the `--color` using
[style directive](#style-directive) on any parent element to affect the same css
variable in the children:

```svelte
<div class="boxes" --color="yellow">
  <Box />
  <Box />
  <Box />
</div>
```

Or you can also set the `--color` for each individual child component in the
following way:

```svelte
<div class="boxes">
  <Box --color="red" />
  <Box --color="green" />
  <Box --color="blue" />
</div>
```

# Context API

## Reactivity of context

Context is not inherently reactive. If you need reactive values in context then
you can pass a store into context, which will be reactive.

## Import context API

Module `svelte` provides `getContext()` and `setContext()`.

## Set context

```svelte
<script>
  import { setContext } from "svelte";

  setContext("myContext", {
    userName: "myName",
    age: 18,
    greet() {
      console.log(`Hello, ${this.userName}!`);
    },
  })
</script>
```

## Get context

```svelte
<script>
  import { getContext } from "svelte";

  const myContext = getContext("myContext");
  const userName = myContext.userName;
  myContext.greet();
</script>
```

# Module Context

In a svelte component file, code inside `<script context="module">` will only
execute once when the module first evaluates, rather than when a component is
instantiated. This is the **_module context_**.

Variables declared in module context are shared among all instances of the
components. Exports in module context can be imported by other files and called
or updated from anywhere, and the changes will be reflected to all instances of
that module.

```svelte
<script context="module">
  let current;
</script>
```

# Search Params

See [doc](dexter-d7njuvoix-dexterkluis-projects.vercel.app).

Access through:

- `load()` functions via `url` parameter
- inside components via `$page.url.searchParams`

Set by

- `<a href="...">`
- `<form action="...">`
- `goto('?key=value')`

# 🔗 References

- [Svelte Tutorial - Advanced](https://learn.svelte.dev/tutorial/tweens)

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
