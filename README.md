# express-notes
Reading these notes along with doing [expressworks](https://github.com/azat-co/expressworks) exercises will benifit you the most

## What is express ?
Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.

## Creating a simple express js app

This is how we can create an Express.js app on port 3000, that responds with
a string on '/':

```js
    var express = require('express')
    var app = express()
    app.get('/', function(req, res) {
      res.end('Hello World!')
    })
    app.listen(3000)
```

Program to create an Express.js app that outputs "Hello World!" when somebody goes to `/home` :

``` js
    var express = require("express");
    var app = express();
    app.get("/home", function(req, res) {
        res.end("Hello World!");
    });
    app.listen(process.argv[2]);
```
