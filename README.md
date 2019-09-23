# Backend Challenges Node & Express



# Start a Working Express Server
In the first two lines of the file myApp.js you can see how it’s easy to create an Express app object. This object has several methods, and we will learn many of them in these challenges. One fundamental method is app.listen(port). It tells your server to listen on a given port, putting it in running state. You can see it at the bottom of the file. It is inside comments because for testing reasons we need the app to be running in background. All the code that you may want to add goes between these two fundamental parts. Glitch stores the port number in the environment variable ```javascript process.env.PORT ```. Its value is 3000.
Let’s serve our first string! In Express, routes takes the following structure: app.METHOD(PATH, HANDLER). METHOD is an http method in lowercase. PATH is a relative path on the server (it can be a string, or even a regular expression). HANDLER is a function that Express calls when the route is matched.
Handlers take the form ```javascript function(req, res) {...}```, where req is the request object, and res is the response object. For example, the handler
```javascript
function(req, res) {
res.send('Response String');
}
```
will serve the string 'Response String'.
Use the app.get()method to serve the string Hello Express, to GET requests matching the / root path. Be sure that your code works by looking at the logs, then see the results in your browser, clicking the button ‘Show Live’ in the Glitch UI.

> Solution
```javascript
app.get("/", function(req, res){
  res.send("Hello Express")
})  
```

# Serve an HTML File
We can respond with a file using the method res.sendFile(path).
You can put it inside the app.get('/', ...)route handler. Behind the scenes this method will set the appropriate headers to instruct your browser on how to handle the file you want to send, according to its type. Then it will read and send the file. This method needs an absolute file path. We recommend you to use the Node global variable __dirnameto calculate the path.
e.g.
```javascript
absolutePath = __dirname + relativePath/file.ext.
```
The file to send is /views/index.html. Try to ‘Show Live’ your app, you should see a big HTML heading (and a form that we will use later…), with no style applied.
Note: You can edit the solution of the previous challenge, or create a new one. If you create a new solution, keep in mind that Express evaluates the routes from top to bottom. It executes the handler for the first match. You have to comment out the preceding solu

> Solution
```javascript
app.get("/", function(req, res){
        res.sendFile( __dirname + "/views/index.html")
        });
```
tion, or the server will keep responding with a string.


# Serve Static Assets
An HTML server usually has one or more directories that are accessible by the user. You can place there the static assets needed by your application (stylesheets, scripts, images). In Express you can put in place this functionality using the middleware express.static(path), where the parameter is the absolute path of the folder containing the assets. If you don’t know what a middleware is, don’t worry. We’ll discuss about it later in details. Basically middlewares are functions that intercept route handlers, adding some kind of information. A middleware needs to be mounted using the method app.use(path, middlewareFunction). The first path argument is optional. If you don’t pass it, the middleware will be executed for all the requests.
Mount the express.static()middleware for all the requests with app.use(). The absolute path to the assets folder is __dirname + /public.
Now your app should be able to serve a CSS stylesheet. From outside the public folder will appear mounted to the root directory. Your front-page should look a little better now!

> Solution

```javascript
app.use("/",  express.static(__dirname + "/public"))
```


Serve JSON on a Specific Route
While an HTML server serves (you guessed it!) HTML, an API serves data. A REST (REpresentational State Transfer) API allows data exchange in a simple way, without the need for clients to know any detail about the server. The client only needs to know where the resource is (the URL), and the action it wants to perform on it (the verb). The GET verb is used when you are fetching some information, without modifying anything. These days, the preferred data format for moving information around the web is JSON. Simply put, JSON is a convenient way to represent a JavaScript object as a string, so it can be easily transmitted.

