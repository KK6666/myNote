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
              limit:10240
              // name: '[name]_[hash].[ext]',
            },
          },
        ],
      },
    ],
  },
}
```
