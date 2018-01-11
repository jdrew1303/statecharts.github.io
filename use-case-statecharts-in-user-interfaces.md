# Statecharts in User Interfaces

Statecharts are a good fit for solving certain problems with coding user interfaces.  Both statecharts and most UI technologies are event driven, but the technologies complement one another very well.

## Event Action paradigm

Most UIs are event driven, meaning that the user interface components themselves generate events when the user interacts with them.  These events typically trigger actions attached directly to the UI components.  In some systems, the events are passed around things like component hierarchies, or forwarded to an event loop that gets to decide what _action_ to perform.  The implementation of the UI component somehow decides what to do, what to show, what to stop doing and what to stop showing.

This is called the **Event Action** paradigm, because the action is tightly coupled to the event.

Statecharts inject themselves between the event being generated and the action being performed.  A statechart's main function is to take an event created somewhere, and make a decision on _doing stuff_.  It does precisely what most event driven UIs _don't_ do, or provide a framework for.

## A comparison

In a traditional event driven user interface component, say, the ubiquitous HTML `<input>` element, the UI component generates a lot of events, that the developer can subscribe to, from gaining or losing focus, editing, selecting, mouse movement and so on.  A developer has a plethora of events to choose between.  There is a similar almost infinite set of things that can be _done_ to a user interface.  Each element has a wide range of possible mutations.  It is up to the developer to decide _what to do_ based on whatever event that happens.

In simple user interfaces, the event-action paradigm is fine.  Here, for example is an input handler that turns the text green when text is entered into the field:

```css
.green {
    box-shadow: 0 0 30px green
}
```

```html
<input id="my_editor">
```

```js
var field = document.getElementById("my_editor");
function handleChange(e) {
  field.classList.add("green")
}
field.onchange = handleChange
```

Don't cringe, it's a simple component that turns green (an action) whenever a particular event happens (the field is modified).  This is the component's **behaviour**.

However, in order for this code to support _anything_ other than turning it green when it's modified, it will need conditional logic.  If the field should only turn green if the has any text, then the event handler needs an _if_ test, _and_ it needs to check the "current state" of the component.  This is the beginning of the complexity creep.

```js
function handleChange(e) {
  if (field.value == "") {
    field.classList.add("green")
  }
}
```