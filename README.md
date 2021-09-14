<p align="center">
  <a href="https://www.medusa-commerce.com">
    <img alt="Medusa" src="https://user-images.githubusercontent.com/7554214/129161578-19b83dc8-fac5-4520-bd48-53cba676edd2.png" width="100" />
  </a>
</p>
<h1 align="center">
  Deploy Medusa App on Heroku
</h1>
<p align="center">
The following documentation will guide you through the one-click creation of a new <a href="https://github.com/medusajs/medusa">Medusa</a> project hosted on Heroku. Follow the steps below to get ready.
</p>
<p align="center">
  <a href="https://github.com/medusajs/medusa/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="Medusa is released under the MIT license." />
  </a>
  <a href="https://github.com/medusajs/medusa/blob/master/CONTRIBUTING.md">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat" alt="PRs welcome!" />
  </a>
  <a href="https://discord.gg/xpCwq3Kfn8">
    <img src="https://img.shields.io/badge/chat-on%20discord-7289DA.svg" alt="Discord Chat" />
  </a>
  <a href="https://twitter.com/intent/follow?screen_name=medusajs">
    <img src="https://img.shields.io/twitter/follow/medusajs.svg?label=Follow%20@medusajs" alt="Follow @medusajs" />
  </a>
</p>

- [Setup](#setup)
  * [1. Install Heroku](#1-install-heroku)
  * [2. Login to Heroku from your terminal](#2-login-to-heroku-from-your-terminal)
  * [3. Install MedusaCLI](#3-install-medusacli)
  * [4. Create an Heroku App](#4-create-an-heroku-app)
  * [5. Install Postgresql and Redis on Heroku](#5-install-postgresql-and-redis-on-heroku)
      - [Postgresql](#postgresql)
      - [RedisToGo](#redistogo)
  * [6. Configure the environment variables on Heroku](#6-configure-the-environment-variables-on-heroku)
      - [Config the Redis URL](#config-the-redis-url)
  * [7. Configure Medusa](#7-configure-medusa)
      - [Update `medusa-config.js`](#update--medusa-configjs-)
      - [Update `package.json`](#update--packagejson-)
  * [8. Build the project](#8-build-the-project)
  * [9. Push changes to Heroku](#9-push-changes-to-heroku)
  * [10. Check Heroku build logs](#10-check-heroku-build-logs)
  * [Appendix and FAQ](#appendix-and-faq)

# Setup

## 1. Install Heroku

**Ubuntu**
```shell=
sudo snap install --classic heroku
```
**MacOS**
```shell=
brew tap heroku/brew && brew install heroku
```
**Windows**

Download the appropriate installer for your Windows installation:

[64-bit installer](https://cli-assets.heroku.com/heroku-x64.exe)
[32-bit installer](https://cli-assets.heroku.com/heroku-x86.exe)

## 2. Login to Heroku from your terminal


```shell=
heroku login
```
> Follow the instructions on your terminal


## 3. Install MedusaCLI


```shell=
git clone https://github.com/gianmarcomurru/heroku-starter-medusa.git
cd heroku-starter-medusa
```


## 4. Create an Heroku App


```shell=
HEROKU_PROJECT=medusa-$(uuid | cut -d "-" -f1)
heroku create $HEROKU_PROJECT
heroku git:remote -a $HEROKU_PROJECT
```



## 5. Install Postgresql and Redis on Heroku

#### Postgresql
Install Postgresql using this command

```shell=
heroku addons:create heroku-postgresql:hobby-dev
```
#### RedisToGo
> :warning: Heroku requires you to add a payment method before installing RedisToGO
```shell=
heroku addons:create redistogo:nano
```


## 6. Configure the environment variables on Heroku


```shell=
heroku config:set NODE_ENV=production
heroku config:set MY_HEROKU_URL=$(heroku info -s | grep web_url | cut -d= -f2)
heroku config:set JWT_SECRET=your-super-secret
heroku config:set COOKIE_SECRET=your-super-secret-pt2
heroku config:set NPM_CONFIG_PRODUCTION=false
```

#### Config the Redis URL

Get the current Redis URL:

```shell=
heroku config:get REDISTOGO_URL
```

You should get something like:
```shell=
redis://redistogo:c55451f6a1966969b25b5e537135b73f@sole.redistogo.com:9660/
```

Remove the username from the Redis URL:
redis://~~redistogo~~:c55451f6a1966969b25b5e537135b73f@sole.redistogo.com:9660/

```shell=
heroku config:set REDISTOGO_URL=redis://:c55451f6a1966969b25b5e537135b73f@sole.redistogo.com:9660/
```
## 7. Configure Medusa

#### Update `medusa-config.js`

Set `REDIS_URL` to get `REDISTOGO_URL` environment variable
```javascript=
// Medusa uses Redis, so this needs configuration as well
const REDIS_URL = process.env.REDISTOGO_URL || "redis://localhost:6379";
```

Set `module.export` like this:
```javascript=
module.exports = {
  projectConfig: {
    redis_url: REDIS_URL,
    database_url: DATABASE_URL,
    database_type: "postgres",
    store_cors: STORE_CORS,
    admin_cors: ADMIN_CORS,
    database_extra: {
            ssl: { rejectUnauthorized: false },
    }
  },
  plugins,
};
```

#### Update `package.json`

set `.scripts` like this:

```json=
"scripts": {
    "serve": "medusa start",
    "start": "medusa develop",
    "heroku-postbuild": "medusa migrations run",
    "prepare": "cross-env NODE_ENV=production npm run build",
    "build": "babel src -d dist --extensions \".ts,.js\""
  }
```

## 8. Build the project

```shell=
npm install cross-env babel-preset-medusa-package
```

## 9. Push changes to Heroku
```shell=
git add .
git commit -m "Deploy Medusa App on Heroku"
heroku buildpacks:set heroku/nodejs
git push heroku HEAD:master
```

## 10. Check Heroku build logs

```shell=
heroku logs -n 500000 --remote heroku --tail
```

## Appendix and FAQ

Visit [docs.medusa-commerce.com](https://docs.medusa-comerce.com) for further guides.

<p>
  <a href="https://www.medusa-commerce.com">
    Website
  </a> 
  |
  <a href="https://medusajs.notion.site/medusajs/Medusa-Home-3485f8605d834a07949b17d1a9f7eafd">
    Notion Home
  </a>
  |
  <a href="https://twitter.com/intent/follow?screen_name=medusajs">
    Twitter
  </a>
  |
  <a href="https://docs.medusa-commerce.com">
    Docs
  </a>
</p>



> **Find this document incomplete?** Leave a comment!

###### tags: `MedusaJS` `Heroku` `Documentation`
