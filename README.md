# Simple Semantic Version

Simple semantic versioning supporting experimental releases.

## Usage

```yml
name: NPM Publish

on:
  push:
    branches:
      - experimental
    tags:
      - v*.*.*
      - v*.*.*-*

jobs:
  container-job:
    runs-on: ubuntu-latest

    steps:
      - id: version
        uses: actions/simple-semantic-version@v0
        with:
          defaultTag: "latest" # Optional. Only when tags matching `v*.*.*` .
      - name: Echo result
        run: |
          echo ${{ steps.version.outputs.version }}
          echo ${{ steps.version.outputs.tag }}
```

| Git tag       | Version      | Package Tag     |
| ------------- | ------------ | --------------- |
| v1.2.3        | 1.2.3        | `${defaultTag}` |
| v1.2.3-rc.1   | 1.2.3-rc.1   | rc              |
| v1.2.3-beta.1 | 1.2.3-beta.1 | beta            |

| Git Branch   | Version                     | Package Tag  |
| ------------ | --------------------------- | ------------ |
| nightly      | 0.0.0-nightly.`${sha}`      | nightly      |
| experimental | 0.0.0-experimental.`${sha}` | experimental |

## License

[MIT](./LICENSE)

Copyright ©️ 2022-present ViPro (京京)
