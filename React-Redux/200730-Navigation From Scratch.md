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

The last thing we have to do now, we need to make sure that anytime that URL does get change, the route component updates some piece of state which will cause the route component to re render. And that would be our opportunity to decide whether or not we want to now hide or show the component that the route is displaying.

so now the last thing we have to do is make sure that any time on location changed function is called, we have to tell the route component to update itself or to re-render itself. When it re renders itself, we're going to fetch whatever the current path name is, and use that to decide whether or not this route component should now display its child or hide it.

we're going to create a piece of state that essentially follows or tracks whatever the value of window.location.pathname is. This property right here is always kept perfectly up to date. In other words, whenever you look at window.location.pathname, it's always gonna reflect exactly whatever the current path name is inside the address bar. we're essentially introducing a piece of state that is just has the sole purpose of trying to get the route component to re render itself as the only job for this piece of state, because we could otherwise just refer to window.location.pathname to figure out what the current path name is.

```js
const Route = ({ path, children }) => {
  const [currentPath, setCurrentPath] = useState(window.location.pathname);

  useEffect(() => {
    const onLocationChange = () => {
      setCurrentPath(window.location.pathname);
    };
    window.addEventListener("popstate", onLocationChange);

    return () => {
      window.removeEventListener("popstate", onLocationChange);
    };
  }, []);
  return currentPath === path ? children : null;

  //return window.location.pathname === path ? children : null;
};
```

whenever our location changes, we will call set current path and update its value to be whatever the current path name is inside of that address bar. whenever this route component is first displayed on the screen, current path is set to be whatever path name is. And then whenever the pathname changes, we just update current path to be once again, whatever path name is. The only reason this piece of state exists is just to get our route to update.

finally, down here at the return statement, we could update this to be currentPath or technically it could also stay as window.location.pathname as well. It really doesn't make a difference.

Now, as you click around, if you go to that search component, you might see a warning like this appear. It's because essentially are your request over to that Wikipedia API is being resolved only after we have navigated away. You'll probably only see that warning if you're clicking between these different tabs very quickly.

Normally, whenever you find a link, you should be able to hold down command on your keyboard if you're on Mac OS and click it or hold down control if you're on windows and click it. And in either case, that link should be opened up in a new tab.

```js
const onClick = event => {
  if (event.metaKey || event.ctrlKey) {
    return;
  }

  event.preventDefault();
  window.history.pushState({}, "", href);

  const navEvent = new PopStateEvent("popstate");
  window.dispatchEvent(navEvent);
};
```

So to do so, we're gonna go back to our link component. And then inside of onClick function, we're going to add in a little check on that event. metaKey & ctrlKey are both boolean properties. These are going to be either true or false to indicate whether or not that respective key was held down when a user clicked on this thing. So if either these are true, then we're going to not want to run any this stuff below. Instead, we're going to want to allow the browser to just do its normal thing, which is to open up a new tab and navigate to the href on this link. So if either these cases are true, we're going to return early.
