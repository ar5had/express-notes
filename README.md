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

## Adding middleware

Express is a routing and middleware web framework that has minimal functionality of its own: An Express application is essentially a series of middleware function calls.

Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named next.

Middleware functions can perform the following tasks:

   * Execute any code.
   * Make changes to the request and the response objects.
   * End the request-response cycle.
   * Call the next middleware function in the stack.
   * If the current middleware function does not end the request-response cycle,
   it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.
    
To serve static files such as images, CSS files, and JavaScript files, use the express.static built-in middleware function in Express.

Pass the name of the directory that contains the static assets to the express.static middleware function to start serving the files directly. For example, use the following code to serve images, CSS files, and JavaScript files in a directory named public:

    app.use(express.static('public'));
Now, you can load the files that are in the public directory:

    http://localhost:3000/images/kitten.jpg
    http://localhost:3000/css/style.css
    http://localhost:3000/js/app.js
    http://localhost:3000/images/bg.png
    http://localhost:3000/hello.html
Express looks up the files relative to the static directory, so the name of the static directory is not part of the URL.
To use multiple static assets directories, call the express.static middleware function multiple times:

    app.use(express.static('public'));
    app.use(express.static('files'));
Express looks up the files in the order in which you set the static directories with the express.static middleware function.

To create a virtual path prefix (where the path does not actually exist in the file system) for files that are served by the express.static function, specify a mount path for the static directory, as shown below:

app.use('/static', express.static('public'));
Now, you can load the files that are in the public directory from the /static path prefix.

    http://localhost:3000/static/images/kitten.jpg
    http://localhost:3000/static/css/style.css
    http://localhost:3000/static/js/app.js
    http://localhost:3000/static/images/bg.png
    http://localhost:3000/static/hello.html
    
However, the path that you provide to the express.static function is relative to the directory from where you launch your node process. If you run the express app from another directory, it’s safer to use the absolute path of the directory that you want to serve:

    app.use('/static', express.static(__dirname + '/public'));
