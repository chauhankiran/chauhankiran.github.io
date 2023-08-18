# Sails Tutorial - Chapter 1

We're going to build the Sails.js application from the scratch with some unusual steps. At the end, we'll have the small working CRM application. Without further ado, let's get started.

Create a folder with name `crm`.

```bash
$ mkdir crm
```

Go inside the created folder.

```bash
$ cd crm
```

Create a `package.json` file with defaults.

```bash
$ npm init -y
```

Install the Sails framework (`sails`) as the dependecy in this project.

```bash
$ npm i sails
```

Also install the Nodemon (`nodemon`) to restart the development server on change as development dependecy.

```bash
$ npm i -D nodemon
```

Go ahead and create a new file with name `app.js`.

```bash
$ touch app.js
```

Open this file and write the following code.

```js
const sails = require('sails');

sails.lift();
```

We first `require`d the sails package and then invoked the `.lift()` method. That's it!

Open `package.json` file and add two scrips - one to run `app.js` with `node` and second to run `app.js` with `nodemon`.

```json
{
  "name": "crm",
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
    "sails": "^1.5.7"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

Also, change the `main` file from `index.js` to `main.js`.

```json
{
  "name": "crm",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "dev": "nodemon app.js",
    "start": "node app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "sails": "^1.5.7"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

In the terminal, run the `dev` script.

```bash
$ npm run dev
```

The application is running at http://localhost:1337. Open this in the browser and you shoud see 'Not Found' with 404 status code. This is obvious as we haven't write any code for the root route. But, the important thing is you just wrote the Sails application in up and running mode with just two lines of code in `app.js` file. Rest of the things were setup!
