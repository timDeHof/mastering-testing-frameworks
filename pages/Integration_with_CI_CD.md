---
title: Integration with CI/CD
hideInToc: true
---
# Integration with CI/CD

### GitHub Actions Example

```yaml
name: Test Suite

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      mongodb:
        image: mongo:6
        ports:
          - 27017:27017
        env:
          MONGO_INITDB_ROOT_USERNAME: root
          MONGO_INITDB_ROOT_PASSWORD: password
          MONGO_INITDB_DATABASE: testdb
        options: >-
          --health-cmd mongo --eval "db.runCommand({ ping: 1 })"
          --health-interval 10s --health-timeout 5s --health-retries 5
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npx jest --watchAll=false
      - run: npx cypress run
```
---
hideInToc: true
---
### Jenkins Example

```groovy
pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh 'npm ci'
                sh 'npm run test:unit'
                sh 'npm run test:e2e'
            }
        }
    }
}
```
