# Simple Semantic Version

Simple semantic versioning supporting experimental releases.

## Usage

```yml
name: Test Version

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
        uses: VdustR/semver-action@v0
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
| v1.2.3-rc     | 1.2.3-rc     | rc              |
| v1.2.3-rc.1   | 1.2.3-rc.1   | rc              |
| v1.2.3-beta.1 | 1.2.3-beta.1 | beta            |

| Git Branch   | Version                     | Package Tag  |
| ------------ | --------------------------- | ------------ |
| nightly      | 0.0.0-nightly.`${sha}`      | nightly      |
| experimental | 0.0.0-experimental.`${sha}` | experimental |

## Example

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
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
        with:
          version: 7
      - uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "pnpm"
      - run: pnpm install --frozen-lockfile
      - name: Build
        run: pnpm build
      - id: version
        uses: VdustR/semver-action@v0
      - name: version
        run: npm --no-git-tag-version version ${{ steps.version.outputs.version }}
      - name: Publish
        uses: JS-DevTools/npm-publish@v1
        with:
          token: ${{ secrets.NPM_TOKEN }}
          tag: ${{ steps.version.outputs.tag }}
          access: public
```

## License

[MIT](./LICENSE)

Copyright ©️ 2022-present ViPro (京京)
