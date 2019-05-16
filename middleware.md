# Middleware
In Astro (and most web development scenarios), middleware is the final security layer between the router and the controller, if you choose to utilize it. Middleware is typically used to validate & authenticate token headers, permissions, etc.  

## Creating Middleware
Use the provided **NPM script** to create a new middleware:  

`npm run generate:middleware`  

You will be prompted to enter a name for your middleware, which is used for the class & file name.  

## Writing Middleware
Once you've made your [middleware](#creating-middleware), you can find it in `app/middleware` in a pre-set template made by the generator. This is where the middleware code will go, and terminate after pass and fails.  

### API
Middleware is automatically executed on the `run()` method. **Do not** call this in the constructor.

### Base Template
```ts
import { Middleware } from 'vendor/astro/http/Middleware';

export class MiddlewareName extends Middleware {

    constructor(private request, data) {
        super(data);
    }

    run() {
        return this.next();
    }

}
```

### Constructor Variables
`request` (private): Accessed with `this.request` and is the HTTP request object from the router. This object holds parameters, input, and much more. Read more at [Express.js](https://expressjs.com/en/api.html#req).  

### Communicating with the Database
Like controllers, you should use models to communicate with the database. In the future, you will be able to manually query the database. If you want to do this right now, read the [npm mysql package](https://www.npmjs.com/package/mysql) documentation.  

## Pass & Fail
When a middleware is **passed**, it will validate as true and continue. If you use multiple middlewares, it will continue to them until it passes to the controler.  
When **failed**, the entire route process will terminate and the end-user will be returned an error `403 Unauthorized` and denied access to the resource. No further middleware will be executed.  

**Pass**  
```ts
this.next();
```  

**Fail**
```ts
this.exit();
```

## Next: [Database Schemas]()