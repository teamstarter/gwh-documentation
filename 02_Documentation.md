---
title: Documentation
showTitle: true
hideAnchor: false
sidebar: Documentation
---

## Table of contents

1. [Getting started](index.md)
2. Documentation
3. [Contributing](03_Contributing.md)

---

## getCallWebhook

This function initializes and returns callWebhook by assigning it getMetadataFormContext function.

**Params :**

- **getMetadataFromContext** : function that should return the securityMetadata of a webhooks from a context.

## callWebhook

Function to call webhook.

**How it works :** <br />

- Retrieves webhooks according to the type of event and its security metadata
- Make request to all webhook with the associated data

**Params :**

- **data** : data to be sent to the webhook
- **eventType** : type of event
- **context** : context related to the event

## getMetadataFromContext

The purpose of this function is to return the security parameters of a webhook from a context. (It's called when you fetch a webhook)

**Example :**

```js
const { getApolloServer } = require('graphql-web-hook')

...

getApolloServer({
  ...
  getMetadataFromContext: (context) => {
    return getUserIdFromContext(context)
  }
})
```