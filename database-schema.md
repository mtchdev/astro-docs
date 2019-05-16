# Database Schema/Migrations
Astro includes a very simple database migration system, where you can create/update/insert data using raw SQL syntax.  
Unlike most existing frameworks, Astro relies on `.sql` files rather than custom functions to preserve the extensive functionaliy of SQL.  

## Creating a Migration
Create a `.sql` file in `database/schema`, and write your SQL script. For example:

```sql
CREATE TABLE IF NOT EXISTS users (
    id INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,

    PRIMARY KEY (id)
)
```

## Runnning a Migration

To run your migrations, execute `npm run db:build` in the command line. This will execute every script in the schema folder, so you should use `IF NOT EXISTS` to avoid losing your data.