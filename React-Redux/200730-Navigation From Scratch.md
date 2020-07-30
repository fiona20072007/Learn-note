# 20200730 Navigation From Scratch

Whenever a user navigates to some page, they generally have the expectation that they should be able to bookmark that page and then come back to it in the future and see the same content up here on the screen. They also had the expectation that if they copy the link that they see right here, open it up in a new tab paste the link and hit enter. They should see the exact same content. You'll notice right now that did not happen for me. That's why as developers, we always try to make sure that the URL is kept in sync with the content that is displayed on the screen.

So in order to change the URL, but not do a full page refresh. We are going to use a function that is built directly into the browser.

```js
window.history.pushState({}, "", "/translate");
```

I'm going to provide an empty object as the first argument, as a second I'll provide an empty string, and as a third I'll provide the route or the path name that I want to change the URL to. I put in translate right there, which means the URL should be updated to slash translate. But I should not otherwise see any other change to my application. We can change the URL. We can keep it in sync with what content we are displaying, but we are not actually refreshing the page.

```js
const Link = ({ className, href, children }) => {
  const onClick = event => {
    event.preventDefault();
    window.history.pushState({}, "", href);
  };

  return (
    <a onClick={onClick} className={className} href={href}>
      {children}
    </a>
  );
};
```

And then the third argument we're going to provide the href that was passed to the link component.

---

```js
const Link = ({ className, href, children }) => {
  const onClick = event => {
    event.preventDefault();
    window.history.pushState({}, "", href);

    const navEvent = new PopStateEvent("popstate");
    window.dispatchEvent(navEvent);
  };

  return (
    <a onClick={onClick} className={className} href={href}>
      {children}
    </a>
  );
};
```

Anytime we do change that URL, we need to make sure that all of our different route components detect that change. To do so, we're gonna make sure that whenever user clicks on that link, we produce and emit a navigation event of sorts. This navigation event is going to be sent over to all of our different route components. So this is how we're going to tell our route components that the URL has just changed.

the purpose of this code is to tell those route components that some data just changed or specifically that the URL just changed. All you need to understand is that this is going to communicate over to those route components that the URL has just changed.

we're then going to go over to our route component and add some code inside there to listen for this event. We have to set up a route handler inside there to listen for that event we just dispatch. Anytime we set up an event listener inside of a component that's usually a sign that we need to define a useState hook.

```js
const Route = ({ path, children }) => {
  useEffect(() => {
    const onLocationChange = () => {
      console.log("Location Change");
    };
    window.addEventListener("popstate", onLocationChange);

    return () => {
      window.removeEventListener("popstate", onLocationChange);
    };
  }, []);
  return window.location.pathname === path ? children : null;
};
```

Now, quick reminder on event handlers that we wire up manually inside of a component. Usually we only want to run that event handler one time or wire it up and start listening one time. So that is usually a sign that inside of useEffect, we want to provide a second argument of an empty array. So we only run that arrow function inside of useEffect when route component is first rendered to the screen. Then inside of arrow function, we can start to wire up an event listener. We're going to listen for an event of popstate. And anytime that occurs, I want to run a function (on location change).

why my defining this callback function as a separate variable? remember, if we ever decide to stop showing the route component on the screen, we would want to make sure that we clean up this event listener. So from this useEffect function, I return a cleanup function and inside there we will remove that event listener

so now whenever we click on a link, we should see that console log appear inside of our console. Every single time I click on a link, I get four separate console logs because we have four copies of the route component right now, and each of them is listening for that navigation event. So this means that we can update the URL and detect that has been updated inside of our route components as well.
