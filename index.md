---
title: Graphql-Web-Hooks
showTitle: true
hideAnchor: false
sidebar: Documentation
---

## Table of contents

1. Getting started
2. [Documentation](02_Documentation.md)
3. [Contributing](03_Contributing.md)

---

GWH provides a flexible and simple tool to executes and manages webhooks in a GraphQL environement.

It is based on graphql-sequelize-generator and inspired by graphql-node-jobs.

---

## Prerequisites

To use GWH, you need a project with:

- Apollo Server V4
- Express
- Sequelize
- [GraphQL-Sequelize-Generator](https://github.com/teamstarter/graphql-sequelize-generator)

A few modules need to be installed, like WebSocket, etc.

---

## How to setup

---

### Add GWH to your repository

```
yarn add graphql-web-hook
```

---

### Migrate models

---

```
yarn run gnj migrate <configPath>
```

---

### Add server apollo to your express server

```js
const path = require("path");
const { getApolloServer } = require("./../lib/index");
const express = require("express");
const http = require("spdy");
const { PubSub } = require("graphql-subscriptions");
const { WebSocketServer } = require("ws");
const { expressMiddleware } = require("@apollo/server/express4");
const cors = require("cors");
const { json } = require("body-parser");

const config = require("./sqliteTestConfig.js");
const { getMetadataFromContext } = require("./tools");
const app = express();

var dbConfig = require(path.join(__dirname, "/sqliteTestConfig.js")).test;

async function startServer() {
  var options = {
    spdy: {
      plain: true,
    },
  };
  const httpServer = http.createServer(options, app);

  const pubSubInstance = new PubSub();

  const wsServer = new WebSocketServer({
    // This is the `httpServer` we created in a previous step.
    server: httpServer,
    // Pass a different path here if app.use
    // serves expressMiddleware at a different path
    path: "/graphql",
  });

  const server = await getApolloServer({
    dbConfig,
    pubSubInstance,
    getMetadataFromContext,
    playground: true,
    wsServer,
    apolloServerOptions: {},
  });

  await server.start();

  app.use(
    "/graphql",
    cors(),
    json(),
    expressMiddleware(server, {
      // THIS IS FOR TESTING PURPOSE, DO NOT DO THAT IN PRODUCTION
      context: ({ req }) => ({ userId: req.headers.userId }),
    })
  );

  const port = process.env.PORT || 8080;

  httpServer.listen(port, async () => {
    console.log(
      `🚀 http/https/h2 server runs on  http://localhost:${port}/graphql .`
    );
  });
}

startServer();
```

---

## How to use

```js
const { getCallWebhook } = require('graphql-web-hook')

const callWebhook = getCallWebhook(getMetadataFromContext)

await callWebhook({
  evenType: 'publish',
  context: {
    userId: 1,
    ...
  },
  data: {
    text: 'New post publish',
  },
})
```

---

[Next: Documentation](02_Documentation.md)

---

### Teamstarter's other libraries

- [GraphQL-Sequelize-Generator](https://teamstarter.github.io/gsg-documentation/)
  - A Graphql API generator based on Sequelize.
- [GraphQL-Node-Jobs](https://teamstarter.github.io/gnj-documentation/)
  - A job scheduler, a runner and an interface to manage jobs. In one lib.
