# css特性
*
    * [css规则](#1)
    * [实现三栏布局的多种方式](#2)

<h2 id=1> css规则 </h2>

- @charset 和 @import （元数据）
- @media 或 @document (条件信息，又被称为嵌套语句)
- @font-face（描述性信息）

使用示范： 该@-规则向当前的CSS导入其他CSS文件中
```css
    @import  ‘index.css’
```
嵌套语句，具体语法 （@media) 也叫媒体查询。
```css
@media (min-width: 700px) {
    body {
        background-color:red;
    }
}
```

### 属性选择器 

### 存在和值属性选择器 
- [attr] :该选择器选择包含attr 属性的所有元素。不论attr的值是什么。
- [attr=val] :该选择器仅仅选择的是attr属性被赋值为val的元素。
- [attr~=val] : 该选择器仅选择具有attr属性的元素。而且要求val的值是包含被空格分隔的取值列表里的一个。

### 子串值属性选择器 （也称为伪正则选择器） 
- [attr|=val]: 选择attr属性的值是val 或者val- 开头的元素，val- 中的-不是❌，而是用来处理编码语言。
- [attr^=val]:选择attr 属性的值以val开头。
- [attr$=val]:选择attr 属性的值以val结尾。
- [attr*=val]: 选择attr 属性的值包含在字符串val的元素中。


### 盒子模型中的一些问题 

### 盒子外边距重叠问题 （记住重叠只会发生在上下，不会发生在左右）
- 相邻元素之间的重叠。 （可以给第二个盒子浮动）
- 父子元素之间发生重叠，如果父子元素只有单纯的父子元素之间的关系，不存在边框，内边距，行内内容，也没创建BFC上下文，也没有浮动。那么两者是存在外边距会发生重叠的现象。代码可测～
- 空的块级元素。就是说该元素不存在边框，内容，等及浮动的话，有一定宽高，就非常容易的形成外边距重叠的效果。
- 在使用的时候，需要特别注意这个外边距重叠的事情，搞不好就带来布局的紊乱。
- [具体参考，点我](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)


<h2 id=2> 实现三栏布局的多种方式<h2>

### 方式1 流体布局 
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .left {
            float: left;
            width: 100px;
            height: 200px;
            background-color: red;
        }

        .right {
            float: right;
            width: 100px;
            height: 200px;
            background-color: pink;
        }

        .main {
            height: 200px;
            margin-left: 120px;
            margin-right: 120px;
            background-color: purple;
        }
    </style>
</head>

<body>
    <div class="content">
        <div class="left"></div>
        <div class="right"></div>
        <div class="main"></div>
    </div>
</body>

</html>
```
#### 注意使用这个方式的弊端，由于自己的架构设计的是流体布局，看结构就可以知道，优先加载left ,right。 最后加载main。

### 方式2 BFC三栏布局
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .left{
            float: left;
            margin-right: 20px;
            width: 100px;
            height: 200px;
            background-color: red;
        }
        .right{
            float: right;
            margin-left: 20px;
            width: 100px;
            height: 200px;
            background-color: pink;
        }
        .main{
            height: 200px;
            background-color: purple;
            /* 点睛之笔 */
            overflow: hidden;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="left"></div>
        <div class="right"></div>
        <div class="main"></div>
    </div>
</body>
</html>
``` 
#### 这种方式跟第一种方式非常相似，也同样存在的一样的不足，影响用户体验度。

### 方式3  双飞翼布局 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .content{
            float: left;
            width: 100%;
        }
        .main{
            height: 200px;
            margin-left: 120px;
            margin-right: 120px;
            background-color: red;
        }
        .main::after{
            content: "";
            clear: both;
            height: 0;
            visibility: hidden;
            overflow: hidden;
        }
        .left{
            float: left;
            margin-left: -100%;
            width: 100px;
            height: 200px;
            background-color:pink;
        }
        .right{
            float: right;
            margin-left: -200px;
            width: 100px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="main"></div>
    </div>
    <div class="left"></div>
    <div class="right"></div>
</body>
</html>
``` 
#### 这样做的好处是，使得主体内容优先加载。利用的是浮动元素的负值影响。

### 方式4 圣杯布局 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .content{
            margin-right: 120px;
            margin-left: 120px;
        }
        .main{
            float: left;
            width: 100%;
            height: 200px;
            background-color: red;
        }
        .left{
            position: relative;
            top: 0;
            left:-120px; 
            float: left;
            margin-left: -100%;
            width: 100px;
            height: 200px;
            background-color: pink;
        }
        .right{
            position: relative;
            top: 0;
            right: -120px;
            float: left;
            margin-left: -100px;
            width: 100px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="main"></div>
        <div class="left"></div>
        <div class="right"></div>
    </div>
</body>
</html>
```
#### 圣杯布局跟双飞翼布局非常相似

### 方式5 flex布局 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .content{
            display: flex;
        }
        .left{
            float: left;
            margin-right: 20px;
            width: 100px;
            height: 200px;
            background-color:pink;
        }
        .right{
            float: right;
            margin-left: 20px;
            width: 100px;
            height: 200px;
            background-color:pink;
        }
        .main{
            flex: 1;
            height: 200px;
            background-color: red;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="left"></div>
        <div class="main"></div>
        <div class="right"></div>
    </div>
</body>
</html>
```
#### 该方式简洁明了好用，需要注意兼容性。

### 方式6  table布局 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .content{
            width: 100%;
            display: table;
        }
        .left,.right,.main{
            display: table-cell;
        }
        .left{
            width: 100px;
            height: 200px;
            background-color: pink;
        }
        .right{
            width: 100px;
            height: 200px;
            background-color: pink;
        }
        .main{
            background-color: red;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="left"></div>
        <div class="main"></div>
        <div class="right"></div>
    </div>

</body>
</html>
```
#### 该方式虽然简单，但是无法设置中间栏的间距。。

### 方式4 绝对定位方式 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .content{
            position: relative;
        }
        .left{
            position: absolute;
            top: 0;
            left: 0;
            width: 100px;
            height: 200px;
            background-color:pink;
        }
        .right{
            position: absolute;
            top: 0;
            right: 0;
            width: 100px;
            height: 200px;
            background-color: pink;
        }
        .main{
            margin-right: 120px;
            margin-left: 120px;
            height: 200px;
            background-color: red;
        }
    </style>
</head>
<body>
    <div class="content">
        <div class="left"></div>
        <div class="main"></div>
        <div class="right"></div>
    </div>
</body>
</html>
``` 
#### 刚方式通俗易懂！