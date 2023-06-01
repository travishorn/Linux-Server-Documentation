---
label: Connect to MariaDB from a Next.js App
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -20
---

# Connect to MariaDB from a Next.js App

## Prequisites

- A running Next.js app
- A running MariaDB (or MySQL) instance with user access configured

## Install Dependencies

This solution uses the `knex` and `mysql2` packages. Install them as
dependencies.

```sh
npm install knex mysql2
```

## Set Environment Variables

Set the following environment variables in whichever way makes sense for your
situation. If you are connecting to a development database server from a
development machine, you may create a file called `.env.local` in the root
directory of your app.

```
DB_HOST= 127.0.0.1
DB_PORT= 3306
DB_DATABASE= [database name]
DB_USER= [database user name]
DB_PASSWORD= [database user password]
```

You may need to change the host or the port depending on your setup.

## Create a Database Connection File

Create a new file called `lib/db.ts`.

```javascript
import knex from "knex";

export const db = knex({
  client: "mysql2",
  connection: {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
  },
});
```

We'll import this file into any file that needs to connect to the database.

## Get Data from the Database Server-Side

Inside a Next.js page like `pages/index.tsx`, import the `db` function from the
file we just created..

```typescript
import { db } from "../lib/db";
```

Then create a `getServerSideProps()` function. Next.js will use this to get data
server-side and pass them as props to the page.

```typescript
export async function getServerSideProps() {
  const message = await db("Message").first("text").where({ id: 1 });

  return {
    props: { message },
  };
};
```

Now, use the prop in the page's render function.

```typescript
export default function Home({ message }) {
  return (
    <main>
      <h1>{message.text}</h1>
    </main>
  )
}
```

## Using a Raw Query

Knex has a built in `raw()` function that allows you to execute raw SQL.
Unfortunately, the format of the data returned by this function doesn't work
well with Next.js.

However, we can write a simple wrapper around it to help us.

Add the following to `lib/db.ts`.

```typescript
export const raw = async (sql: string, bindings?: any) => {
  const result = await db.raw(sql, bindings);
  const data = result[0];
  const parsed = JSON.parse(JSON.stringify(data));
  return parsed.length === 1 ? parsed[0] : parsed;
};
```

Now on your page, you can import this function.

```tsx
import { db, raw } from "../lib/db";
```

And you can use it to execute raw queries in `getServerSideProps()`.

```tsx
const message = await raw("SELECT text FROM Message WHERE id = 1 LIMIT 1;");
```
