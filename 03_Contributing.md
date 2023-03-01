---
title: Contributing
showTitle: true
hideAnchor: false
sidebar: Documentation
---

## Table of contents

1. [Getting started](index.md)
2. [Documentation](02_Documentation.md)
3. Contributing

---

## Install environnement

yarn :

```
  apt-get install git curl yarn
  # or
  brew install git curl yarn
```

# Project

```
git clone git@github.com:teamstarter/graphql-web-hooks.git
cd graphql-web-hooks
yarn
yarn start
```

# Test the migration script locally

```
yarn run gnj migrate ./../tests/sqliteTestConfig.js
```

# Start a test server using the test database migrated previously

```
yarn start
```

# Running the test

```
yarn test
```

Debugging a specific test

```
node --inspect-brk ./node_modules/jest/bin/jest.js ./tests/job.spec.js
```

---

[Previous: Documentation](02_Documentation.md)

---

### Teamstarter's other libraries

- [GraphQL-Sequelize-Generator](https://teamstarter.github.io/gsg-documentation/)
  - A Graphql API generator based on Sequelize.
- [GraphQL-Node-Jobs](https://teamstarter.github.io/gnj-documentation/)
  - A job scheduler, a runner and an interface to manage jobs. In one lib.
