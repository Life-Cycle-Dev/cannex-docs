---
icon: bolt
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Development Guide

## Dependencies

* [Node.js](https://nodejs.org/): Only [Active LTS or Maintenance LTS versions](https://nodejs.org/en/about/previous-releases) are supported (currently `v20` and `v22`). Odd-number releases of Node, known as "current" versions of Node.js, are not supported (e.g. v21, v23).
* Your preferred Node.js package manager:
  * [npm](https://docs.npmjs.com/cli/v6/commands/npm-install) (`v6` and above)
  * [yarn](https://yarnpkg.com/getting-started/install)
  * [pnpm](https://pnpm.io/)

## **Databases installation**

This project required [PostgreSQL](https://www.postgresql.org/) for store your data&#x20;

## **Storage installation**

This project required S3 Storage Object like [R2](https://www.cloudflare.com/developer-platform/products/r2/), or [AWS S3](https://aws.amazon.com/s3/) for store your media file&#x20;

## Setup project

* Install postgresql

```
1: 📄 brew install postgresql@14
2: 📄 echo 'export PATH="/usr/local/opt/postgresql@14/bin:$PATH"' >> ~/.bash_profile
      echo 'export PATH="/usr/local/opt/postgresql@14/bin:$PATH"' >> ~/.zshrc
3: 📄 brew services start postgresql@14
4: 📄 brew services list
```

* 📄 Make sure your PostgreSQL is started
* Install nvm for node.js

```
1: 📄 brew install nvm
2: 📄 export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  

```

* Create your database name `cannex`&#x20;

```
1: 📄 psql --version
2: 📄 createdb cannex
3: 📄 psql -l
```

* Create database account `postgres`

```
1: 📄 psql -U <your_or_username> -d cannex
2: 📄 CREATE ROLE postgres WITH LOGIN SUPERUSER PASSWORD 'postgres';
```

* If you set follow by document you can use database environment follow by `example.env`

```sh
# Database
DATABASE_CLIENT=postgres
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=
DATABASE_USERNAME=root
DATABASE_PASSWORD=root
DATABASE_SSL=false
```

* Set up S3 Storage in your local&#x20;

<pre><code>
<strong>1: 📄  docker run -d --name minio \
</strong>    -p 9000:9000 \
    -p 9090:9090 \
    -e "MINIO_ROOT_USER=admin" \
    -e "MINIO_ROOT_PASSWORD=admin123" \
    quay.io/minio/minio server /data --console-address ":9090"
</code></pre>

* Go to [http://localhost:9090](http://localhost:9090/) enter username `admin` and password `admin123` and create bucket name `cannex`
* Set your bucket public

```
1: 📄 brew install minio/stable/mc
2: 📄 mc alias set local http://localhost:9000 admin admin123
3: 📄 mc policy set public local/cannex
```

* 📄 you can check bucket access in [http://localhost:9090](http://localhost:9090/)&#x20;
* If you set follow by document you can use storage environment follow by `example.env`

```sh
# S3 storage config
S3_PUBLIC_URL=http://localhost:9000/cannex
S3_ROOT_PATH=uploader
S3_ENDPOINT=http://localhost:9000
S3_ACCESS_KEY_ID=admin
S3_ACCESS_SECRET=admin123
S3_REGION=us-east-1
S3_ACL=public-read
S3_BUCKET=cannex
```

* Install node

```
1: 📄 nvm install v20
2: 📄 nvm alias default 20
```

* Install node dependencies

```
1: 📄 yarn install
```

* Create `.env`  by using value in `example.env`  and set your email config manually
  * Set up your [gmail app password](https://support.google.com/mail/answer/185833?hl=en) for email service

```sh
SMTP_HOST=smtp.gmail.com # fixed if using gmail #
SMTP_PORT=465 # fixed if using gmail #
SMTP_SECURE=true # fixed if using gmail #
SMTP_USERNAME=<your_email>
SMTP_PASSWORD=<your_app_password>
SMTP_REJECT_UNAUTH=true # fixed if using gmail #
SMTP_DEFAULT_FROM=<your_email>
SMTP_DEFAULT_REPLYTO=<your_email>
```

* Build your project

```
1: 📄 yarn build
2: 📄 yarn start
```

* Go to [http://localhost:1337/](http://localhost:1337/) and set up your local strapi account
* Before your set up your local strapi account you can develop by using command

```
1: 📄 yarn dev
```

**Happy hacking :)**
