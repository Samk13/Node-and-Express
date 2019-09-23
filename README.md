# Backend Challenges Boilerplate - Basic Express


Serve Static Assets
An HTML server usually has one or more directories that are accessible by the user. You can place there the static assets needed by your application (stylesheets, scripts, images). In Express you can put in place this functionality using the middleware express.static(path), where the parameter is the absolute path of the folder containing the assets. If you don’t know what a middleware is, don’t worry. We’ll discuss about it later in details. Basically middlewares are functions that intercept route handlers, adding some kind of information. A middleware needs to be mounted using the method app.use(path, middlewareFunction). The first path argument is optional. If you don’t pass it, the middleware will be executed for all the requests.
Mount the express.static()middleware for all the requests with app.use(). The absolute path to the assets folder is __dirname + /public.
Now your app should be able to serve a CSS stylesheet. From outside the public folder will appear mounted to the root directory. Your front-page should look a little better now!

```
app.use("/",  express.static(__dirname + "/public"))
```
