---
title: webpack和babel配置
date: 2019-05-10 14:20:02
tags:webpack
---

## webpack.common.js

```javascript
const path = require("path");
const webpack = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CleanWebpackPlugin = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: ["@babel/polyfill", "./src/index.js"],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "./dist")
  },
  resolve: {
    alias: {
      components: path.resolve(__dirname, "../src/components/")
    }
  },
  module: {
    rules: [
      {
        test: /\.ejs$/,
        include: [path.resolve(__dirname, "../src")],
        use: "ejs-loader"
      },
      {
        test: /\.js$/,
        include: [path.resolve(__dirname, "../src")],
        use: "babel-loader"
      },
      {
        test: /\.(less|css)$/,
        exclude: [path.resolve(__dirname, "../node_modules")],
        use: [
          {
            loader: MiniCssExtractPlugin.loader
          },
          {
            loader: "css-loader",
            options: {
              modules: true,
              localIdentName: "[hash:base64:6]"
            }
          },
          {
            loader: "less-loader",
            options: {
              javascriptEnabled: true
            }
          }
        ]
      },
      {
        test: /\.(css|less)$/,
        include: [path.resolve(__dirname, "../node_modules")],
        use: [
          {
            loader: MiniCssExtractPlugin.loader
          },
          {
            loader: "css-loader"
          },
          {
            loader: "less-loader",
            options: {
              javascriptEnabled: true
            }
          }
        ]
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        use: [
          {
            loader: "url-loader",
            options: {
              limit: 8192
            }
          }
        ]
      },
      {
        test: /\.(md)$/,
        use: [
          {
            loader: "file-loader"
          }
        ]
      }
    ]
  },
  plugins: [
    new webpack.DefinePlugin({
      "process.env.NOVA_ROOT": `process.env.NOVA_ROOT`,
      IS_PROXY: process.env.IS_PROXY,
      DEV_MODE: process.env.DEV_MODE
    }),
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "./src/index.ejs"
    }),
    new MiniCssExtractPlugin({
      filename: `[name].css`
    })
  ]
};
```

### MiniCssExtractPlugin 会在控制台触发警报的原因

#### https://github.com/webpack-contrib/mini-css-extract-plugin/issues/250

## webpack.dev.js

```javascript
const merge = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "development",
  devtool: "eval-source-map",
  devServer: {
    contentBase: "./dist",
    proxy: {
      "/api": {
        target: `process.env.NOVA_ROOT`,
        changeOrigin: true
      }
    }
  }
});
```

## webpack.prod.js

```javascript
const merge = require("webpack-merge");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "production"
});
```

## .babelrc

```javascript
{
    "presets": [
        [
            "@babel/preset-env",
            "useBuiltIns": "usage"
        ],
        "@babel/preset-react"
    ]
    plugins: [
        ["@babel/plugin-proposal-decorators", { "legacy": true }],
        ["@babel/plugin-proposal-class-properties", { "loose" : true }],
        ["import", { libraryName: "antd", style: true }]
        [
			'babel-plugin-module-resolver',
			{
				alias: {
					components: './src/components',
					assets: './src/assets/',
				},
			},
		],
	],
}
```

### https://babeljs.io/docs/en/babel-polyfill
