---
title: 'How to self-host TinaCMS'
status: 'published'
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

Go to [https://github.com/tinacms/tina-self-hosted-demo](https://github.com/tinacms/tina-self-hosted-demo) and copy the env variables in the readme. yest

You will need a mongodb database. Create a free account at [https://www.mongodb.com/](https://www.mongodb.com/)

Create a free shared database for testing.

To get the database URI,

- go to Database

- Click connect

- Select the "Connect to your application"

- Choose node-js and copy the connection string.

- Replace with the database user password.

Fill in the database uri in the .env file

FOr other env variables

\-- Create a github repo

\-- copy your github suername

\-- Add your epo name

\-- Add your brnach name

\-- Generate a new personal access token in Github: [https://github.com/settings/tokens](https://github.com/settings/tokens)

```properties
TINA_PUBLIC_IS_LOCAL=true
```

This tells you whether you want to use the local tinacms setup or mongodb. True means you want to use the local version.

\--

Create a database.ts file within the tina folder and copy over the code from [https://github.com/tinacms/tina-self-hosted-demo/blob/main/.tina/database.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/.tina/database.ts) into it

Install the dependencies in the file:

`yarn add mongodb-level @octokit/rest js-base64`

Expected diff: [https://github.com/iris-i/tina-self-host/commit/d2700c082e5670d135b98050e4abb2d90fcf85b0](https://github.com/iris-i/tina-self-host/commit/d2700c082e5670d135b98050e4abb2d90fcf85b0)

`--`

Create a lib/databaseConnection.ts file in the root folder and copy in the code from here: [https://github.com/tinacms/tina-self-hosted-demo/blob/main/lib/databaseConnection.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/lib/databaseConnection.ts)

Create and copy this [https://github.com/tinacms/tina-self-hosted-demo/blob/main/pages/api/gql.ts](https://github.com/tinacms/tina-self-hosted-demo/blob/main/pages/api/gql.ts)

In pages and templates, use the dbConnection directly instead of the tina client. This is because we don't have access to the api connection at build-time.

posts/[filename.tsx] replace all instances of `client.queries.xxx` with `dbConnection.queries.`

Also import the dbConnection.

```typescriptreact
import { dbConnection } from "../../lib/databaseConnection";
```

Repeat in pages.tsx and posts/[filename.tsx]

Remove the unused tina client import.

Update the tina config to look at the api instead of Tina Cloud when we're are editing.

```javascript
\// .tina/config.jsx
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

Run yarn dev<br>

Your github repo will now have new commits with any changes you make in the /admin page.

Your mongodb will also have the content schema and key-value pairs.

## Authentication:

