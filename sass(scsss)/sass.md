# 一、sass概念 
- Sass是一种动态样式语言，由Ruby开发者设计开发，语法：属于缩排语法 <a herf="https://www.sass.hk/">Sass官网</a>
##  css预处理器是什么？
- 概念：
```txt
Css 预处理器是一种专门的编程语言，进行web网页样式设计，然后再编译成正常的CSS文件，以供项目使用。
增加了编程式特性，无需考虑浏览器兼容问题。如（sass/scss , less ,stylus）
```
!Alt[Css预处理器原理](./img/yuanli.png)
## 1.2 Sass与Scss的关系
- Sass从第三代开始放弃了缩进风格，并且完全向下兼容普通的css代码，这一带Sass也称为Scss
# 二、使用Sass
## 2.1 基础知识
### （1）Sass配置输出的四种模式
- nested(嵌套格式), expanded（展开格式）, compact（紧凑格式）, compressed（压缩格式）
### （2）嵌套模式
```scss
#main p {
  color: #00ff00;
  width: 97%;
  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}
```
### (3) 父选择器 & 
- 可以用 & 代表嵌套规则外层的父选择器。
```scss
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
- & 必须作为选择器的第一个字符，其后可以跟随后缀生成复合的选择器
```scss
#main {
  color: black;
  &-sidebar { border: 1px solid; }
}
```
### （4）属性嵌套
- font-family, font-size, font-weight 都以 font 作为属性的命名空间。为了便于管理这样的属性
```scss
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}

//css
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```
- 命名空间也可以包含自己的属性值
```scss
.funky {
  font: 20px/24px {
    family: fantasy;
    weight: bold;
  }
}
```
### （5）占位符选择器 %foo (Placeholder Selectors: %foo)
## 2.2 sass基础
### （1）变量 $ 
#### 1.局部变量和全局变量（!global）
#### 2.变量默认值（!default）
```scss
// 变量默认值（!default）
$x:20px;
$x:12px !default;----------加了default就会先执行默语句再执行$x:20px;
// 局部变量
#main {
  $width: 5em;
  width: $width;
}
// 全局变量变量
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```
#### 3.多值变量
- nth(变量,索引)
```scss
$paddings:10px 12px 20px 13px;
body{
  padding；$paddings;
  padding-left:nth($paddings,1); // 索引
} 

```
#### 4.map()类型变量
- map-get(map数组,map中的变量)
```scss
$maps:(color:red,boderColor:blue;)
body{
  padding；$paddings;
  // map-get()索引
  background-color:map-ge($maps,color); 
} 

```
#### 5.变量的特殊用法
- 变量放在<span style="color:red;font-weight:800;">属性或者选择器上</span>
- 引用样式.#{样式变量名}{}
```scss
$className:main;
.#{$className}{
  width:auto;
}
//css
.main{
  width:auto;
}

```
- 变量中用<span style="color:red;font-weight:800;">中横线，下划线相同</span>
```scss
$text_info:lightgreen;
$text-info:red;  //会影响$text_info；中横线，下划线相同
body{
  color:$text_info; //呈现红色，
}
```
### （2）样式的导入
