# 20200722 Understanding Hooks in React

![my-img](img/200722-1.png)

the dropdown component itself doesn't actually get inserted into the DOM, in other words, there's not an element inside the DOM called dropdown. I only put this little block inside of here to indicate that everything underneath this represents elements created by the dropdown components.

with our dropdown component, we have a react component. Whenever we create a react component. We return some JSX, that JSX that we return, create some number of elements on the screen. And we can also optionally setup event handlers by providing some props to those elements.

And the key thing to keep in mind here is that the drop down component can only use that syntax(onClick={() => onSelectedChange(option)}) to set up event handlers on elements that it creates. The dropdown can only watch for clicks easily on these elements.

![my-img](img/200722-2.png)

The dropdown, on the other hand, can not easily listen to click events or any kind of event on any component or any element that is not created by the dropdown. The dropdown can not easily set up a click event handler on the body, or some element that is created by the accordion or some other div that might exist inside of our app.

We are trying to click on some element on the screen that is not created by the dropdown and the dropdown just has a hard time to receive or listen to any events that are triggered by clicking anywhere else on the screen. We want to listen to click events being issued to something outside of the dropdown.

because the built in event system that we have with react, this entire feature of adding on click event handlers to particular elements only allows a component to listen to clicks on elements that are created by that component.

---

This is just how the browser works and how the DOM works.

The dropdown does not actually create an element for itself in the DOM.

![my-img](img/200722-4.png)

So we're gonna imagine for a second that a user clicks on this item right here. The browser itself then creates an event object, the event object describes an information about the click (for example, where the user's mouse was is on the screen and what element the user just clicked on).

The browser then hands that event object off to react, react does a little bit of processing on that event and then provides an event object to our onClick event handler.

```js
<div
  key={option.value}
  className="item"
  onClick={() => onSelectedChange(option)} //this one
>
  {option.label}
</div>
```

So onClick gets invoked and the first argument to it is an event object.

Now, here's the key thing to understand, and this is where the term event bubbling comes from.

Whenever a user clicks on that element, the event does not stop there. Instead, this event object then travels up to the next parent element. So the parent element to that div, in this case, it is a div with class name ui.menu. If that element has a click event handler on it, it is automatically invoked.

The event object then goes up to the next parent element, in this case, it is the div with the class of ui.selection. If that element has an onClick event handler, it is also invoked with this event object. And then the event travels up to all these successive parent elements. And in every step the browser checks to see if that element has a click event handler. If it does, it is invoked automatically. This is referred to as event bubbling.

So we would say that this event is kind of bubbling up or kind of rising up our DOM structure.

whenever we click on an element, the dropdown closes. It is because the user is clicking on this div right here. We run that on click. We update the currently selected item, the event then bubbles up, goes to the div with class of ui.selection, which does have an onClick function tied to it. That onClick right there is executed, and we update our open piece of state, which causes the dropdown to close.

![my-img](img/200722-5.png)

So what we've released is that the dropdown component needs to detect a click event on any element inside of our document besides an element that it created. So if a user clicks anywhere outside the dropdown, the dropdown needs to detect that.

We kind of established that the dropdown component has a tough time setting up event handlers on elements that it did not itself create. It is not impossible for a react component to setup event handlers on other elements, but it is just a little bit challenging.

If we click on some element, that event is going to bubble up our DOM structure.

![my-img](img/200722-6.png)

So our solution is going to be to have our dropdown components setup a manual event listener, and we're going to set up that listener on the body element. Then anytime that someone clicks on any element inside of our entire document, that event is going to bubble up to the body element, and that will thus tell the dropdown that something has been clicked.

we can make use of native browser events and event listeners as much as we please from our react code. So I wanted to set up a event listener on the body element without using react, we would write inside of our console.

```js
//in browser console
document.body.addEventListener("click", () => console.log("click!"));
```

the first argument to this is going to be the type of event we want to listen for, which is a click, and then the second argument will be a function to run anytime that event occurs. Now, if I click on any element on the entire screen, the event is going to bubble up, eventually get to the body and we should see a console log of click.

Without a doubt, we can setup a manual event listener, anytime we click anywhere, the event bubbles up.

```js
useEffect(() => {
  document.body.addEventListener("click", () => {
    console.log("CLICK!");
  });
}, []);
```

So we can setup a useEffect hook inside of dropdown, and inside there, whenever our component is first rendered onto the screen, we could set up an event listener to listen to that body element. And I want to make sure that this arrow function inside useEffect only runs one time when we first render our component onto the screen. I only want to run it one time because I only need to set up the event listener one time.

```js
useEffect(() => {
  document.body.addEventListener("click", () => {
    setOpen(false);
  });
}, []);
```

We can open up our dropdown and select an item, but the drop down stays open and it's only when we click outside of it now that the dropdown actually closes.

![my-img](img/200722-8.png)

So if we really think about event bubbling, you would probably guess that first event listener inside div.item gets invoked, and then inside div.ui.selection, and then inside body.

![my-img](img/200722-7.png)

Take a look at the order in which those event listeners actually get invoked.

These purple boxes represent event handlers that have been wired up through react. And the green one is one that was wired up with a manual event listener. Whenever we think about the order in which event listeners are invoked, all the event listeners that were wired up using add event listener actually get called first. After all of those are called, and only then do all of our react event listeners get called. And it's always from the most child element up to the most parent.

```js
//body
document.body.addEventListener("click", () => {
  setOpen(false);
});
//item
onClick={() => onSelectedChange(option)}
//dropdown
onClick={() => setOpen(!open)}
```

Now that the body event listener is always gonna be the first to be invoked. So inside there, the first thing that happens whenever we click on an item is we set open to false, which in theory closes our dropdown.

Then the next thing that happens is the click on the individual item. So we update our currently selected option, that doesn't have anything to do with opening or closing the dropdown.

And then finally, the last thing that actually gets invoked is the on Click listener to the dropdown itself. We take whatever the current value of open is and flip it to its opposite. Well, that means that we'd take that false value and we flip it back over to true, which results in the dropdown staying open. In theory, the dropdown does close for a fraction of the second, but then it immediately opens back up when this event listener gets invoked. So that is why the drop down appears to always be open whenever we click on an item.
