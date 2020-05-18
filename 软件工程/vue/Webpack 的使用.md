## Webpack 的使用

```bash
npm install -g vue-cli  # 下载对应的脚手架
vue list  # 官方的骨架
```



安装

```bash
npm install webpack  // 局部安装
npm install webpack@3.6.0 // 指定版本
npm install webpack -g // 全局安装
```



###  配置

[官方配置详解]( https://webpack.docschina.org/concepts/ )

`webpack.config.js`

- 设置打包的文件和输出的文件

  - ```js
    let path = require('path')
    
    module.exports = {
            devServer: { // 开发服务器配置 webpack-dev-server
                port: 8000,
                progress: true,
                contentBase: './build',
                compress: true
            },
            mode: 'development', // 开发模式，生产模式production
            entry: './src/index.js', // 入口
            output: {
                filename: 'bundle.js', // 打包后的文件名
                // filename: 'bundle.[hash].js'  // 每次打包后的文件都不同，避免缓存造成的无法更新
                // filename: 'bundle.[hash:8].js'  // 只要8位
                path: path.resolve(__dirname, "dist") // 路径必须是一个绝对路径
            },
            plugin: { // 配置的插件
                ...
            }，
            module: { //模块
                rules: [ // 规则，
                    // 不用数组使用字符串只用一个loader
                    // loader顺序 从下往上执行，从右往左执行
                    // 还可以写成对象的形式，可以有更多的参数
                // 需要的文件需要在对应的js文件中import
                    {
                        test: /\.css$/,
                        use: ['style-loader',
                            {
                                loader: 'css-loader',
                                options: {
                                    insertAt: 'top', // 设置css样式的位置
                                }
                            }
                        ]
                        }, // 处理css文件
                    ]
                }
            }
    ```

  - 设置多页面

    - ```js
      let path = require('path')
      module.exports = {
          // 多入口
          entry:{
              home: './src/index.js',
              community: './src/community.js'
          },
          output{
          filename: '[name].js'// name 会对entry中的key进行迭代生成多个文件
          path: path.resolve(__dirname, "dist")
      }
      }
      ```

    - 如果有多个页面，可以`npm install html-webpack-plugin`，然后导入

    - ```javascript
      let path = require('path')
      let HtmlWebpackPlugin = require("html-webpack-plugin")
      module.exports = {
          // 多入口
          entry: {
              home: './src/index.js',
              community: './src/community.js'
          },
          output: {
              filename: '[name].js', // name 会对entry中的key进行迭代生成多个文件
              path: path.resolve(__dirname, "dist")
          },
          plugins: [
              new HtmlWebpackPlugin({
                  template: './index.html',
                  filename: 'home.html',
                  chunks: ['home']  // index.js  如果不指定那么所有的js文件都会加载过去
      
              }),
              new HtmlWebpackPlugin({
                  template: './index.html',
                  filename: 'community.html', 
                  chunks: ['community']
              })
          ]
      }
      ```

    - ```npx webpack` build files ！

`package.json`  项目的相关文件

```json
{
  "name": "bbs",  // 名称
  "version": "0.1.0",  // 版本
  "private": true,  // 是否私有
  "scripts": {  // npm run 可以运行的脚本 比如 npm run serve
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {  // 依赖文件
    "core-js": "^3.6.4",
    "vue": "^2.6.11"
  },
  "devDependencies": {  // 开发依赖
    "@vue/cli-plugin-babel": "~4.3.0",
    "@vue/cli-plugin-eslint": "~4.3.0",
    "@vue/cli-service": "~4.3.0",
    "babel-eslint": "^10.1.0",
    "eslint": "^6.7.2",
    "eslint-plugin-vue": "^6.2.2",
    "vue-template-compiler": "^2.6.11"
  },
  "eslintConfig": {  // eslint的配置
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {}
  },
  "browserslist": [  // 浏览器的文件
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}

```







