# webpack

**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。更适用于应用程序的打包。

官方概念部分文档：[https://webpack.docschina.org/concepts/](https://webpack.docschina.org/concepts/) 

> 由于官方文档已经写得很清楚了（API、概念、配置、指南），文档不再复写官方文档的内容，而是对一些比较关键的点进行提取。
>
> **当前文档参考的 webpack 4 官方文档！！！** 

## 配置

官方文档中大部分内容都是介绍如何去配置 `webpack` ，从而实现内容打包。

从这里你可以找到所有内置的配置项：[https://v4.webpack.docschina.org/configuration/](https://v4.webpack.docschina.org/configuration/) 

## loader

> loader 会从右到左（从下到上）地取值(evaluate)/执行(execute)。

理解一个内容的最好办法便是实践：[https://v4.webpack.docschina.org/contribute/writing-a-loader/](https://v4.webpack.docschina.org/contribute/writing-a-loader/) ，通过这篇文档，可以更进一步的了解 `loader` 。

## plugin

[https://v4.webpack.docschina.org/contribute/writing-a-plugin/](https://v4.webpack.docschina.org/contribute/writing-a-plugin/) ，`plugin` 允许我们调用 `webpack` 内部构建打包调用的部分钩子函数，从而参与构建过程。

## 热更新

## 性能优化

### 1. 提升打包构建速度

- Source Map（源代码映射）
  - cheap-module-source-map ：打包编译速度快，只包含行映射
  - source-map ：包含行、列映射，打包速度更慢
  ```js
  module.exports = {
    devtool: 'development' ? 'cheap-module-source-map' : 'source-map'
  }
  ```
- Hot Module Replacement（HMR）
- OneOf loader 优化技术，包裹的 rules 中有一个匹配规则命中时，则不继续往下匹配 loader 
```js
modules.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.css$/,
            use: ['style-loader', 'css-loader']
          },
          {
            test: /\.s[ac]ss$/,
            use: ['style-loader', 'css-loader', 'sass-loader']
          }
        ]
      }
    ]
  }
}
```
- Cache 通过缓存之前的 eslint 检查和 babel 编译结果，提高之后打包、编译的速度, webpack5 默认开启缓存并关闭缓存文件压缩
```js
module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            include: path.resolve(__dirname, '../src'),
            loader: 'babel-loader',
            options: {
              cacheDirectory: true, // 指定缓存目录，为Boolean值时表示关闭或开启缓存
              cacheCompression: false // 关闭缓存文件压缩
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new EslintPlugin({
      cache: true
    })
  ]
}
```
- Thread（多进程）；多进程技术仅在特别耗时的操作中使用，因为每个进程的启动都需要大约 600ms 左右
```js
const threads = require('os').length
const TerserPlugin = require('terser-webpack-plugin')
​
module.exports = {
  // ...
  plugins: [
    new EslintPlugin({
      cache: true,
      threads: threads
    }),
    new TerserPlugin({
      parallel: threads
    })
  ]
}

```
### 2. 减少代码体积

- TreeShaking: 默认开启，通过 ES Module 技术，打包时 ‘筛除’ 未使用的代码
- babel runtime优化：@babel/plugin-transform-runtime 
```js
/**
 * @babel/plugin-transform-runtime 禁用了 Babel 自动对
 * 每个文件的 runtime 注入（* 默认情况下 babel 会对一些公共
 * 方法使用辅助代码，并将其添加到每一个需要它的文件中），使所有
 * 辅助代码从这里引入
*/
module.exports = {
  module: {
    rules: [
      {
        oneOf: [
          {
            test: /\.js$/,
            include: path.resolve(__dirname, '../src'),
            loader: 'babel-loader',
            options: {
              plugins: ['@babel/plugin-transform-runtime']
            }
          }
        ]
      }
    ]
  }
}
```
- image minimizer: mage-minimizer-webpack-plugin
```js
module.exports = {
  plugins: [
    new ImageMinimizerPlugin({
      minizer: {
        implementation: ImageMinimizerPlugin.imageminGenerate,
        plugins: [...]
      }
    })
  ]
}
```

### 3. 优化代码运行性能

- Code Split
```js
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendors: {
          name: 'chunk-vendors',
          test: /[\\/]node_modules[\\/]/,
          priority: -10,
          chunks: 'initial'
        },
        common: {
          name: 'chunk-common',
          minChunks: 2,
          priority: -20,
          chunks: 'initial',
          reuseExistingChunk: true
        }
      }
    },
  }
}
```
- preload/prefetch: preload-webpack-plugin
  - 共同点：都有缓存（再次加载时，直接从缓存加载资源 from fetch cache）；都只加载资源，不执行
  - 异同点：Preload: 立即加载资源；兼容性比 prefetch 好一点；Prefetch：浏览器空闲时加载资源
```js
module.exports = {
  plugins: [
    new PreloadPlugin({
      rel: 'preload',
      include: 'initial',
      fileBlacklist: [/\.map/]
    }),
    new PreloadPlugin({
      rel: 'prefetch',
      include: 'asyncChunks'
    })
  ]
}
```
- PWA 渐进式网络应用程序，使得Web App 可以在离线（无网络）时继续使用，内部通过 Service Workers （由浏览器提供）实现：workbox-webpack-plugin
```js
module.exports = {
	plugins: [
		new WorlboxPlugin.GenerateSW({
			clientsClaim: true,
			skipWaiting: true
		})
	]
}

If ('serviceWorker' in navigator) {
	window.addEvenlistener('load', () => {
		navigator.serviceWorker.register('./service-worker.js').then().catch()
	})
}
```