---
title: '2. Self-host TinaCMS: Install NextJS & TinaCMS'
status: 'draft'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'self-host-tinacms'
description: ''
coverImage: '/images/screen-shot-2023-04-08-at-11.32.42-pm-U3Mz.png'
publishedAt: '2023-04-09T02:32:13.340Z'
---

Content is pushed to your personal DataLayer instead of TinaCloud

TinaCMS has.a datalayer which prevents you frpm fetching data from github each time you update your content.

The Data layer:

Database.ts has to Leveldb implementations. This saves the data in the file system locally or mongodb in production. Also responsible for indexing the leveldb from the file systemwhen you're in dev mode/build.

Also has onPut & onDelete functions which save to github when you are not running locally or file system.

There is an api endpoint in the data layout, api/gql.ts, which uses the database to query and mutate the data if it changes. It will update levelDB and call the onPut/onDelete functions.

When you're using getStaticProps/getStaticPaths you won't have access to api/gql at build time. A dbConnection.ts file uses the db directly so it can be used in the backend. getStaticprops uses the dbConnection.



Pre-requisites:

Mongodb



1. Install NextJS `npx create-next-app@latest`

2. Install TinaCMS `npx @tinacms/cli@latest init`

3. Update the tinaconfig.ts file to point to an api for content updates instead of TinaCloud. By doing this you no longer need to create a project in TinaCloud and get a client id & token to use TinaCMS.

```javascript
// ./tina/config.ts
const config = defineConfig({
  contentApiUrlOverride: '/api/gql',
```



Setup Database connections:



<br>

 <br>







