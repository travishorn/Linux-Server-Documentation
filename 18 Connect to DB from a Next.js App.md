# Linux Server

## Connect to MariaDB from a Next.js App

### Prequisites

- A running Next.js app
- A running MariaDB (or MySQL) instance with user access configured

### Install Dependencies

This solution uses `serverless-mysql`. Install it as a dependency.

```sh
npm install serverless-mysql
```

### Set Environment Variables

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

### Create a Database Connection File

Create a new file called `lib/db.js` in the root directory of your app.

```javascript
import dbConnection from 'serverless-mysql';

const db = dbConnection({
  config: {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    database: process.env.DB_DATABASE,
    user: process.env.DB_USER,
    password: process.env.DB_PASSWORD,
  },
});

export default async function executeQuery({ query, values }) {
  try {
    const results = await db.query(query, values);
    await db.end();
    return results;
  } catch (error) {
    return { error };
  }
};
```

We'll import this file into any file that needs to connect to the database.

### Create an API Route

Create a new file called `pages/api/message.ts`. Feel free to change "`message`"
to something more appropriate for your application.

```typescript
import type { NextApiRequest, NextApiResponse } from 'next';
import executeQuery from '../../lib/db';

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  switch (req.method) {
    case "GET":
      try {
        const result = await executeQuery({
          query: "SELECT text FROM Messages ORDER BY createdAt LIMIT 1",
          values: [],
        });
  
        res.status(200).json(result);
      } catch (err) {
        console.error(err);
      }
      break;
    case "POST":
      try {
        const result = await executeQuery({
          query: "INSERT INTO Messages (text) VALUES (?)",
          values: [req.body.text],
        });
  
        res.status(200).json(result);
      } catch (err) {
        console.error(err);
      }
      break;
    default:
      res.status(405).end();
  }
};
```

The request methods and queries will need to be changed to match your
application.

At this point, (as long as the app is running) you can access the endpoint at
`http://localhost:3000/api/message`.

### Use this Endpoint on Client-side Pages

Inside a client-side page like `pages/index.tsx`, import the `useState` and
`useEffect` functions from `react`.

```typescript
import { useState, useEffect } from "react";
```

Then, inside `export default function Home() {}`, describe how to use and store
the message text.

```typescript
const [messageText, setMessageText] = useState("");

useEffect(() => {
  fetch("/api/message")
    .then(res => res.json())
    .then(data => {
      setMessageText(data[0].text);
    })
    .catch(err => {
      console.error(err);
    });
}, []);
```

Now you can display the text on the page, inside the returned TSX.

```tsx
<div>{messageText}</div>
```
