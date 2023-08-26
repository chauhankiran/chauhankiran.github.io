# Express.js Code Reading

Following are the list of the points that I find interesting while reading Express code (I'm still reading the source code).

- Express 4.x support Node v0.10. Due to this, code is [not written in ES6](https://github.com/expressjs/express/pull/4522).
- ['use strict' increase the performance](https://github.com/expressjs/express/commit/e71014f522fb8db9ba1ef634f2e9f2c8069e4b6c).
- When you write `app.set('view engine', 'something')`, Express will check `something` module in `node_modules` and load it. EJS use this trick and due to this, you just have write this line only. Even you don't have to import `ejs` (or `something` in example).
- All the repos under [Express](https://github.com/expressjs) org on GitHub has either index.js if the package is consist of only one file or the package code is within lib folder.

_In progress. I'll add more points as I learn more._
