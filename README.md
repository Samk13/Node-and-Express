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

Let's create a simple API by creating a route that responds with JSON at the path /json. You can do it as usual, with the app.get()method. Inside the route handler use the method res.json(), passing in an object as an argument. This method closes the request-response loop, returning the data. Behind the scenes it converts a valid JavaScript object into a string, then sets the appropriate headers to tell your browser that you are serving JSON, and sends the data back. A valid object has the usual structure {key: data}. Data can ba a number, a string, a nested object or an array. Data can also be a variable or the result of a function call; in which case it will be evaluated before being converted into a string.

Serve the object ```json {"message": "Hello json"}```as a response in JSON format, to the GET requests to the route /json. Then point your browser to your-app-url/json, you should see the message on the screen.
