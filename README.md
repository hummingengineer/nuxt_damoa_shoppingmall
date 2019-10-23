# 다모아 쇼핑몰

> Nuxt를 이용한 서버사이드 렌더링 쇼핑몰 제작 프로젝트

## Used
- node.js
- Nuxt.js
- express.js
- mongoDB
- vue
- vuetify

## Installed Dependencies
- express-session
- mongoose
- multer
- passport
- passport-local
- dotenv

## 설치 방법

``` bash
# download
$ git clone https://github.com/hummingengineer/nuxt_damoa_shoppingmall.git

# move
$ cd damoa

# install dependencies
$ yarn install

# .env 파일 생성 및 내용 작성
eg)
SESSION_SECRETKEY = 'your secret key'
DB_URL = mongodb://localhost:27017/test

# 개발 모드 구동
$ yarn dev

# 크롬으로 localhost:3000 접속
```

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).
