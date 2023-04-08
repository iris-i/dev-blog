---
title: 'Custom authentication for self-hosted TinaCMS'
status: 'draft'
author:
  name: 'Iris Ibekwe'
  picture: 'https://avatars.githubusercontent.com/u/5355315?v=4'
slug: 'custom-authentication-for-self-hosted-tinacms'
description: ''
coverImage: ''
publishedAt: '2023-04-08T14:39:11.217Z'
---

Install nextauth `yarn add next-auth`

Follow the steps to

- Add SessionProvider to app.jsx

- Add oauth providers

- In the [...nextauth].js enable jwt then add the accessToken to the user session object. Make sure that you have a value for NEXTAUTH\_SECRET which enables JWT to work.

```javascript
,
  session: {
    strategy: "jwt"
  },
  callbacks: {
    async jwt({ token, account }) {
      if (account) {
        token.accessToken = account.access_token;
      }
      return token;
    },
    async session({ session, token, user }) {
      session.accessToken = token.accessToken;
      return session;
    },
  },
```

- 

- In config.tsx, add custom overrides for t login, logout, getUser and getToken functions used by tinacms.

```javascript
const getServerSideProps = async () => {
  const res = await fetch(`/api/auth/session`)
  return res.json();
}

const config = defineConfig({
  contentApiUrlOverride: '/api/gql',
  admin: {
    auth: {
      // useLocalAuth: true,
      // process.env.TINA_PUBLIC_IS_LOCAL == 'true',
      customAuth: true,
      authenticate: async () => {
        window.location.href = '/api/auth/signin?callbackUrl=/admin'

      },
      getToken: async () => {
        let sessionData = await getServerSideProps();
        return sessionData?.accessToken ? { id_token: sessionData.accessToken } : '';
      },
      getUser: async () => {
        let sessionData = await getServerSideProps();
        return sessionData?.user;
      },
      logout: async () => {
        window.location.href = '/api/auth/signout?callbackUrl=/'
      },
    },
  },
```

in /api/gql check if the user is logged in

```javascript
import { NextApiHandler } from "next";
import { databaseRequest } from "../../lib/databaseConnection";
import { getServerSession } from "next-auth/next"
import { authOptions } from "./auth/[...nextauth]"


const nextApiHandler: NextApiHandler = async (req, res) => {

  const session = await getServerSession(req, res, authOptions)
  const userHasValidSession = Boolean(session)

  const isAuthorized =
    process.env.TINA_PUBLIC_IS_LOCAL === 'true'
    || userHasValidSession === true
    || false

  if (isAuthorized) {
    const { query, variables } = req.body;
    const result = await databaseRequest({ query, variables });

    return res.json(result);
  } else {
    return res.status(401).json({ error: "Unauthorized" });
  }
};

export default nextApiHandler;
```



