# Routing
Astro uses the [Express.js](https://expressjs.com/) library to power it's robust HTTP and routing systems. Every request is permitted, with the ability to add multiple middleware and even IP address restrictions.
  
## Config
`config/router.config.ts`  
Modify custom options to the router.  

`api_prefix` (string): The prefix to use when using the api.ts router. For example: example.org/**api**/users  
`allowed_ips` (array): An array of IP addresses to accept on each request. If this is empty, there will be no IP checks.  

## Basic Routing
The router uses the custom Astro HTTP library to give more powerful features to that of Express. Currently, **only the API** router is used as Astro does not/does not plan to introduce views. The sole purpose is to provide a fast and efficient way to communicate between the client and the database.  
API router template:
```js
import { Http } from 'vendor/astro/router/http';
const http = new Http();

/**
 * Place your routes below. Example is provided using the Route provider
 */

import { UserController } from 'app/controllers/UserController';
import { AuthMiddleware } from 'app/middleware/AuthMiddleware';

http.get('users', (req: any, res: any) => new UserController(res).getUsers(), [AuthMiddleware]);
```

### HTTP API
HTTP is a class, therefore you must initialize a new `http` instance by instantiating a new class instance: `new Http();`  

`http.get(url: string, callback: any, middleware?: any)`  
`url`: the URL string to match.  
`callback`: the callback function from the router. The callback returns both the *request* and *response* instances, which you must pass thorugh the callback in order to power the controllers.  
`middleware?`*: The middleware parameter can take
 either a single class, or an array of middleware classes. The middleware is run before the callback is called.  

### Request Types
| Request       | Method        |
| ------------- |:-------------:|
| GET      | `http.get()` | $1600 |
| POST     | `http.post()`      |
| PUT | `http.put()`      |
| DELETE | `http.delete()`      |

### Calling Controllers
Calling controllers in a route is easy. You must call it in the callback, and pass the `Response` to the controller (as seen in the controller constructor). For example:  
```js
http.get('users', (request: any, response: any) => new UserController(response).getUsers());
```
This will allow you to access the response object globally within the controller. Each **route** must call a **method** inside the desired controller.  

### Controllers With Input
Just as a normal Express route, you can pass the `Request` object to the **method** that you're calling:
```js
http.post('user', (request: any, response: any) => new UserController(response).addUser(request));
```
You can access form input data through the `request.input` object.  

### Route Parameters
Similar to [Controllers With Input](#controllers-with-input), you can pass parameters in the URL and access them in the controller, as done in Express.
```js
http.get('user/:username', (request: any, response: any) => new UserController(response).getUser(request));
```
You can access the URL parameters using the `request.params` object.  

## Handling Route Errors

### 404 (Not Found)
Handling a simple **404 Not Found** error in Astro is easy. At the bottom of all your routes, add a simple wildcard route checker to match non-existent routes:
```js
http.get('*', (req: any, res: any) => {
    res.status(404).send('Resource not found.');
});
```
This methodology will be improved in the future, but for now this remains the only way.