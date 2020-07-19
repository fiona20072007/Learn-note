# 20200719 Understanding Hooks in React

To set up this cascading timer of sorts, we're going to first write in some code that is going to just set up a timer to do the search in 500 milliseconds. So we're still going on the same behavior as before where every single key press results in a search. They will just all be lagged by 500 milliseconds. Once we get this code together, we'll then start to discuss how to add in that cancel step.

```js
setTimeout(() => {
  if (term) {
    search();
  }
}, 500);
```

Now if I put in 'programming', everything is lagged by 500 milliseconds.

```js
setTimeout(() => console.log("Hi"), 10000); //116
clearTimeout(116);
```

whenever you create a time out using set time out, you get back a number or an integer like 116. This is an identifier that identifies the timer that was just created. Before that timer times out or completes, we have the opportunity to cancel this timer by making use of a function that is built into JavaScript. I'm going to very quickly call clearTimeout and pass in that 116. By calling clearTimeout, we are cancelling this timeout right here, and even after ten seconds, that arrow function is not going to be invoked.

Whenever we call setTimeout, we get back an identifier, we can use that id right there to stop the timer from executing.

So we can take this set time out statement and assigned the result of it to a variable timeoutId. And then at some point in time in the future, we can attempt to clear our time out making use of the clear timeout function. All we have to do is pass in that Id.

---

```js
useEffect(() => {
  console.log("Initial render or term was changed");

  return () => {
    console.log("CLEANUP");
  };
}, [term]);
```

With this current setup, we're going to invoke that arrow function the very first time our component is rendered to the screen. We will then also invoke this function any time the term piece of state changes.

Whenever we define useEffect or call it, and we provide an function as the first argument. There's only one type of value we are allowed to return from this arrow function. The only thing that we are allowed to return is another function. The goal of the returned arrow function is to do essentially some cleanup.

whenever our component first renders the overall arrow function is invoked and we return that inner arrow function. Then any time that is time to run outer arrow function again, first, react is going to call the cleanup function that it got from the last time useEffect ran. And then it's going to call the overall arrow function again.

As soon as that is time our useEffect to be invoked again, first, the cleanup function we had returned from the last time around gets invoked, and then the overall function gets invoked again.

![my-img](img/200719-1.png)

```js
useEffect(() => {
  const search = async () => {
    const { data } = await axios.get("https://en.wikipedia.org/w/api.php", {
      params: {
        action: "query",
        list: "search",
        origin: "*",
        format: "json",
        srsearch: term
      }
    });
    setResults(data.query.search);
  };

  const timeoutId = setTimeout(() => {
    if (term) {
      search();
    }
  }, 500);

  return () => {
    clearTimeout(timeoutId);
  };
}, [term]);
```

Now, whenever a user first clicks or types inside of that input, we are going to immediately set a timer to search in five hundred milliseconds. And we return that cleanup function. cleanup function does not get invoked right away. Instead, the next time a user type something, we're gonna cancel the previous timer and set up another one. If the user types something again, cancel a previous timer, set up another one, we're gonna repeat this process over and over again until five hundred milliseconds goes by without useEffect being invoked. So those five milliseconds go by, we're going to execute that timer automatically (We do our search the API. We update our results piece of state and we should see our results appear on the screen.).

The one downside at present is that we do not do any initial search until after 500 ms the lapses when our application first starts up.

So to fix this up, we're just going to add in a couple of little checks into our hook. So we really have to do inside of useEffect is detect whether or not this is the first time our component is being rendered. If it is the very first time, we're going to skip doing any kind of time out, we're just going to do a search immediately. Then for every additional type that this useEffect function is called, if we know for certain that this is not the first time that the component is rendered, then we will go ahead and set the time out and return that cleanup function.

```js
useEffect(() => {
  const search = async () => {
    const { data } = await axios.get("https://en.wikipedia.org/w/api.php", {
      params: {
        action: "query",
        list: "search",
        origin: "*",
        format: "json",
        srsearch: term
      }
    });
    setResults(data.query.search);
  };

  if (term && !results.length) {
    search();
  } else {
    const timeoutId = setTimeout(() => {
      if (term) {
        search();
      }
    }, 500);

    return () => {
      clearTimeout(timeoutId);
    };
  }
}, [term]);
```

if there is a term present and we have no results yet, that probably means that this is the first time that we are rendering our component, and we probably want to do a search right away.

if we do have a term or do not have a term, whatever it is, and if we do already have some results, then we probably want to set up our time out and return the cleanup function.

---

![my-img](img/200719-2.png)

But we have a new warning right here around react hook use effect has a missing dependency.

Anytime that you refer to a prop or a piece of state inside of useEffect, react or specifically a rule built in to create react app or something called ESLint is going to want you to reference any different prop or piece of state inside of the useEffect dependency array([term]), it is the array that controls when useEffect gets executed.

So if you ever make reference to a piece of state or props, that rule wants you to see it listed inside
