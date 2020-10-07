---
title: 如何让 VScode 识别到你的 alias
date: 2020-10-07
---

假设有这样一个 alias：

```js
alias: {
  '@': './src'
}
```

## tsconfig.json

`tsconfig.json` 或者 `jsconfig.json` 中加入以下配置：

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

这时候 vscode 就能识别到你的 alias ，也就是可以在输入路径的时候出现提示

## eslint

如果你有用 eslint ，即使 vscode 已经识别出来你的 alias，但 eslint 依然有可能会报 “找不到模块” 的错误。这时候可以使用这个模块 [eslint-import-resolver-webpack](https://www.npmjs.com/package/eslint-import-resolver-webpack) 。就可以去掉 eslint 的报错