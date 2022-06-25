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
### （6）@at-root 跳出嵌套
#### 1.常规跳出嵌套
- 使用@at-root
```scss
body{
  color:red;
  @at-root .container{
    font-size:20px;
  }
}

//css
body{
  color:red; 
}
.container{
  font-size:20px;
}
```
#### 2.@media或者@support跳出嵌套
- 需要使用@at-root(with:media rule(关键字));@at-root(with:support rule(关键字))
- 4种关键字:all（表示所有）;rule（常规css）;media（表示media）;support(表示support)
```scss
body{
  color:red;
  @media screen and(max-width:800px){
    @at-root(with:media rule){
      .container{
        color:red;
      }
    }
  }
}
//css
body{
  color:red;
}
.container{
  color:red;
}
```
## 2.2 sass基础
### （1）变量 $ 
#### 1数据类型
```txt
数字，1, 2, 13, 10px
字符串，有引号字符串与无引号字符串，"foo", 'bar', baz
颜色，blue, #04a3f9, rgba(255,0,0,0.5)
布尔型，true, false
空值，null
数组 (list)，用空格或逗号作分隔符，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
maps, 相当于 JavaScript 的 object，(key1: value1, key2: value2)
```
- 使用 #{} (interpolation) 时，有引号字符串将被编译为无引号字符串，这样便于在 mixin 中引用选择器名：
```scss
@mixin firefox-message($selector) {
  body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}
@include firefox-message(".header"); //字符串变成无引号，方便引用

// css
body.firefox .header:before {
  content: "Hi, Firefox users!"; }
```
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
#### 5.变量的特殊用法<span style="color:red;font-weight:800;">引用样式.#{$selector}{}</span>
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
### (2)运算
- 支持数字的加减乘除、取整等运算 (+, -, *, /, %)，相等运算 == 或 !=如果必要会在不同单位间转换值。
```scss
p {
  width: 1in + 8pt;
}
//css
p {
  width: 1.111in; }

```
### （3）样式的导入
#### 1. 部分文件的导入
- 部分文件以<span style="color:red;font-weight:800;">下划线</span>开头,只做引入使用
- 引入方式@import "文件名（不带下划线）"
```scss
//_part1.sass
body{
  color:red;
}

//index.sass
@import "part1"

```
#### 2. 嵌套导入
```scss
```
#### 3. 原生css的导入
- 1. 被导入的文件名以css结尾
- 2. 被导入文件名是一个URL地址（例如：http://xxx/a.css）
- 3. 被导入文件名是CSS的url()值
```scss
```
### （4）继承
- 关键字 @extend 类名
```scss
.d1{
  color:red;
}
.container{
  @extend .d1;
  font-size:20px;
}
```
#### 1. 多个继承
- 关键字 @extend 类名1,类名2
```scss
.d1{
  color:red;
}
.d2{
  background:red;
}
.container{
  @extend .d1, .d2;
  font-size:20px;
}
```
#### 2. 链式继承
- 关键字 @extend 类名1,类名2
```scss
.one{
  color:red;
}
.two{
  @extend .one
  background:red;
}
.three{
  @extend .two;
  font-size:20px;
}
```

