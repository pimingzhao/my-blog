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

