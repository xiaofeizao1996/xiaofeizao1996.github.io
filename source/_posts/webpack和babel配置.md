---
title: webpack和babel配置
date: 2019-05-10 14:20:02
tags: webpack
---

## webpack.common.js

```javascript
const path = require("path");
const webpack = require("webpack");
const CleanWebpackPlugin = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const ProgressBarPlugin = require("progress-bar-webpack-plugin");

const devMode = process.env.NODE_ENV === "development";

module.exports = {
  entry: {
    app: "./src/index.js"
  },
  output: {
    filename: "[name].[hash].js",
    path: path.resolve(__dirname, "../dist"),
    publicPath: "/"
  },
  resolve: {
    alias: {
      root: path.resolve(__dirname, "./"),
      components: path.resolve(__dirname, "../src/components/"),
      assets: path.resolve(__dirname, "../src/assets/"),
      services: path.resolve(__dirname, "../src/services/"),
      utils: path.resolve(__dirname, "../src/utils/"),
      models: path.resolve(__dirname, "../src/models/"),
      "@ant-design/icons/lib/dist$": path.resolve(__dirname, "../src/icons.js")
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
          devMode ? "style-loader" : MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              modules: true,
              localIdentName: "[hash:base64:6]"
            }
          },
          { loader: "postcss-loader" },
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
          devMode ? "style-loader" : MiniCssExtractPlugin.loader,
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
        test: /\.(jpg|jpeg|bmp|svg|png|webp|gif)$/,
        include: [path.resolve(__dirname, "../src")],
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
      "process.env.NOVA_ROOT": JSON.stringify(process.env.NOVA_ROOT),
      IS_PROXY: JSON.stringify(process.env.IS_PROXY),
      DEV_MODE: JSON.stringify(process.env.DEV_MODE)
    }),
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "./src/index.ejs"
    }),
    new MiniCssExtractPlugin({
      filename: `[name].css`
    }),
    new ProgressBarPlugin(),
    new webpack.optimize.ModuleConcatenationPlugin()
  ],
  optimization: {
    runtimeChunk: true,
    usedExports: true,
    splitChunks: {
      chunks: "async",
      minSize: 30000,
      maxSize: 0,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: "~",
      name: true,
      cacheGroups: {
        vendors: {
          name: "vendor",
          chunks: "async",
          minChunks: 5
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
};
```

### MiniCssExtractPlugin 会在控制台触发警报的原因

#### https://github.com/webpack-contrib/mini-css-extract-plugin/issues/250

## webpack.dev.js

```javascript
const merge = require("webpack-merge");
const BundlePlugin = require("webpack-bundle-analyzer");
const common = require("./webpack.common.js");

const { BundleAnalyzerPlugin } = BundlePlugin;

module.exports = merge(common, {
  mode: "development",
  devtool: "eval-source-map",
  devServer: {
    contentBase: "../dist",
    proxy: {
      "/api": {
        target: process.env.NOVA_ROOT,
        changeOrigin: true
      }
    }
  },
  optimization: {
    runtimeChunk: {
      name: entrypoint => `runtime~${entrypoint.name}`
    }
  },
  pluginsL: [
    new BundleAnalyzerPlugin({
      analyzerMode: "server",
      analyzerHost: "127.0.0.1",
      analyzerPort: 8888,
      reportFilename: "report.html",
      defaultSizes: "parsed",
      openAnalyzer: true,
      generateStatsFile: false,
      statsFilename: "stats.json",
      logLevel: "info"
    })
  ]
});
```

## webpack.prod.js

```javascript
const merge = require("webpack-merge");
const TerserPlugin = require("terser-webpack-plugin");
const CompressionPlugin = require("compression-webpack-plugin");
const common = require("./webpack.common.js");

module.exports = merge(common, {
  mode: "production",
  devtool: "false",
  plugins: [
    new CompressionPlugin({
      cache: true,
      test: /\.js(\?.*)?$/i
    })
  ],
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        parallel: 4
      })
    ],
    flagIncludedChunks: true,
    occurrenceOrder: true,
    mergeDuplicateChunks: true
  }
});
```

## .babelrc

```javascript
{
	"presets": [
		[
			"@babel/preset-env",
			{
				"useBuiltIns": "usage",
				"corejs": 2
			}
		],
		["@babel/preset-react"]
	],
	"plugins": [
		[
			"import",
			{
				"libraryName": "antd",
				"style": true
			}
		],
		["@babel/plugin-proposal-decorators", { "legacy": true }],
		["@babel/plugin-proposal-class-properties", { "loose": true }],
		["@babel/plugin-syntax-dynamic-import"],
		[
			"@babel/plugin-transform-runtime",
			{
				"absoluteRuntime": false,
				"corejs": false,
				"helpers": true,
				"regenerator": true,
				"useESModules": false
			}
		]
	]
}
```

### https://babeljs.io/docs/en/babel-polyfill
