## flex 布局

阮一峰的教程：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

### 常见的问题

- flex 快捷设置（等同于同时设置了 `flex-grow` 、`flex-shrink` 、`flex-basis` ）
  - flex: auto;  指 flex: 1 1 auto;
  - flex: none; 指 flex: 0 0 auto;
  - flex: 1; 指 flex: 1 1 0%;