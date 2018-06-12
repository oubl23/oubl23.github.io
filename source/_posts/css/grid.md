---
title: 'css3 grid'
date: 2018-01-10 20:43:15
categories: css
tags: 
---

## 定义一个网格
在父容器中display 属性中设为*grid/inline-grid*

### 定义网格横纵样式
grid-template-columns和grid-template-rows

### 示例

```css
.wrapper {
    display: grid;
    grid-template-columns: 100px 10px 100px 10px 100px 10px 100px;
    grid-template-rows: auto 10px auto 10px auto;
}
```

## 网格元素站位
1. 行 
- grid-column-start/grid-column-end
- grid-column: start/end
2. 列 
- grid-row-start/grid-row-end
- grid-row: start/end

合并简写:
grid-area: row-start/column-start/row-end/column-end

```css
.a {
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 1;
  grid-row-end: 2;
}
.a {
    grid-column: 1 / 2;
    grid-row: 1 / 2;
}
.a {
  grid-area: 1/1/2/2;
}
```
