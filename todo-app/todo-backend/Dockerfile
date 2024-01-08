FROM node:latest AS build
# Ctrl-C works
RUN apt-get update && apt-get install -y --no-install-recommends dumb-init

WORKDIR /usr/src/app
COPY package*.json /usr/src/app
RUN npm ci --omit=dev

FROM node:20.10.0-bullseye-slim
COPY --from=build /usr/bin/dumb-init /usr/bin/dumb-init

ENV NODE_ENV production
COPY --chown=node:node --from=build /usr/src/app/node_modules /usr/src/app/node_modules
COPY --chown=node:node . /usr/src/app

WORKDIR /usr/src/app
USER node
CMD ["dumb-init", "npm", "start"]
