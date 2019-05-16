# Controllers
In Astro, controllers are used to actually execute the functions once the route and middleware have passed. It's where all the logic is stored.

## Creating a Controller
Like models, Astro has it's own component generation command bundled within an NPM script. Run this command to easily generate a model:  

`npm run generate:controller`  

and follow the instructions to create the controller:  
`Controller Name:` – The name of the controller, which will be used as the file & class name. This is commonly done using the following conventions: "UserController".  

## Generic Conventions
To make your controller more efficient, you should prefix your methods with `async` to avoid promise hell. For example:
```ts
async getUsers() {
    var result = await new User().all();

    // return result
}
```  

## Controller API
The controller class has many helper methods, with custom response types and dynamic variables.  

### State
The state is a global dynamic object instance which can be used to pass variables between different methods, controllers, or models without actually having to pass data as parameters between them.  

The **state variable** can be accessed through `this.state`, and `this.state.property`.  

### Adding Properties to State
Adding state properties is easy, and in a controller can be done like so:  

```ts
this.setState({
    newObj: newVal
});
```
You **do not** need to spread the existing state, as it is automatically done in the parent class.  

### Updating State
Just like any object in JavaScript, you can update state by changing the property itself:  

```ts
this.state.property = newVal;
```  

#

### JSON Return Types
Astro has built-in response methods so you don't have to configure different responses (although you can, if you want to):  

`this.respondWithSuccess(message?: ResponseTemplate)` – Respond in JSON format with a success message, code, and custom data.  
`this.respondWithError(message?: ResponseTemplate)` – Respond in JSON format with an error code, custom message, or data.  

If you pass **only 1 argument** to the response, it will be treated as part of the "data" response object, and returned as follows:
```json
{
    "message": "success",
    "code": 200,
    "data": {
        // result
    }
}
```  
The 'message' parameter interface:  
```ts
interface ResponseTemplate {
    status?: number,
    message?: string,
    data?: any
};
```
**Defaults**  
If not provided, success responses will default to a code of `200`, and a message `"success"`. Data will be `null`, if not given.  
Similarly, error responses default with code `500` and message `"error"`. Data will also be `null`, if not provided.  

## Sample Controller
Sample controller with usage of models and response templates:  
```ts
export class UserController extends Controller {
    constructor(data) {
        super(data);
    }

    async getUsers() {
        var users = await new User().all();
        return this.respondWithSuccess(users);
    }
}
```