# wol-web

## Use Cases

A web app hosted locally for **wakeonlan**, turn on computers in private network.

Use VPN to go into your network and wake up your computers with a simple click in your browser.

**Frontend:** Vue.js + TypeScript

**Backend:** golang + fiber

## UI

![image-20210411083628619](README.assets/image-20210411083628619.png)

## Docker Environment

The app can be hosted with docker which requires the machine to have `docker` and `docker-compose` installed.

If you want to run it without docker (with native golang), see later sections: [Deploy Without Docker](#deploy-without-docker)

Modify the image tags within `docker-compose.yml` and `docker-compose-helpers.yml` depend on what machine you are running.

### x64

#### golang image:

- golang:1.16.3-alpine3.13

#### postgres image:

- postgres:13.2

### raspberry pi (arm)

#### golang image:

- arm32v7/golang:1.16.3-alpine3.13

#### postgres image:

- arm32v7/postgres:13.2

#### Other Arm Libraries

> replace arm32v7 with any of the following based on your arm device
> e.g. `arm64v8/postgres:13.2`

- arm32v5
- arm32v6
- arm32v7
- arm64v8

## Build Frontend

The frontend is written in vuejs and needs to be built manually to generate a `dist` folder which contains the `index.html` and other resources.

### Manual Installation and Build

If you have nodejs 15+, npm on your machine, you can cd into **frontend** and run

```bash
sudo npm install -g @vue/cli        # if you don't have vue on your machine.
npm install
npm run build
```

### Build with docker-compose (recommended)

If you don't have the dependencies installed, you can use **docker-compose** to build the frontend production build.

```bash
docker-compose -f docker-compose-helpers.yml run build-frontend
```

### Build with Makefile (recommended)

```bash
make build-frontend     # exactly the same as the docker-compose method, just a simplified wrapper
```

## Deploy App

### docker-compose (Recommended)

```bash
docker-compose up       # see debug messages if doesn't run
docker-compose up -d    # run in detach mode to show no messages
```

Run the app without `-d` first to confirm that it's running properly (see error messages if any) then run with `-d`.

### Makefile version

The following 2 make commands are exactly the same as the 2 docker-compose commands above

```bash
make deploy-test
make deploy
```

## Backend

For more information and configuration related to backend, check [backend README](./backend/README.md)

You can configure
- database user, password
- port of the server
- number of users allowed to sign up
- Timezone of Database
- JWT Secret and Login Time (JWT_VALID_TIME)

## Deploy Without Docker

Build the frontend app with either native vue or docker-compose, make sure `dist` is in frontend directory.

See [Start Database](./backend#set-up-postgresql-database-with-docker) for how to start a database.

### Start Server

```bash
cd backend
go build -o server .
./server
```
