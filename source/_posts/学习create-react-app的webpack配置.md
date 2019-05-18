---
title: 学习create-react-app的webpack配置
date: 2019-05-16 19:24:38
tags: webpack create-react-app
---

## 如何查看 create-react-app 的 webpack 配置

### `npm run eject`

####

- fs : `https://nodejs.org/dist/latest-v10.x/docs/api/fs.html#fs_file_system`

  - node.js 中 fs 模块
  - fs 模块提供了一个 API，用于以模仿标准 POSIX 函数的方式与文件系统进行交互

- is-wsl : `https://www.npmjs.com/package/is-wsl`

  - 检查进程是否在 Windows Subsystem for Linux 内运行（Windows 上的 Bash）

- path : `https://nodejs.org/dist/latest-v10.x/docs/api/path.html`

  - path 模块提供用于处理文件路径和目录路径的实用工具

- resolve : `https://www.npmjs.com/package/resolve`

  - 实现 node.js 中 require.resolve()算法，以便使用 require.resolve()异步和同步地代表文件

- pnp-webpack-plugin : `https://github.com/arcanis/pnp-webpack-plugin`

  - 暂时不知道这个 plugin 是干什么的

- html-webpack-plugin : `https://webpack.js.org/plugins/html-webpack-plugin/`
  `https://github.com/jantimon/html-webpack-plugin`

  - HtmlWebpackPlugin 简化了 HTML 文件的创建，以便为你的 webpack 包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个 HTML 文件，使用 lodash 模板提供你自己的模板，或使用你自己的 loader。

- case-sensitive-paths-webpack-plugin : `https://github.com/Urthen/case-sensitive-paths-webpack-plugin`

  - 强制所有必需模块的完整路径与磁盘上实际路径的确切大小写相匹配。使用此插件有助于缓解处理 OSX 的开发人员（不遵循严格的路径区分大小写）会导致与其他开发人员或运行其他需要正确路径路径的操作系统的构建框冲突的情况。

- react-dev-utils : `https://github.com/zanettin/react-dev-utils`

  - react-dev-utils 是 Facebook 开发的 Creact-React-App 里的一个工具库

- terser-webpack-plugin : `https://webpack.js.org/plugins/terser-webpack-plugin/#root`

  - webpack 官方提供的一个压缩 js 文件的插件
  - 使用说明:

```javascript
const TerserPlugin = require("terser-webpack-plugin");
module.exports = {
  optimization: {
    minimizer: [new TerserPlugin(options)]
  }
};
```

- options  
  |name|type|default|description|
  |-|-|-|-|
  |test|String RegExp Array(String RegExp)|/\.m?js(\?.\*)?\$/i|匹配符合规则的文件|
  |include|String RegExp Array(String RegExp)|undefined|要包含的文件|
  |exclude|String RegExp Array(String RegExp)|undefined|不包含的文件|
  |chunkFilter|Function<(chunk) -> boolean>|() => true|自定义规则压缩某些代码,默认情况下全部都会被压缩,true 的时候会进行压缩,false 则不进行压缩|
  |cache|Boolean String |false|是否启动文件缓存,默认缓存路径:node_modules/.cache/terser-webpack-plugin.当 cache 为 String 时,指定缓存路径.|
  |cacheKeys|Function<(defaultCacheKeys, file) -> Object>|defaultCacheKeys => defaultCacheKeys|允许你覆盖默认的 cache keys|
  |parallel|Boolean Number|false|使用多进程并行运行来提高构建速度,默认并发进行书为:os.cpus().length - 1|
  |sourceMap|Boolean|false|使用 sourceMap 映射报错信息到指定模块,如果你使用你自己自定义的 minify 函数,请仔细阅读 minify 部分从而正确的处理 sourceMap。⚠️cheap-source-map 这个选项在这个插件中不会运行。|
  |minify|Function|undefined|允许你覆盖默认的 minify 函数,默认情况下,插件使用 terser 包中的 minify 函数。适用于使用和测试未发布的版本或分支。|
  |terserOptions|Object|default|Terser minify 选项:https://github.com/terser-js/terser#minify-options |

* mini-css-extract-plugin ：`https://webpack.docschina.org/plugins/mini-css-extract-plugin/`

- webpack 官方提供的一个将 CSS 提取到单独的文件中的插件。它为每个包含 CSS 的 JS 文件创建一个 CSS 文件。它支持 CSS 和 SourceMaps 的按需加载。

* optimize-css-assets-webpack-plugin : `https://github.com/NMFR/optimize-css-assets-webpack-plugin`

- 用于优化/最小化 CSS 的插件

* postcss-safe-parser : `https://github.com/postcss/postcss-safe-parser`

- PostCSS 的容错 CSS 解析器，可以找到并修复语法错误，能够解析任何输入

* webpack-manifest-plugin : `https://github.com/danethurber/webpack-manifest-plugin`

- 用于生成资源清单的 Webpack 插件

* workbox-webpack-plugin : `https://www.npmjs.com/package/workbox-webpack-plugin`

- 详情 ： `https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin`

* url : `https://www.npmjs.com/package/url`

- 一个用于解析 url 的包

* postcss-normalize : `https://github.com/csstools/postcss-normalize`

- PostCSS Normalize 允许使用你所需要的 normalize.css。
