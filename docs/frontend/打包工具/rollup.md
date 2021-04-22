# roolup

一个轻量级的打包工具，适合于一些工具类库的打包。

官方教程文档：[https://www.rollupjs.com/guide/tutorial](https://www.rollupjs.com/guide/tutorial) 

> 以下内容为其中的一些点，是为了简化看的内容；更详细的内容还是推荐查看官方文档。

## 安装环境

~~~shell
npm install rollup --global
~~~

打包一个已存在文件（路径为 `src/main.js`），并生成打包后文件 `bundle.js` ，文件格式为 `commonjs` 

~~~shell
rollup src/main.js -o bundle.js -f cjs
~~~

## 命令行执行

配置文件中的许多选项和命令行的参数是等价的。如果你使用这里的参数，那么将 **覆盖** 配置文件。

命令行文档：[https://www.rollupjs.com/guide/command-line-reference](https://www.rollupjs.com/guide/command-line-reference) 

~~~shell
-i, --input <filename>      要打包的文件（必须）
-o, --file <output>         输出的文件 (如果没有这个参数，则直接输出到控制台)
-f, --format <format>       输出的文件类型 (amd, cjs, esm, iife, umd)
-e, --external <ids>        将模块ID的逗号分隔列表排除
-g, --globals <pairs>       以`module ID:Global` 键值对的形式，用逗号分隔开 
                              任何定义在这里模块ID定义添加到外部依赖
-n, --name <name>           生成UMD模块的名字
-h, --help                  输出 help 信息
-m, --sourcemap             生成 sourcemap (`-m inline` for inline map)
--amd.id                    AMD模块的ID，默认是个匿名函数
--amd.define                使用Function来代替`define`
--no-strict                 在生成的包中省略`"use strict";`
--no-conflict               对于UMD模块来说，给全局变量生成一个无冲突的方法
--intro                     在打包好的文件的块的内部(wrapper内部)的最顶部插入一段内容
--outro                     在打包好的文件的块的内部(wrapper内部)的最底部插入一段内容
--banner                    在打包好的文件的块的外部(wrapper外部)的最顶部插入一段内容
--footer                    在打包好的文件的块的外部(wrapper外部)的最底部插入一段内容
--interop                   包含公共的模块（这个选项是默认添加的）
~~~

## 配置文件

虽然可以使用命令行执行配置，但是配置文件允许我们颗粒化的配置我们的所需。

所有的配置可以在这里找到：[https://www.rollupjs.com/guide/big-list-of-options](https://www.rollupjs.com/guide/big-list-of-options) 

~~~javascript
// rollup.config.js
export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  }
};
~~~

通过命令行可以使用配置文件中的配置

~~~shell
rollup -c # 或者 rollup --config
~~~

## 常用插件包

### 基础包

~~~shell
rollup-plugin-node-resolve # 告诉 Rollup 如何查找外部模块

rollup-plugin-commonjs # 用来将 CommonJS 转换成 ES2015 模块的
~~~

### babel

~~~shell
rollup-plugin-babel # es6 转义 es5
@babel/core # 
@babel/preset-env # 
~~~

将 bebel 添加到配置文件中

~~~javascript
// rollup.config.js
import resolve from 'rollup-plugin-node-resolve';
import babel from 'rollup-plugin-babel';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    resolve(),
    babel({
      exclude: 'node_modules/**' // 只编译我们的源代码
    })
  ]
};
~~~

bebel 配置

~~~json
// src/.babelrc
{
  "presets": [
   [
    "@babel/preset-env",
    {
     "modules": false
    }
   ]
  ]
}
~~~

### 代码压缩

~~~shell
rollup-plugin-uglify #
~~~

添加到配置文件中

~~~javascript
// rollup.config.js
import { uglify } from "rollup-plugin-uglify";

export default {
  plugins: [
    process.env.NODE_ENV === "production" && uglify()
  ]
};
~~~

