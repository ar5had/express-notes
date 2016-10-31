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

```js
    app.use(express.static('public'));
```
Now, you can load the files that are in the public directory:
```js
    http://localhost:3000/images/kitten.jpg
    http://localhost:3000/css/style.css
    http://localhost:3000/js/app.js
    http://localhost:3000/images/bg.png
    http://localhost:3000/hello.html
```
Express looks up the files relative to the static directory, so the name of the static directory is not part of the URL.
To use multiple static assets directories, call the express.static middleware function multiple times:
```js
    app.use(express.static('public'));
    app.use(express.static('files'));
```
Express looks up the files in the order in which you set the static directories with the express.static middleware function.

To create a virtual path prefix (where the path does not actually exist in the file system) for files that are served by the express.static function, specify a mount path for the static directory, as shown below:

Now, you can load the files that are in the public directory from the /static path prefix.
```js
    http://localhost:3000/static/images/kitten.jpg
    http://localhost:3000/static/css/style.css
    http://localhost:3000/static/js/app.js
    http://localhost:3000/static/images/bg.png
    http://localhost:3000/static/hello.html
```   
However, the path that you provide to the express.static function is relative to the directory from where you launch your node process. If you run the express app from another directory, it’s safer to use the absolute path of the directory that you want to serve:
```js
    app.use('/static', express.static(__dirname + '/public'));
```
In Node.js, __dirname is always the directory in which the currently executing script resides. In other words, you typed __dirname into one of your script files and value would be that file's directory.

By contrast, . gives you the directory from which you ran the node command in your terminal window (i.e. your working directory).

The exception is when you use . with require(). The path inside require is always relative to the file containing the call to require, so . always means the directory containing that file.

## Using path to locate a folder
To get the file path to a folder we will require the native Node module path, near the top of app.js.
```js
    var path = require('path');
```
The path module exposes a join method that allows us to chain together variables to create a file path. The join method is used instead of specifying a full file path, as this avoids issues of operating systems working differently with forward slashes and backslashes.

So we'll pass path.join into the express.static method.

And then we'll path the folder information into path.join. The first parameter to use is a native Node variable __dirname which contains the file path of the current folder. The second parameter will be the name of the folder containing the static resources, in our case public.

All together it looks like this.
```js
    app.use(express.static(path.join(__dirname, 'public')));
```
This will tell Express to match any routes for files found in this folder and deliver the files directly to the browser. This should be done before any other routes are defined and before the server is set up to listen.

Here's the app.js file now:
```js
    var express = require('express');
    var path = require('path');
    var app = express();
    app.use(express.static(path.join(__dirname, 'public')));
    // Define the port to run on
    app.listen(3000);
```
When Express receives a request for a route it is now checking to see whether such a file path exists in the static directory we defined. If it does it delivers this directly to the browser with no need for us to add in any routes.

## Defining a subset of routes
You can define that the static files should be found under a particular folder, by specifying the name of the path before the location of the files.

To only try and match routes that start with /public update the line to:

app.use('/public', express.static(path.join(__dirname + '/public')));
Head to http://localhost:3000/public/index.html or http://localhost:3000/public/ this time to see the page.

Program to serve a static html file:
```js
    var express = require("express");
    var path = require("path");
    var app = express();
    app.use(express.static(process.argv[3]||path.join(__dirname,"public")));
    //casting process.argv[2]
    app.listen(Number(process.argv[2]));
```

## Using template engine with Express

A template engine enables you to use static template files in your application. At runtime, the template engine replaces variables in a template file with actual values, and transforms the template into an HTML file sent to the client. This approach makes it easier to design an HTML page.

Some popular template engines that work with Express are Pug, Mustache, and EJS. The Express application generator uses Jade as its default, but it also supports several others.

See Template Engines (Express wiki) for a list of template engines you can use with Express. See also Comparing JavaScript Templating Engines: Jade, Mustache, Dust and More.

Note: Jade has been renamed to Pug. You can continue to use Jade in your app, and it will work just fine. However if you want the latest updates to the template engine, you must replace Jade with Pug in your app.
To render template files, set the following application setting properties, set in app.js in the default app created by the generator:

views, the directory where the template files are located. Eg: app.set('views', './views'). This defaults to the views directory in the application root directory.
view engine, the template engine to use. For example, to use the Pug template engine: app.set('view engine', 'pug').
Then install the corresponding template engine npm package; for example to install Pug:

`$ npm install pug --save`
Express-compliant template engines such as Jade and Pug export a function named __express(filePath, options, callback), which is called by the res.render() function to render the template code.

Some template engines do not follow this convention. The Consolidate.js library follows this convention by mapping all of the popular Node.js template engines, and therefore works seamlessly within Express.
After the view engine is set, you don’t have to specify the engine or load the template engine module in your app; Express loads the module internally, as shown below (for the above example).

app.set('view engine', 'pug');
Create a Pug template file named index.pug in the views directory, with the following content:

    html
      head
        title= title
      body
        h1= message
Then create a route to render the index.pug file. If the view engine property is not set, you must specify the extension of the view file. Otherwise, you can omit it.
```js
    app.get('/', function (req, res) {
      // If view engine is now set -
      // res.render('index.pug', { title: 'Hey', message: 'Hello there!'});
      res.render('index', { title: 'Hey', message: 'Hello there!'});
    });
```
When you make a request to the home page, the index.pug file will be rendered as HTML.

