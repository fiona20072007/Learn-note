# 20200729 Navigation From Scratch

```js
import React from "react";

const Header = () => {
  return (
    <div className="ui secondary pointing menu">
      <a href="/" className="item">
        Accordion
      </a>
      <a href="/list" className="item">
        Search
      </a>
      <a href="/dropdown" className="item">
        Dropdown
      </a>
      <a href="/translate" className="item">
        Translate
      </a>
    </div>
  );
};

export default Header;
```

The headers can be very basic. We'll show anchor elements to direct the user to the appropriate URLs to show all of our different components.

even though it looks like it works. However, there is a really big downside to this approach. I'm then going to click on one these links and when I do so, you'll see that a variety of different requests are being issued. So every single time that I click on one of these links, I'm making a wide variety of different requests.

![my-img](img/200729-1.png)

This diagram right here is meant to represent a traditional normal HTML based Web application. So not react or anything like that. In a normal Web application, kind of a traditional Web app consisting of a variety of different HTML documents you might navigate to some Web page inside of your browser. Whenever you navigate to a page, your browser makes a request off to some server and gets back in HTML document. The HTML then parsed and displayed on the screen. Any appropriate script tags are loaded up along with any appropriate CSS tags as well.

Then whenever a user clicks on a link, your browser makes an entire another request to another server, and gets back another HTML document. And again, that HTML document might have its own set of script tags and CSS. This is how a traditional Web page handles navigation. We provide normal links whenever you click on a link. You load up another HTML document.

And this is exactly what is happening inside of our app right now. Whenever we click on one of these links, we completely reload the entire index.HTML file inside of our project and reload all the JavaScript and all the CSS as well. And that is not ideal inside of a react application. We have already, when we first come to our application, loaded up our index.HTML file, all the JavaScript, all the CSS. There is no reason in a react app for us to do a hard reload of the page and reload all these different assets. Instead will be really ideal is if we could click on one these links, update the URL but not do a full page reload. Because a full page reload causes a whole bunch of network traffic that is not required just to change some very basic content on the screen.

![my-img](img/200729-2.png)

If I clicked on that list link and went over to this other page, here is ideally what we would do inside of our app. So first, a user would click on that list link and then ideally we would change the URL but not do a full page refresh. So in other words, ideally, we would not make all these additional requests because we've already loaded up all that JavaScript and all that CSS into our app. Then each route component ideally could detect that the URL has changed. We could then possibly have each route component update some piece of state that is tracking the current path name. When we update some piece of state, each route could then re-render and show the appropriate or hide the appropriate components.

So in other words, whenever we click on some link inside of application in a react app, we just plain do not want to refresh the entire page. All we want to do is update the URL. And get all of our different routes to update as well. But we don't want to reload the index.HTML or all the associated CSS and JS.

![my-img](img/200729-3.png)
