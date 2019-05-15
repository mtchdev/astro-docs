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