Let's create a simple API by creating a route that responds with JSON at the path /json. You can do it as usual, with the app.get()method. Inside the route handler use the method 
```javascript
res.json() 
```
, passing in an object as an argument. This method closes the request-response loop, returning the data. Behind the scenes it converts a valid JavaScript object into a string, then sets the appropriate headers to tell your browser that you are serving JSON, and sends the data back. A valid object has the usual structure ``` {key: data} ``` . Data can ba a number, a string, a nested object or an array. Data can also be a variable or the result of a function call; in which case it will be evaluated before being converted into a string.

Serve the object
```javascript
{"message": "Hello json"}
```
as a response in JSON format, to the GET requests to the route /json. Then point your browser to your-app-url/json, you should see the message on the screen.
> Solution
```javascript
app.get("/json", function(req, res){
    res.json({"message": "Hello json"})
})
```

# Use the .env File
The **.envfile** is a hidden file that is used to pass environment variables to your application. This file is secret, no one but you can access it, and it can be used to store data that you want to keep private or hidden. For example, you can store API keys from external services or your database URI. You can also use it to store configuration options. By setting configuration options, you can change the behavior of your application, without the need to rewrite some code.

The environment variables are accessible from the app as **process.env.VAR_NAME**. The process.envobject is a global Node object, and variables are passed as strings. By convention, the variable names are all uppercase, with words separated by an underscore. The .envis a shell file, so you don’t need to wrap names or values in quotes. It is also important to note that there cannot be space around the equals sign when you are assigning values to your variables, e.g. VAR_NAME=value. Usually, you will put each variable definition on a separate line.

Let's add an environment variable as a configuration option. Store the variable MESSAGE_STYLE=uppercasein the .envfile. Then tell the GET /jsonroute handler that you created in the last challenge to transform the response object’s message to uppercase if ``` process.env.MESSAGE_STYLEequals uppercase ``` . The response object should become 
```javascript
{"message": "HELLO JSON"}
```
> Solution 

```javascript
app.get("/json", (req, res) => { let message = "Hello json"; 
(process.env.MESSAGE_STYLE == "uppercase") ? message=message.toUpperCase()
: message=message; res.json({"message": message}); });
```


# Implement a Root-Level Request Logger Middleware
Before we introduced the express.static()middleware function. Now it’s time to see what middleware is, in more detail. Middleware functions are functions that take 3 arguments: the request object, the response object, and the next function in the application’s request-response cycle. These functions execute some code that can have side effects on the app, and usually add informations to the request or response objects. They can also end the cycle sending the response, when some condition is met. If they don’t send the response, when they are done they start the execution of the next function in the stack. This is triggered calling the 3rd argument next(). More information in the express documentation.

Look at the following example :

```javascript
function(req, res, next) {
console.log("I'm a middleware...");
next();
}
```
Let’s suppose we mounted this function on a route. When a request matches the route, it displays the string “I’m a middleware…”. Then it executes the next function in the stack.

In this exercise we are going to build a root-level middleware. As we have seen in challenge 4, to mount a middleware function at root level we can use the method app.use(<mware-function>). In this case the function will be executed for all the requests, but you can also set more specific conditions. For example, if you want a function to be executed only for POST requests, you could use app.post(<mware-function>). Analogous methods exist for all the http verbs (GET, DELETE, PUT, …).

Build a simple logger. For every request, it should log in the console a string taking the following format: method path - ip. An example would look like: GET /json - ::ffff:127.0.0.1. Note that there is a space between methodand pathand that the dash separating pathand ipis surrounded by a space on either side. You can get the request method (http verb), the relative route path, and the caller’s ip from the request object, using req.method, req.pathand req.ip. Remember to call next()when you are done, or your server will be stuck forever. Be sure to have the ‘Logs’ opened, and see what happens when some request arrives…

Hint: Express evaluates functions in the order they appear in the code. This is true for middleware too. If you want it to work for all the routes, it should be mounted before them.
> Solution 

```javascript
app.use("/json", (req, res, next)=>{
  console.log(`${req.method} ${req.path} - ${req.ip}`);
  next();
  
})
```