Program to create an Express.js app with a home page rendered by Jade template engine.
The homepage should respond to /home.
The view should show the current date using toDateString.

``` js 
    var exp = require("express");
    var path = require("path");
    var app = exp();
    app.set("views", process.argv[3]||path.join(__dirname, "templates"));
    app.set("view engine", 'jade');
    app.get("/home", function(req, res) {
         res.render('index', {date: new Date().toDateString()});
    });
    app.listen(process.argv[2]);
```

``` jade
    // index.jade (templates/index.jade)
    h1 Hello World
    p1 Today is #{date}.
```

## Handing post request

To handle POST request use the post() method which is used the same way as get():

    app.post('/path', function(req, res){...})

Express.js uses middleware to provide extra functionality to your web server.

Simply put, a middleware is a function invoked by Express.js before your own
request handler.

Middlewares provide a large variety of functionalities such as logging, serving
static files and error handling.

A middleware is added by calling use() on the application and passing the
middleware as a parameter.

To parse x-www-form-urlencoded request bodies Express.js can use urlencoded()
middleware from the body-parser module.

``` js 
    bodyParser.urlencoded(options)
```

Returns middleware that only parses urlencoded bodies. This parser accepts only UTF-8 encoding of the body and supports automatic inflation of gzip and deflate encodings.

A new body object containing the parsed data is populated on the request object after the middleware (i.e. req.body). This object will contain key-value pairs, where the value can be a string or array (when extended is false), or any type (when extended is true).

Program to write a route ('/form') that processes HTML form input (\<form>\<input name="str"/>\</form>) and prints backwards the str value:

``` js
    var exp = require("express");
    var bodyParser = require("body-parser");
    var app = exp();
    app.use(bodyParser.urlencoded({extended: false}));
    app.post("/form", function(req, res) {
        res.send(req.body.str.split("").reverse().join(""));
    });
    app.listen(process.argv[2]);
```

## Using stylus middleware

To plug-in stylus someone can use this middleware:

    app.use(require('stylus').middleware(__dirname + '/public'));

Program to style your HTML from previous example with some Stylus middleware.

Your solution must listen on the port number supplied by process.argv[2].

The path containing the HTML and Stylus files is provided in process.argv[3]
(they are in the same directory). You can create your own folder and use these:

The main.styl file:

    p
      color red

The index.html file:

    <html>
      <head>
        <title>expressworks</title>
        <link rel="stylesheet" type="text/css" href="/main.css"/>
      </head>
      <body>
        <p>I am red!</p>
      </body>
    </html>
    
``` js
    var exp = require("express");
    var path = require("path");
    var stylus = require("stylus");
    var app = exp();
    app.use(stylus.middleware(path.join(__dirname, "public")));
    app.use(exp.static(path.join(__dirname, "public")));
    app.listen(process.argv[2]);
```

## Handling PUT request

Program to create an Express.js server that processes PUT '/message/:id' requests.

For instance:

    PUT /message/526aa677a8ceb64569c9d4fb

As a response to these requests, return the SHA1 hash of the current date
plus the sent ID:

``` js
    require('crypto')
      .createHash('sha1')
      .update(new Date().toDateString() + id)
      .digest('hex')
```

To handle PUT requests use:

    app.put('/path/:NAME', function(req, res){...});

To extract parameters from within the request handlers, use:

    req.params.NAME
    
Program: 

``` js
    var exp = require("express");
    var app = exp();
    app.put("/message/:id", function(req, res) {
        var hash = require("crypto")
            .createHash('sha1')
            .update(new Date().toDateString() + req.params.id)
            // set hex encoding
            .digest('hex');
        res.send(hash);
    });
    app.listen(process.argv[2]);
```

**Find out : When to use put and post request and how idempotance decides it?**

## Extracting data from url queries

In Express.js to extract query string parameters, we can use:

    req.query.NAME

To output JSON we can use:

    res.send(object)
    
Program to create a route that extracts data from query string in the GET '/search' URL
route, e.g. `?results=recent&include_tabs=true` and then outputs it back to
the user in JSON format.

``` js
    var exp = require('express');
    var app = exp();
    app.get("/search", function(req, res) {
        var json = JSON.stringify(req.query);
        res.send(json);
    });
    app.listen(process.argv[2]);
```

## Parsing file into JSON


For reading, there's an fs module, e.g.,

    fs.readFile(filename, callback)

While the parsing can be done with JSON.parse:

    object = JSON.parse(string)

Program to create a server that reads a file, parses it to JSON and outputs the content
to the user.

The port is passed in process.argv[2].  The file name is passed in process.argv[3].

Respond with:

    res.json(object)

Everything should match the '/books' resource path.

Program code:

``` js
    var exp = require('express');
    var fs = require("fs");
    var app = exp();
    app.get("/books", function(req, res) {
        fs.readFile(process.argv[3], "utf8", function(err, data) {
            if (err) return res.send(500)
            try {
              var json = JSON.parse(data);
            } catch (error) {
              console.error(error);
              res.send(500);
            }
            var json = JSON.parse(data);
            res.send(json);    
        });
    });
    app.listen(process.argv[2]);
```











