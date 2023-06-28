This is a step by step to run/deploy a JavaScript App (Backend + Frontend + DB) on AWS VM instance or any Linux self hosted (with docker installed).

It was originally applied to Dapp names bLearnCms and bLearnMCQ running on Cardano.

- Create folder for MongoDB persistent data
```
mkdir -p dbdata
```
- Create folder for certbot if you want to use HTTPS
```
mkdir -p certbot/www certbot/conf
```
- Create .env with below content
```
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=***

#MCQ
REACT_APP_API_URL=https://your_domain/api

#CMS
REACT_APP_LOGIN_URL=https://your_domain/api/auth/login
REACT_APP_API_URL=https://your_domain/api
REACT_APP_RENEW_ACCESS_TOKEN_URL=https://your_domain/api/auth/refresh
REACT_APP_LOGOUT_URL=https://your_domain/api/auth/logout
REACT_APP_CARDANO_SCAN_URL=https://cardanoscan.io/address
REACT_APP_DONATE_ADDRESS=***
REACT_APP_MAP_KEY=***

#API
CONNECTION_STRING="mongodb://admin:***@mongo:27017/db_name?authSource=admin&readPreference=primary"
#Blockfrost
BLOCKFROST_PROJECT_ID=***
BLOCKFROST_URL=https://cardano-testnet.blockfrost.io/api/v0
#GitHub
GITHUB_TOKEN=***
#unlock script
UNLOCK_SHELL_SCRIPT=getFromScriptV2AutoSelectUtxo.sh
#mail service for register user
MAIL_HOST=smtp.gmail.com
MAIL_USER=***@gmail.com
MAIL_PASSWORD=***

MAIL_FROM=***@gmail.com
#jwt settings
JWT_TOKEN_EXPIRE=60m
JWT_RENEW_TOKEN_EXPIRE=7d
JWT_TOKEN_SECRET=secret
JWT_REFRESH_TOKEN_SECRET=secret
JWT_VERIFY_TOKEN_SECRET=secret
```
- copy .env file to frontend/ , bLearn/
- build docker image for frontend (cms+mcq) and backend api, commands below
```
docker build -f Dockerfile-frontend -t blearn-frontend ./sourcecode
docker build -f Dockerfile-api -t blearn-api ./sourcecode
docker build -f Dockerfile-mcq -t blearn-mcq ./sourcecode
```

- start blearn services, commands below
```
docker-compose -f docker-compose.yml up -d
```
- You need to init certbot certificate for HTTPS (run once only) , please refer this guide: https://mindsers.blog/post/https-using-nginx-certbot-docker/
```
docker exec certbot sh -c 'certbot certonly --webroot --webroot-path /var/www/certbot/ -d your_domain'
```
- to stop creport service, commands below
```
docker-compose -f docker-compose.yml down
```

