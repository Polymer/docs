---
title: Events
---

<!-- toc -->

## Event listener setup {#event-listeners}

Add event listeners to the host element by providing a
`listeners` object that maps events to event handler function names.

You can also add an event listener to any element in the `this.$` collection
using the syntax <code><var>nodeId</var>.<var>eventName</var></code>.

Example: { .caption }

```
<dom-module id="x-custom">
  <template>
    <div>I will respond</div>
    <div>to a tap on</div>
    <div>any of my children!</div>
    <div id="special">I am special!</div>
  </template>

  <script>
    Polymer({

      is: 'x-custom',

      listeners: {
        'tap': 'regularTap',
        'special.tap': 'specialTap'
      },

      regularTap: function(e) {
        alert("Thank you for tapping");
      },

      specialTap: function(e) {
        alert("It was special tapping");
      }

    });
  </script>
</dom-module>
```

## Annotated event listener setup {#annotated-listeners}

To add event listeners to local DOM children, use
<code>on-<var>event</var></code>  annotations in your template. This often
eliminates the need to give an element an `id` solely for  the purpose of
binding an event listener.

Example: { .caption }

```html
<dom-module id="x-custom">
  <template>
    <button on-tap="handleTap">Kick Me</button>
  </template>
  <script>
    Polymer({
      is: 'x-custom',
      handleTap: function() {
        alert('Ow!');
      }
    });
  </script>
</dom-module>
```


**Tip: Use `on-tap` rather than `on-click` for an event that fires consistently
across both touch (mobile) and click (desktop) devices**. See [gesture
events](gesture-events) for a complete list of reliable, cross-platform events.
{.alert .alert-info}

Because the event name is specified using an HTML attribute, **the event name is always
converted to lowercase**. This is because HTML attribute names are case
insensitive. So specifying `on-myEvent` adds a listener for `myevent`. The event handler
_name_ (for example, `handleClick`) **is** case sensitive.

**Lowercase event names.** When you use a declarative handler, the event name
is converted to lowercase, because attributes are case-insensitive.
So the attribute `on-core-signal-newData` sets up a listener for `core-signal-newdata`,
_not_ `core-signal-newData`. To avoid confusion, always use lowercase event names.
{.alert .alert-info}

## Imperatively add and remove listeners {#imperative-listeners}

Use [automatic node finding](local-dom#node-finding) and the
convenience methods
[`listen`](/1.0/docs/api/Polymer.Base#method-listen) and
[`unlisten`](/1.0/docs/api/Polymer.Base#method-unlisten).

```js
this.listen(this.$.myButton, 'tap', 'onTap');
this.unlisten(this.$.myButton, 'tap', 'onTap');
```

The listener callbacks are invoked with `this` set to the element instance.

If you add a listener imperatively, you need to remove it imperatively.
This is commonly done in the `attached` and `detached`
[callbacks](registering-elements#lifecycle-callbacks). If you use
the [`listeners`](#event-listeners) object or [annotated event
listeners](#annotated-listeners), Polymer automatically adds
and removes the event listeners.

## Custom events {#custom-events}

To fire a custom event from the host element use the `fire` method. You can also pass in data to event handlers as an argument to `fire`.

Example: { .caption }

```html
<dom-module id="x-custom">
  <template>
    <button on-click="handleClick">Kick Me</button>
  </template>

  <script>
    Polymer({

      is: 'x-custom',

      handleClick: function(e, detail) {
        this.fire('kick', {kicked: true});
      }

    });

  </script>

</dom-module>
<x-custom></x-custom>

<script>
    document.querySelector('x-custom').addEventListener('kick', function (e) {
        console.log(e.detail.kicked); // true
    })
</script>
```


## Property change events {#property-changes}

You can configure an element to fire a non-bubbling DOM event when a specified
property changes. For more information, see [Change notification events](#change-events).

## Event retargeting {#retargeting}

Shadow DOM has a feature called "event retargeting" which changes an event's
target as it bubbles up, such that target is always in the same scope as the
receiving element. (For example, for a listener in the main document, the
target is an element in the main document, not in a shadow tree.)

Shady DOM doesn't do event retargeting for events as they bubble, because the
performance cost would be prohibitive. Instead, Polymer
provides a mechanism to simulate retargeted events when needed.

Use `Polymer.dom(event)` to get a normalized event object that provides
equivalent target data on both shady DOM and shadow DOM. Specifically, the
normalized event has the following properties:

*   `rootTarget`: The original or root target before shadow retargeting
    (equivalent to `event.path[0]` under shadow DOM or `event.target` under
    shady DOM).

*   `localTarget`: Retargeted event target (equivalent to `event.target` under
    shadow DOM). This node is always in the same scope as the node where the
    listener was added.

*   `path`: Array of nodes through which event will pass
    (equivalent to `event.path` under shadow DOM).

Example: { .caption }

```html
<!-- event-retargeting.html -->
 ...
<dom-module id="event-retargeting">
  <template>
    <button id="myButton">Click Me</button>
  </template>

  <script>
    Polymer({

        is: 'event-retargeting',

        listeners: {
          'click': 'handleClick',
        },

        handleClick: function(e) {
          console.info(e.target.id + ' was clicked.');
        }
      });
  </script>
</dom-module>


<!-- index.html  -->
  ...
<event-retargeting></event-retargeting>

<script>
  var el = document.querySelector('event-retargeting');
  el.addEventListener('click', function(){
    var normalizedEvent = Polymer.dom(event);
    // logs #myButton
    console.info('rootTarget is:', normalizedEvent.rootTarget);
    // logs the instance of event-targeting that hosts #myButton
    console.info('localTarget is:', normalizedEvent.localTarget);
    // logs [#myButton, document-fragment, event-retargeting,
    //       body, html, document, Window]
    console.info('path is:', normalizedEvent.path);
  });
</script>
```

In this example, the original event is triggered on a `<button>` inside the `<event-retargeting>`
element's local DOM tree. The listener is added on the `<event-retargeting>` element itself, which
is in the main document. To hide the implementation of the element, the event should be retargeted
so it appears to come from `<event-retargeting>` rather than from the `<button>` tag.

The document fragment that appears in the event path is the root of the local DOM tree. In shady DOM
this is an instance of `DocumentFragment`. In native shadow DOM, this would show up as an instance of
`ShadowRoot` instead.
