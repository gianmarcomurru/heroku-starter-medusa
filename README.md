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
<br>
<a href="https://heroku.com/deploy?template=https://github.com/gianmarcomurru/heroku-starter-medusa">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>
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
  * [7. Build the project](#7-build-the-project)
  * [8. Push changes to Heroku](#8-push-changes-to-heroku)
  * [9. Check Heroku build logs](#9-check-heroku-build-logs)

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

## 7. Build the project

```shell=
npm install cross-env babel-preset-medusa-package
```

## 8. Push changes to Heroku
```shell=
git add .
git commit -m "Deploy Medusa App on Heroku"
heroku buildpacks:set heroku/nodejs
git push heroku HEAD:master
```

## 9. Check Heroku build logs

```shell=
heroku logs -n 500000 --remote heroku --tail
```

---

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

###### tags: `MedusaJS` `Heroku` `Documentation`
