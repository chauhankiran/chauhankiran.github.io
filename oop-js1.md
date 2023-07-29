On the other day, I was looking over the source code of the Sails.js framework. Sails.js framework has nice routing configuration based on routes.js file. In this file, you can have the exported object in following way.

module.exports = {
  'get /': 'HomeController.index',
  'get /pages/contact': 'PagesController.contact',
  'post /users': 'UsersController.create',
}

When user visit GET /, framework maps HomeController.js file from controller's folder and execute index method within file. Key in above object is HTTP (GET) verb followed by space and then route(/) and value is file name without extension(HomeController) followed by dot(.) and then method name(index). Nice and clean mapping!

Sails.js framework is based on Express. In other words, the above code at the end should be converted into standard app.get(), app.post(), etc. functions. So, I thought about the possibility of adding this routing based object in simple Express application. Rest of the article is walk-through on how one can add. I do not yet recommended to try this in production application.

---

Create a folder with name routing-object. Go inside, and then create a package.json file to mark this folder as a package/module.

$ mkdir routing-object
$ cd routing-object
$ npm init -y

Install the Express(express) as the production dependency and Nodemon(nodemon) as development dependency.

$ npm i express
$ npm i -D nodemon

Nodemon is used in development to re-start the server when we do the changes. Let's add dev and start scripts in package.json file to run the application in development and production mode respectively.

{
  "name": "routing-object",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "nodemon app.js",
    "start": "node app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "4.18.2"
  },
  "devDependencies": {
    "nodemon": "3.0.0"
  }
}

Finally, create an app.js file and write the following skeleton Express code.

const express = require("express");
const app = express();

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

We can easily run this application in development mode using dev script.

```bash
$ npm run dev
[nodemon] starting `node app.js`
Application is up and running on port 3000
```

With these changes, the base Express application is up and running.

---

Let's add three route GET /, GET /pages/contact, and POST /users as mentioned in the object at starting of this article. I'll just return simple messages for all these routes.

const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.json({ message: "home page" });
});

app.get("/pages/contact", (req, res) => {
  res.json({ message: "contact us" });
});

app.post("/users", (req, res) => {
  res.json({ message: "user created" });
});

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

Test these routes in browser and/or Insomnia.

With these changes, our application is now supporting the above mentioned routes.

Now, we're going to do the changes in the application in such a way that it should effect the output but the structure of the application.

---

Go ahead and create routes.js file in the route folder.

$ touch routes.js

Open this file and copy the object mentioned in this article at the beginning.

module.exports = {
  'get /': 'HomeController.index',
  'get /pages/contact': 'PagesController.contact',
  'post /users': 'UsersController.create',
}

The plan is, when user call POST /users, we should look for the file name UserController.js. Ever further we'll look for this file in controllers folder. So, let's create the controllers folder first in the root folder.

$ mkdir controllers

In this folder, based on the above routes object, we need to create three files - HomeController.js, PagesController.js, and UserController.js.

$ touch controllers/HomeController.js
$ touch controllers/PagesController.js
$ touch controllers/UsersController.js

HomeController.js should have the index method.

module.exports = {
  index: () => {

  }
}

When user visit POST /users, this index method ultimately runs. In other words, if we map the following with code with above code,

app.post("/users", (req, res) => {
  res.json({ message: "user created" });
});

Then 

(req, res) => {
  res.json({ message: "user created" });
}

should be within the index function in HomeController.js file as follow.

module.exports = {
  index: (req, res) => {
    res.json({ message: "user created" });
  }
}

In the similar way, PagesController.js should have the contact method.

module.exports = {
  contact: (req, res) => {
    res.json({ message: "contact us" });
  }
}

And finally, UsersController.js file should have the create method.

module.export = {
  create: (req, res) => {
    res.json({ message: "user created" });
  }
}

With these changes, the routes are ready and the associated code is implemented. Next, we need to map these two.

---

In app.js file, go ahead and remove three routes code as we have that code in other places.

const express = require("express");
const app = express();

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

In order to call the method from the files or controller's files, we need to first import these files. For this, we can use require-all npm package. This package requires all the files and make available for use to directly call. Go ahead stop the server and install this package.

$ npm i require-all

As mentioned in the documentation of this package, we can write the following code.

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

This controllers object now has reference to all the modules or files within controllers folder. To call index method from HomeController we can simple write

controllers.HomeController.index()

Let's use the require-all package in app.js file and require all the controllers.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

With these changes, we just figure out what to do with values of the routes object. Next, we need to fetch the routes from routes.js file and attach the respected method to each of the route by loop through.

---

Let's simply require the routes.js file to fetch all the routes.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

We're going to use for...in loop to loop through the object. 

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

The route is the key and it contains two things - HTTP verb and route itself. We need to fetch both the details from route. To fetch the HTTP verb, we can simply use the RegEx as follow.

const verbExpr = /^(get|post|put|delete)\s+/;
const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;

