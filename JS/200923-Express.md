# Express Basics

Express is a web framework for node.js.

web framework: a set of software tools to create dynamic web applications.

patterns: solutions for common problems in software.

routing: how you specify what content is to be returned based on the URL requested.

Web Frameworks Tools:

1. Templating: for dynamic layouts and content
2. URL Mapping / Routing
3. User Input Processing

---

To start a node project in a directory without a package.json file run the following command: npm init

```npm
npm init -y
npm install express@4.15.2 --save
node app.js

npm install -g nodemon
```

```js
const express = require("express");
```

---

setting up a server with Express:

express sets up its server, a server is a program that runs on a remote computer, its job is to wait for HTTP requests from clients. Request is what is happening when you type in a URL into a browser. Your browser is making a request to a server at the URL address you typed in. When a browser or client makes an HTTP request to the server, the server brings into action, putting together a response.

```js
const express = require("express");
// require the express module
const app = express();
// create Express application
app.listen(3000);
// setup the development server, port number 3000
```

This code will create a server. When I run it, the server will run on my machine. And I can send a request through a URL called localhost.

---

Creating a Route with Express:

express handles request through routes. From user's perspective, a route is like a URL. From application perspective, a route also called an end point, is a command to run a specific function, which in turn sends a response back to the client.

route: It's the path a user takes to access data on a server.

When type URL into browser, the browser sends get request to the server (you're asking to view or get a web page)(get: HTTP verb, represents what the client wants the server to do)

The URL (noun, or called resource) tells the server what to get for client. A server responds to a get request by sending information off in a web page.

The browser can also send information to the server, like submit a form, called the post request.

when the client makes a request to a route, if the server isn't set up to respond to, server will send back an 404 error.

To create a route, use get method on the app object. get method is used to handle the get requests to a certain URL.

```js
//indicate root route
//first parameter is called the location parameter
app.get("/", (request, response) => {
  //callback will run when the client request this route
  response.send("<h1>I love treehouse!</h1>");
});
```

---
