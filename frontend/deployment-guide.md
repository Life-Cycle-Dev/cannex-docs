---
icon: code
---

# Deployment Guide

## Hardware and software requirements <a href="#hardware-and-software-requirements" id="hardware-and-software-requirements"></a>

* [Node.js](https://nodejs.org/): Only [Active LTS or Maintenance LTS versions](https://nodejs.org/en/about/previous-releases) are supported (currently `v20` and `v22`). Odd-number releases of Node, known as "current" versions of Node.js, are not supported (e.g. v21, v23).
* Your preferred Node.js package manager:
  * [npm](https://docs.npmjs.com/cli/v6/commands/npm-install) (`v6` and above)
  * [yarn](https://yarnpkg.com/getting-started/install)
  * [pnpm](https://pnpm.io/)

## Environment Variable <a href="#environment-variable" id="environment-variable"></a>

### Application Configuration

<table><thead><tr><th width="211.23828125">Key</th><th>Description</th></tr></thead><tbody><tr><td>NEXT_PUBLIC_BACKEND_PATH</td><td>Backend endpoint path Example: <code>https://api.cannex.com</code></td></tr><tr><td>NEXT_PUBLIC_FRONTEND_PATH</td><td>Frontend endpoint path Example: <code>https://cannex.com</code></td></tr><tr><td>NEXT_PUBLIC_API_KEY</td><td>API Access Data from backend (follow by <a href="https://docs.strapi.io/cms/features/api-tokens">https://docs.strapi.io/cms/features/api-tokens</a> docs) </td></tr></tbody></table>



## Deploy with node.js runtime

You need to install [Node.js](https://nodejs.org/) in Virtual Machine and set your enivironment variable in Virtual Machine or create `.env` in root project path

* Install dependencies

```
1: 📄 yarn install

-- or --

1: 📄 npm install
```

* Build your project

```
1: 📄 yarn build

-- or --

1: 📄 npm run build
```

* Start your project

```
1: 📄 yarn start

-- or --

1: 📄 npm start
```



## Deploy with docker

You need to install docker service in Virtual Machine

* Create `.env`  in root project path and set your environment variable
* Build your image

```
1: 📄 docker build -t cannex-frontend .
```

* Start the container

```
1: 📄 docker run -d \
    --name cannex-frontend \
    --env-file .env \
      -p 1337:1337 \
      cannex-frontend
```

## Deploy with docker compose

You need to install docker and docker compose service in Virtual Machine

* Create `.env`  in root project path and set your environment variable
* Build and start  container

```
1: 📄 docker compose up -d
```
