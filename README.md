# hostwithquantum/setup-runway

| under construction | |
|---|---|
| :warning: :construction: | This is a work in progress. |

A GitHub action to deploy your application to runway! Questions, issues? Please use discussions or the issue tracker on the repository. If you like what you see here, **we appreciate a** :star: and if you'd subscribe to [(our monthly) mailing list](https://runway.planetary-quantum.com/) to stay in the loop!

## Quick Start

This uses another action ([hostwithquantum/setup-runway](https://github.com/hostwithquantum/setup-runway)) to install the runway CLI and to login:

```yaml
steps:
- uses: actions/checkout@v4
- uses: hostwithquantum/setup-runway@v0.2.1
  with:
    username: ${{ secrets.RUNWAY_USERNAME }}
    password: ${{ secrets.RUNWAY_PASSWORD }}
- uses: hostwithquantum/deploy-runway@vFIXME
  with:
    application: my-app
```

## Options

Currently supported options:

| option        | default value       | description                                       |
|---------------|---------------------|---------------------------------------------------|
| application   | `<none>`            | The name of the application                       |
| create        | `false`             | Create the application on runway                  |
| plan          | `free-launch`       | The plan identifier (may require a CC!)           |
| log-level     | `error`             | debug, info, warn, error                          |

## Outputs

| output        | description                                       |
|---------------|---------------------------------------------------|
| application   | The name of the application                       |
| url           | The url of the application                        |


## Examples

### Deploy an application

This is an example how to setup the runway CLI and then how to deploy your app `cool-app`.

Once you have the client setup, you can run commands and play around with output and so on. To keep it simple, we're only deploying the code. :) Since the app exists already on runway we use the `runway gitremote` command to initialize the setup.

```yaml
# .github/workflows/release.yml
---
name: release

on:
  push:
    tags:

jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      YOUR_APPLICATION_NAME: cool-app
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: create public/private key on GHA
        run: |
          mkdir -p ~/.ssh/
          echo ${{ secrets.PRIVATE_KEY }} > ~/.ssh/id_rsa
          echo ${{ secrets.PUBLIC_KEY }} > ~/.ssh/id_rsa.pub
          chmod 0600 ~/.ssh/id_rsa*
      - uses: hostwithquantum/setup-runway@v0.2.1
        with:
          username: ${{ secrets.RUNWAY_USERNAME }}
          password: ${{ secrets.RUNWAY_PASSWORD }}
          setup-ssh: true
      - uses: hostwithquantum/deploy-runway@vFIXME
        with:
          application: ${{ env.YOUR_APPLICATION_NAME }}
```
