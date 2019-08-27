# 统一风格配置

## .editconfig

所有项目都有`.editconfig`配置，目前Webstorm、VSCode等主流编辑器和IDE均支持 `.editconfig`。

利用editconfig来统一团队格式。

配置如下：
```
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

```

## ESLint校验

利用ESLint可以帮助发现各种低级错误，
并且代码风格统一后，也能降低维护和debug成本。

### Webstorm

直接支持eslint，开箱即用。

### VSCode

#### 相关插件

VSCode安装如下插件：

 - ESLint
 - Prettier
 - Vetur
  
#### 配置

打开编辑器配置, 

Mac用户使用 ` Command Palette (⇧⌘P)`快捷键，Windows用户使用 `Command Palette (Ctrl+Shift+P) `打开命令。

输入：`Preferences: Open  Settings(json)`，打开配置，加入如下配置:

```js
    //团队配置
    "files.associations": {
        "*.vue": "vue"
    },
    "editor.suggestSelection":"recentlyUsedByPrefix",
    "editor.formatOnType": true,
    "prettier.jsxSingleQuote": true,
    "editor.tabSize": 2,
    // Defines space handling before function argument parentheses. Requires TypeScript >= 2.1.5.
    "typescript.format.insertSpaceBeforeFunctionParenthesis": true,
    // Defines space handling before function argument parentheses. Requires TypeScript >= 2.1.5.
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
    "[javascript]": {
        "editor.defaultFormatter": "vscode.typescript-language-features"
    },
    "[html]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    // Eslint options
    "eslint.run": "onSave",
    "eslint.autoFixOnSave": true,
    "editor.formatOnSave": false,
    "eslint.alwaysShowStatus": true,
    "eslint.enable": true,
    "eslint.options": {
        "extensions": [
            ".html",
            ".js",
            ".vue",
            ".jsx"
        ]
    },
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        {
            "language": "html",
            "autoFix": true
        },
        {
            "language": "vue",
            "autoFix": true
        }
    ],
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    "vetur.format.options.tabSize": 2,
    "vetur.format.options.useTabs": false,
    "vetur.format.defaultFormatterOptions": {
        "prettier": {
          // Prettier option here
          "semi": false
        }
      },
    "vetur.format.scriptInitialIndent": true
    
```

实现保存时自动格式化代码功能。