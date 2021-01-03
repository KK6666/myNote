# url-loader/file-loader
    url-loader在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL。
    当需要打包的文件（如图片）较小时，可以通过设置limit（文件大小小于limit时），让需要打包的文件直接打包到输出文件里，可减少一次http请求；文件较大，则单独打包出一个文件，可提升输出的主文件的加载速度
```
module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit:10240 //打包文件小于10240K，则直接打包进bundle.js;大于则单独打包成一个文件
              name: '[name]_[hash].[ext]',
            },
          },
        ],
      },
    ],
  },
}
```

# babel
### 1.业务代码，只需配置presets，在需要使用babel的文件使用 import "@babel/polyfill"（会污染全局环境）
```
    rules: [
        {
        test: /\.m?js$/,
        exclude: /node_modules/,  //不应用的文件夹
        use: {
          loader: "babel-loader",
          options: {
            presets: [
              [
                '@babel/preset-env',
                {
                  useBuiltIns: 'usage', //按需引入babel规则，减小index.js编译后的文件大小
                  targets: {
                    edge: "17",
                    firefox: "60",
                    chrome: "67",
                    safari: "11.1",
                  },
                }  //根据浏览器版本决定是否引用
              ]
            ]
          }
        }
      }
    ]
```
### 2.库项目代码，只需配置plugins：transform-runtime（以闭包形式注入，不存在全局污染的问题）
```
     {
        test: /\.m?js$/,
        exclude: /node_modules/,  //不应用的文件夹
        use: {
          loader: "babel-loader",
          options: {
            "plugins": [
              [
                "@babel/plugin-transform-runtime",
                {
                  "corejs": 2,
                  "helpers": true,
                  "regenerator": true,
                  "useESModules": false,
                }
              ]
            ]

          }
        }
      }
```

# Tree Shaking
功能：实现模块文件只引用需要的代码（例如引用的js文件包含多个方法，则只会引用用到的方法）
仅支持静态引入，如ESModule(import)，不支持动态引用，如commonJS（require）
### 用法
webpack.config.js
```
  {
    mode: 'development',

    // mode为development
    devtool: 'cheap-module-source-map',

    // mode为production
    // devtool: 'cheap-module-eval-source-map',

    // mode为production，不必配置optimization
    optimization: {
      usedExports: true,
    },
  }
```
package.json
```
  {
    //过滤掉不需要引用的文件，如果全部引用则设置为false
    "sideEffects": "false"
  }
```

# webpack不同环境，相同配置提取
  1.创建webpack.common.js,提取不同环境的相同配置代码
  2.各环境配置文件引用webpack.common.js，使用使用webpack-merge，合并相同配置代码
    webpack-merge：https://www.npmjs.com/package/webpack-merge
### 用法
webpack.dev.js
```
const path = require('path')
const { merge } = require('webpack-merge');
const commonConfig = require('./webpack.common.js')

const devConfig = {
  mode: 'development',
  devtool: 'cheap-module-source-map',
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    port: 9000,
    open: true,
    // hotOnly: true  //hotOnly: true页面将不自动刷新
  },
}

module.exports = merge(devConfig, commonConfig)
```