The first line is the RegEx and loop for four HTTP verbs and second line check for the verbs and return it and saved it into the verb variable.

Let's add these lines in our app.js file.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  const verbExpr = /^(get|post|put|delete)\s+/;
  const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

Next, we need to fetch the route itself. Fetching the route is as simple as replacing the verb with empty string as whatever remain must be the route.

let path = route;
path = path.replace(verbExpr, "");

Instead of directly changing the route iterator, I used intermediate path variable and saved the route in it.

Let's add these lines in our app.js file.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  const verbExpr = /^(get|post|put|delete)\s+/;
  const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;

  let path = route;
  path = path.replace(verbExpr, "");
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

We now have all the needed code but in de-composed mode.

---

Even though all the methods within controllers are available in controllers object, we can not dynamically call them. We need to save the controllers and methods separately to call.

To save the controllers and method from object value, we can simply split by dot(.).

let location = routes[key];
location = location.split('.');
let controllerLocation = location[0];
let methodLocation = location[1];

The above code save the value of route object in location and the we split it by dot(.). With this we can have the controller in controllerLocation and method in methodLocation variables in callable form.

Let's add these lines in our app.js file.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  const verbExpr = /^(get|post|put|delete)\s+/;
  const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;

  let path = route;
  path = path.replace(verbExpr, "");

  let location = routes[key];
  location = location.split('.');
  let controllerLocation = location[0];
  let methodLocation = location[1];
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

The exact callable form is within controllers object. So, let's fetch specific controller and method from the controller per route.

let controller = controllers[controllerLocation];
let method = controller[methodLocation];

Let's add these lines in our app.js file.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  const verbExpr = /^(get|post|put|delete)\s+/;
  const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;

  let path = route;
  path = path.replace(verbExpr, "");

  let location = routes[key];
  location = location.split('.');
  let controllerLocation = location[0];
  let methodLocation = location[1];

  let controller = controllers[controllerLocation];
  let method = controller[methodLocation];
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

With these changes, we now have the verb, route, and the method to call.

---

The only code we now need to write is,

app[verb](path, method);

The above code attach .get() or .post() method to app and the path and method as the argument to it.

Let's add these lines in our app.js file to complete the working Express application.

const express = require("express");
const app = express();

const controllers = require("require-all")({
  dirname: __dirname + "/controllers",
  filter: /(.+Controller)\.js$/
});

const routes = require("./config/routes");

for (let route in routes) {
  const verbExpr = /^(get|post|put|delete)\s+/;
  const verb = key.match(verbExpr || [])[key.match(verbExpr || []).length - 1] || null;

  let path = route;
  path = path.replace(verbExpr, "");

  let location = routes[key];
  location = location.split('.');
  let controllerLocation = location[0];
  let methodLocation = location[1];

  let controller = controllers[controllerLocation];
  let method = controller[methodLocation];

  app[verb](path, method);
}

app.listen(3000, () => {
  console.log("Application is up and running on port 3000");
});

Go ahead and re-run all three route in browser and/or Insomnia. You should see the identical output. 

The above code is not complete. I intentionally left many scenarios and improvements. For example,

1. You can add all the HTTP verbs in verbExpr and even do case-insensitive matching.
2. You can place this code into separate file.
3. You can have better names for the variables.
4. You can even implement your own require-all package as this package quite small as one file only.
5. You can add error checking as what if someone forgot to add . between controller and method name of added tab between HTTP verb and route instead of single space, etc.














# OOP in JavaScript Pt. 1

To define a class in JavaScript, `class` keyword is used.

```js
class Hello {

}
```

We just defined the class with name `Hello` in PascalCase.

Class can have the functions or _methods_.

```js
class Hello {
  greet() {
    console.log("hello, world");
  }
}
```

We just defined the `greet()` method within `Hello` class.

In order to call this method, we need to first create an object of the class using `new` keyword.

```js
const h = new Hello();
```

Using dot operator or `.`, we can invoke the `greet()` method on created `h` object.

```js
h.greet()
// => hello, world
```

---

Class can have _constructor_. For this, JavaScript has the `constructor()` method.

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  print() {
    console.log(`x is: ${this.x} and y is: ${this.y}`)
  }
}
```

We just defined the `Point` class with constructor that accepts `x` and `y` as values when creating a new object instance and print() method to just log the entered values of `x` and `y`.

We can now utilize this constructor when creating an instance of it by passing the default value of the `x` and `y`.

```js
const p = new Point(1, 2);
p.print();
// => x is: 1 and y is: 2
```

We can even update the value of `x` and `y` at the later point of the program.

```js
p.x = 2;
p.y = 4;

p.print();
// => x is: 2 and y is: 4
````

We can even have the multiple instances of the `Point` class with different value of `x` and `y` i.e. unique values per object.

```js
const p1 = new Point(3, 3);
const p2 = new Point(0, 0);
const p3 = new Point(-3, -3);
```

---
