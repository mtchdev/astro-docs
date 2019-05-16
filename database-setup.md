# Database Configuration
Astro supports many database systems out of the box, ultimately using SQL as the main communication platform.  

## Configuration
You must setup your database configuration in `config/database.config.ts`. Most of the fields are self explanatory:
```json
export const DBConfig = {
    mysql: {
        host: 'localhost',
        port: 8889,
        user: 'root',
        password: 'root',
        database: 'astro'
    }
}
```
`host`: The database host server. For most local development environments, you would set this to "localhost".  
`port`: The database port. This typically runs on '3306'.  
`user`: The username used to authenticate with the database.  
`password`: The password used to authenticate with the database.  
`database`: The actual name of the database.  

When you've configured your database configuration properly, you should continue to the next step which starts introducing database usage.  

### Next: [Models](https://spliitzx.github.io/astro-docs/models)