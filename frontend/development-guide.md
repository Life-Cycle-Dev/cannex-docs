---
icon: bolt
---

# Development Guide

## Dependencies

* [Node.js](https://nodejs.org/)
* Your preferred Node.js package manager:
  * [npm](https://docs.npmjs.com/cli/v6/commands/npm-install) (`v6` and above)
  * [yarn](https://yarnpkg.com/getting-started/install)
  * [pnpm](https://pnpm.io/)

## Setup project

*   Install node

    ```
    1: 📄 nvm install v20
    2: 📄 nvm alias default 20
    ```
*   Install node dependencies

    ```
    1: 📄 yarn install
    ```
* Create `.env`  by using value in `example.env` (If you use localhost backend path you need to start the backend service in local)
* You need to set `NEXT_PUBLIC_API_KEY` in backend follow by [https://docs.strapi.io/cms/features/api-tokens](https://docs.strapi.io/cms/features/api-tokens)

```
NEXT_PUBLIC_BACKEND_PATH=http://localhost:1337
NEXT_PUBLIC_FRONTEND_PATH=http://localhost:3000
NEXT_PUBLIC_API_KEY=tHiSiSeCrEt
```

* Start your project

```
1: 📄 npm run dev
```

* Go to [http://localhost:3000](http://localhost:3000/) and set up your local strapi account

