---
title: 'How to self-host TinaCMS'
status: 'draft'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'how-to-self-host-tinacms'
description: ''
coverImage: ''
publishedAt: '2023-04-06T12:01:42.342Z'
---

```javascript
npx create-tina-app@latest
```

Go to [https://github.com/tinacms/tina-self-hosted-demo](https://github.com/tinacms/tina-self-hosted-demo) and copy the env variables in the readme.

You will need a mngodb databse. Creae a free acount at [https://www.mongodb.com/](https://www.mongodb.com/)

Create a free shared database for testing.

To get the database URI,

- go to Database

- Click connect

- Select the "Connect to your application"

- Choose node-js and copy the connection string.

- Replace with the database user password.

Fill in the databas uri in the .env file

FOr other env variables

\-- Create a github repo

\-- copy your github suername

\-- Add your epo name

\-- Add your brnach name

\-- Generate a new personal access token in Github: [https://github.com/settings/tokensTINA\_PUBLIC\_IS\_LOCAL=true](https://github.com/settings/tokens)

```properties
TINA_PUBLIC_IS_LOCAL=true
```

THist tells you wether you want to yse the local tinacms setup or mongodb. True means you wnat to use the local version.

\--

Create a database.ts file within the .tina folder and copy over the code from [https://github.com/tinacms/tina-self-hosted-demo/blob/main/.tina/database.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/.tina/database.ts) into it

Install the dependencies in the file:

`yarn add mongodb-level @octokit/rest js-base64`



`-- `



Create a lib/databaseConnection.ts file in the root folder and copy in the code from here: [https://github.com/tinacms/tina-self-hosted-demo/blob/main/lib/databaseConnection.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/lib/databaseConnection.ts)

Create and copy this [https://github.com/tinacms/tina-self-hosted-demo/blob/main/pages/api/gql.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/pages/api/gql.ts)

In pages and templates, use the dbConnection directly instead of the tina client. This is because we don't have access to the api connection at build-time.

poststs/[filename.tsx] replace all instances of `client.queries.xxx` with `dbConnection.queries.`

Also import the dbConnection.

```typescriptreact
import { dbConnection } from "../../lib/databaseConnection";
```

Repeat in pages.tsx and posts/[filename.tsx]

Remove the unused tina client import.

Update the tina config to look at the api instead of Tina Cloud when we're are editing.



```javascript
// .tina/config.jsx
const config = defineConfig({
  contentApiUrlOverride: '/api/gql',
  admin: {
    auth: {
      useLocalAuth: true,
      // process.env.TINA_PUBLIC_IS_LOCAL == 'true',
    },
  },
  clientId: process.env.NEXT_PUBLIC_TINA_CLIENT_ID!,
```



## Authentication:



