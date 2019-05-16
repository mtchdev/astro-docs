# Models
Models are a way of *modelling dynamic data* to be stored in a database with relation to that model. For example, a 'users' table in the database would correspond with a 'User' model in the server. Controllers use these models to communicate with the respective database table.  

## Creating a Model
Astro has it's own model generation command bundled within an NPM script. Run this command to easily generate a model:  

`npm run generate:model`  

and follow the instructions to create the model:  
`Model Name:` – The name of the model, which will be used as the file & class name. This is commonly done using the following conventions: "User".  
`Table:` – The database table related to the model. For example, "users".

## Model Variables
In each model, there are **3 variables** which are used to define specifically what the table columns are, and what the model can & can't return:  

`this.table` (string) – The table related to the model, set automatically by the generator.  
`this.fillable` (array) – The rows that can be updated/inserted to. This **must be populated** in order for the `Model.save()` method to work.  
`this.noReturn` (array) – Defines what the model shouldn't return from the database. This is useful for protecting private data from custom API calls.  

### Entity Variables
For the model to be used as an entity, you must create a `public` variable in the class for each corresponding database column. For example:
```ts
public username: string;
public email: string;
public password: string;
public id: number;
```   
These variables are used in multiple occasions to assist the database.  

## Database Methods
Models are the *communicators* between the controller and the database. Astro provides a powerful built-in database toolset ready for use. The model, as a class, must be instantiated with `var model = new Model();` to be used: 

`Model.all()` – Gets all rows from a table.  
`Model.where({}: any)` – Gets rows that match certain conditions.  
`Model.update({values:{}, where?:{}}: any)` – Update values in a table. You can specify a "WHERE" to update specific rows.  
`Model.insert([{key: string, value: string}])` – Manually insert a new row into the database.  
`Model.save()` – Saves a new model instance to the database.  

All methods return a type `Promise<QueryResult>`, where `QueryResult` is an object:
```ts
result: {
    // returned values
}
```  
### All
```ts
var result = new User().all();
```

### Where
```ts
var result = new User().where({
    username: 'John Doe'
});
```

### Insert
```ts
new User().insert({
    username: 'John Doe'
});
```

### Update
```ts
new User().update({
    values: {
        username: 'Michael'
    },
    where: {
        id: 1
    }
});
```

### Using the .save() Method
The save method is used to save the model instance values in the database. For example, when creating a new user, you would do the following:  
```ts
var user = new User();
user.username = 'John Doe';
user.email = 'example@example.com';
user.password = 'Password123';
user.save();
```
Each **column** must be set in the new instance to be saved. The `fillable[]` property must also contain the same columns in the array for this method to function correctly.  

### API
```ts
interface SQLQueryModel {
    where(params: any): Promise<QueryResult>;
    all(): Promise<QueryResult>;
    insert(params: Insert[]): Promise<QueryResult>;
    update(params: Update): Promise<QueryResult>;
}
```

### Next: [Controllers](https://spliitzx.github.io/astro-docs/controllers)