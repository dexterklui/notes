---
title: JavaScript Web Events
---

# Web Events

JavaScript itself does not provide events and event methods. This note describes
**_Web Events_** that is provided by Web API (provided by browsers). There are
other event models that are different from Web Event Model, such as event models
of _Node.js_ and _WebExtensions_.

# List of Events

- `change`: When value of an element (e.g. `<input>`) changes. Note that value
  only updates when user leave focus from the element.
- `focus`: When focus on element
- `blur`: When leave focus from element
- `mouseover` and `mouseleave`
- `keydown`
- `click`
- `submit`: Submitting a form

# Adding Events Handlers

## Using method addEventListener

1.  `addEventListener(event, callback)`
2.  `addEventListener(event, callback, abortSignal)`
    - This allows [removing the handler later](#using-abortsignal).

- `event` is a string
- The callback will receive an `Event` object as argument.
- You can add multiple handlers to the same event type.

## Event handler properties

Objects have properties that are named with a prefix "on" followed by the event,
e.g. `onclick` for click event. You can assign a callback to these properties.

The cons is that you cannot add multiple handlers for the same kind of event,
because any assignment overwrites the previous one.

## Inline event handlers

Don't use this approach. Many web server disable these scripts for security
reason. It is also a maintenance nightmare.

See
[MDN web docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#inline_event_handlers_%E2%80%94_dont_use_these).

# Removing Events Handlers

## Using method removeEventListener

`removeEventListener(event, callback)`

- The argument `callback` must be the same function reference.

## Using AbortSignal

For event listeners that were
[added with an AbortSignal](#using-method-addeventlistener), you can remove them
with the `abort()` method of the corresponding `AbortController`.

```javascript
const controller = new AbortController();
btn.addEventListener(
  "click",
  () => {
    /* ... */
  },
  { signal: controller.signal } // pass an AbortSignal
);
// later...
// remove all event handlers associated with this controller
controller.abort();
```

# Event Objects

## Event Object

| Property        | Description                                             |
| --------------- | ------------------------------------------------------- |
| `target`        | Element upon which event occurred                       |
| `currentTarget` | Element within the event bubbling phase                 |
| `cancelable`    | Whether the event can be canceled by `preventDefault()` |

## KeyboardEvent

| Property | Description              |
| -------- | ------------------------ |
| `key`    | The key that was pressed |

### Property key

They are strings. Some examples:

- "a"
- "A"
- "="
- "+"
- " ": The spacebar
- "Enter"
- "Tab"
- "Control"
- "OS": The window key
- "Alt"
- "Shift"
- "End"
- "Home"
- "Delete"
- "Backspace"
- "Insert"
- "PageUp"
- "PageDown"
- "ArrowUp"
- "F1"

It registers **one key at a time**. For `<C-A-t>` for example, it may be
"Control", then "Alt", then "t".

## SubmitEvent

The event target is a `FormObject`, from which you can access the values, e.g.
`e.target.formObject.name.value`.

# Preventing Default Behaviour

Events have their default behaviour. E.g. sending HTTP request and
reload/redirect current page on submission of form. To prevent that, call the
Event method `preventDefault()`.

# Handling Event Propagation

## Stop further propagation

- `event.stopPropagation()` stop this event from propagating onwards.
- Passing `capture` option to `addEventListener()` to make the handler called
  during the [capturing phase](#event-propagation).

  ```javascript
  btn.addEventListener("click", myHandler, { capture: "true" });
  // or simply:
  btn.addEventListener("click", myHandler, true);
  ```

Benefits:

- Prevent functions from interfering with each other
- In complex document, save system resources from searching through DOM for
  handlers

# Concepts

## Event listeners and handlers

An event **_listener_** listens to an event. An event **_handler_** is a
callback that handles the event.

## Event Propagation

1.  **_Capturing phase_**:

    Go from outermost elements towards the innermost target, calling handlers
    that have [`capture` option set](#stop-further-propagation) on the way. This
    is called **_event capturing_**.

2.  **_Bubbling phase_**:

    Go from innermost target and _bubbles up_ to the less nested elements,
    calling handlers that don't have
    [`capture` option set](#handling-event-propagation). This is **_event
    bubbling_**.

### Event delegation

We can delegate event handling to a parent element, instead of adding event
handlers for a large amount of children one after another. In the handler
attached to the parent, we use `event.target` to get the innermost element and
`event.currentTarget` for the element handling the event.

For more detail and an example, see
[MDN web docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#inline_event_handlers_%E2%80%94_dont_use_these).

### Document-wide event handler

Sometimes it is necessary to set a default handler to the `docuemnt` object.
This is especially true for document-wide drag-and-drop action.

You attach `mousedown` handler to the target. Then attach `mousemove` and
`mouseup` event to the document. If these `mousemove` and `mouseup` are
registered to the target layer, they aren't caught.[^1]

# Example Use Cases

Input form validation: `keyup`

# References and Links

References:

- [Introduction to events - MDN web docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events)

Other links:

- [Test your skills: Events](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Test_your_skills:_Events)

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)

# Footnotes

[^1]:
    [Javascript - Event order](https://www.quirksmode.org/js/events_order.html#link8)
