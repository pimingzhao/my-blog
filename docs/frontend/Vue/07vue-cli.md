## 官方文档

[https://cli.vuejs.org/zh/guide/](https://cli.vuejs.org/zh/guide/) 

## 显示 webpack 配置

vue-cli 默认有一套 webpack 配置，通过命令行可以将这些配置显性的显示

~~~shell
# 'mode' 指定运行环境，'>' 指定输出文件
vue inspect --mode=development > webpack.dev.js
vue inspect --mode=production > webpack.prod.js
~~~

