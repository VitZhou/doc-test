# Vue 开发规范（编写中）

## 0.技术栈版本

项目统一使用 vue2

目前脚手架 `package.json` 内版本为 
```javascript
{
  "vue": "2.6.10",
  "vue-router": "3.0.7",
  "vuex": "^3.1.0"
}
```

## 1.ESlint
项目使用 ESLint 处理各种代码风格细节问题，并且可以自动化操作。
规则集采用 `vue/essential`。
完整配置如下：
```js
module.exports = {
  root: true,
  parserOptions: {
    sourceType: 'module',
    parser: 'babel-eslint',
    ecmaVersion: 2017,
  },

  env: {
    browser: true,
    node: true
  },

  // https://github.com/feross/standard/blob/master/RULES.md#javascript-standard-style
  extends: 'plugin:vue/recommended',

  // required to lint *.vue files
  plugins: [
    "vue",
  ],

  // add your custom rules here
  'rules': [
    'plugin:vue/essential',
    'eslint:recommended'
  ],

  rules: {
    'arrow-parens': 0,
    'generator-star-spacing': 0,
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off'
  }
}
```

## 2.风格指南

务必按照官方推荐的[风格指南](https://cn.vuejs.org/v2/style-guide/)开发项目。

所有项目必须强制使用 `ESlint` 。

本规范对一些官方未指定的部分进行了明确规范。

### 2.1、文件夹命名

项目利用流水线构建，流水线操作系统为大小写敏感的Linux。
故所有文件夹应当使用 **小写英文单词** 。

如果名称由多个单词组成应该使用横线连接。

完整例子如下：
```
└── src/
    ├── components/
        ├── ComponentA.vue
        └── ComponentB.vue
    ├── theme-configurator/
        ├──  theme1/
        └──  theme2/
    └── theme/
```

### 2.2、组件命名

单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)

