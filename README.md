# DashPress Docker image

Unofficial [DashPress][1] Docker image

Generate powerful admin apps without writing a single line of code.

[![publish Docker image](https://github.com/freeCodeCamp-China/DashPress-Docker/actions/workflows/publish.yml/badge.svg)][2]

## Run the image

```shell
docker run -p ${YOUR_DESIRED_PORT}:3000  -v ${PATH_TO_SOMEWHERE}:/app -d luojiyin/dashpress
```

## Docker file

```dockerfile
FROM node:18-slim  as builder
WORKDIR /app
RUN npm install -g dashpress

FROM node:18-slim
ENV NODE_ENV=production
COPY --from=builder /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=builder /usr/local/bin /usr/local/bin
WORKDIR /app
EXPOSE 3000
CMD ["dashpress"]

# Steps to use

# 1. Copy the content of this file (mostly the uncommented part) to ${PATH_TO_SOMEWHERE}/Dockerfile

# 2. Open your terminal and cd to ${PATH_TO_SOMEWHERE}/Dockerfile

# 3. Build the image
# docker build . -t dashpress

# 4. Run the image
# docker run -p ${YOUR_DESIRED_PORT}:3000  -v ${PATH_TO_SOMEWHERE}:/app -d dashpress
# ${YOUR_DESIRED_PORT} is the port on the host you want to run DashPress on
# The `${PATH_TO_SOMEWHERE}:/app` is needed to sync the dashpress files
# like the .env, .config-data etc out to your system so that you can edit/view it easily


## Note
# Since docker will cache the steps, it means you will have to rebuild the image often so that you are always running on the latest version
```

## Docker compose

```yml
dashpress:
  image: luojiyin/dashpress
  volumes:
    - ./dashpress/app:/app
  ports:
    - 3000:3000
```

[1]: https://github.com/dashpresshq/dashpress
[2]: https://github.com/freeCodeCamp-China/DashPress-Docker/actions/workflows/publish.yml
