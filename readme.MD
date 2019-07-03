# jest初始化 && typescript && babel && ts-jest

> 介绍jest最简单的使用方式，如果项目使用typescript需要做什么额外的配置；本文提供了两种方式第一种是babel方式，第二种是通过安装配置ts-jest的方式
## 1.初始化一个jest测试项目

1. 初始化，并安装jest

```
npm init
npm install --save-dev jest
```

2. 创建sum.js文件，创建测试文件sum.test.js

```js
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

```js
const sum = require('../sum');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

3. 在package.json中添加配置

```JSON
{
  "scripts": {
    "test": "jest"
  }
}
```

4. 运行npm run test

```
✓ adds 1 + 2 to equal 3 (4ms)
Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.312s
Ran all test suites.
```
> 会寻找项目目录下匹配```**/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x)```规则的文件，作为测试用例文件

## 2.使用typescript-方式1：使用babel

1. 根目录创建babel.config.js
   
```
npm install --save-dev babel-jest @babel/core @babel/preset-env
```
```JS
// babel.config.js
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          node: 'current',
        },
      },
    ],
  ],
};
```
2. 使用typescript,安装@babel/preset-typescript， 在babel.config.js中添加配置

```
npm install --save-dev @babel/preset-typescript
```
```js
// babel.config.js
module.exports = {
  presets: [
    ['@babel/preset-env', {targets: {node: 'current'}}],
+    '@babel/preset-typescript',
  ],
};
```
3. 测试 创建add.ts文件
```ts
function sum(a, b):Number {
  return a + b;
}
module.exports = sum;
```

```js
const sum = require('../add');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```
## 3.使用typescript-方式2：使用ts-jest 

1. 安装相关依赖
```
npm install --save-dev jest typescript ts-jest @types/jest
```
2. 生成jest.config.js文件

```
npx ts-jest config:init
```
```js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
};
```

3. 注视掉babel.config.js文件中的内容，npm run test 进行测试
```ts
function sum(a, b):Number {
  return a + b;
}
module.exports = sum;
```

```js
const sum = require('../add');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```