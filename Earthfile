FROM node:13.10.1-alpine3.11
WORKDIR /js-example
ARG EARTHLY_TARGET_TAG_DOCKER

deps:
    COPY package.json package-lock.json ./
    RUN npm install
    # Also save these back to host, in case package-lock.json changes.
    SAVE ARTIFACT package.json AS LOCAL ./package.json
    SAVE ARTIFACT package-lock.json AS LOCAL ./package-lock.json


build:
    FROM +deps
    COPY src src
    COPY dist dist
    RUN npx webpack
    SAVE ARTIFACT dist /dist AS LOCAL ./dist

docker:
    FROM +deps
    COPY +build/dist dist
    EXPOSE 8080
    ENTRYPOINT ["/js-example/node_modules/http-server/bin/http-server", "./dist"]
    SAVE IMAGE js-example:$EARTHLY_TARGET_TAG_DOCKER