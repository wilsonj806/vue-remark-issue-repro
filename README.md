# Vue Remark issue reproduction
## Relevant files/ directories
Relevant files and directories listed below:
- [layouts](./src/layouts)
- [Index.md](./post/index.md)
- [Post.md](./post/post.md)
- [Gridsome Config](./gridsome.config.js)

## Description
When using `@gridsome/vue-remark`, if you have markdown files with different `layout` values like the below:
```
<!-- File One -->
---
title: Init post
layout: ~/layouts/FirstLayout.vue
---

<!-- File Two -->
---
title: Init post
layout:
  component: ~/layouts/SecondLayout.vue
  props:
    color: pink
---
```
Then Gridsome fails to consistently build and crashes with a TypeError when it usually does.

### Steps to reproduce
Minimal reproduction in this [repository](https://github.com/wilsonj806/vue-remark-issue-repro)

1) Run `git clone https://github.com/wilsonj806/vue-remark-issue-repro.git`
2) Run `npm install`
3) Run `npm run develop` 5 times or so

### Expected Result
Gridsome should build the app successfully every time `npm run develop` is ran.

### Actual Result
Gridsome crashes with the below error:
```
TypeError: Cannot set property 'extensions' of undefined
    at processInferredFields (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\graphql\nodes\index.js:138:26)
    at processInferredFields (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\graphql\nodes\index.js:141:7)
    at createFields (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\graphql\nodes\index.js:109:3)
    at createNodesSchema (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\graphql\nodes\index.js:28:5)
    at createSchema (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\graphql\createSchema.js:59:3)
    at Schema.buildSchema (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\app\Schema.js:28:28)
    at Plugins.createSchema (E:\SOFTWARE-DEV\PRs\vue-remark-issue-repro\node_modules\gridsome\lib\app\Plugins.js:93:22)
```

Also tried doing some debugging and if I `console.log(fieldDefs)` in "gridsome/lib/graphql/nodes/index.js" line 107 I get the below:
```js
{
  id: {
    key: 'id',
    fieldName: 'id',
    extensions: { isInferred: true },
    value: 'e8a30aafa705a3151839a8aaf62c1e59'
  },
  path: {
    key: 'path',
    fieldName: 'path',
    extensions: { isInferred: true },
    value: '/post/'
  },
  fileInfo: {
    key: 'fileInfo',
    fieldName: 'fileInfo',
    extensions: { isInferred: true },
    value: {
      extension: [Object],
      directory: [Object],
      path: [Object],
      name: [Object]
    }
  },
  content: {
    key: 'content',
    fieldName: 'content',
    extensions: { isInferred: true },
    value: '\n# Lorem ipsum'
  },
  title: {
    key: 'title',
    fieldName: 'title',
    extensions: { isInferred: true },
    value: 'Init post'
  },
  layout: {
    key: 'layout',
    fieldName: 'layout',
    extensions: { isInferred: true },
    value: {
      '0': '~',
      '1': '/',
      '2': 'l',
      '3': 'a',
      '4': 'y',
      '5': 'o',
      '6': 'u',
      '7': 't',
      '8': 's',
      '9': '/',
      '10': 'F',
      '11': 'i',
      '12': 'r',
      '13': 's',
      '14': 't',
      '15': 'L',
      '16': 'a',
      '17': 'y',
      '18': 'o',
      '19': 'u',
      '20': 't',
      '21': '.',
      '22': 'v',
      '23': 'u',
      '24': 'e',
      component: [Object],
      props: [Object]
    }
  }
}
```
### Environment
On Windows:
```
  System:
    OS: Windows 10 10.0.18362
    CPU: (4) x64 Intel(R) Core(TM) i5-3570K CPU @ 3.40GHz
  Binaries:
    Node: 12.16.0 - C:\Program Files\nodejs\node.EXE
    npm: 6.13.4 - C:\Program Files\nodejs\npm.CMD
  Browsers:
    Edge: 44.18362.449.0
  npmPackages:
    gridsome: ^0.7.0 => 0.7.0
```

Also fails on my Macbook:
```
  System:
    OS: macOS 10.15.3
    CPU: (4) x64 Intel(R) Core(TM) i5-4258U CPU @ 2.40GHz
  Binaries:
    Node: 10.14.1 - ~/.nvm/versions/node/v10.14.1/bin/node
    npm: 6.4.1 - ~/.nvm/versions/node/v10.14.1/bin/npm
  Browsers:
    Chrome: 79.0.3945.130
    Firefox: 69.0.3
    Safari: 13.0.5
  npmPackages:
    @gridsome/vue-remark: ^0.1.9 => 0.1.9
    gridsome: ^0.7.12 => 0.7.12
  npmGlobalPackages:
    @gridsome/cli: 0.3.1
```