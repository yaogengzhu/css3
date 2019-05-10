# css特性

## css规则 
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

## 属性选择器 

### 存在和值属性选择器 
- [attr] :该选择器选择包含attr 属性的所有元素。不论attr的值是什么。
- [attr=val] :该选择器仅仅选择的是attr属性被赋值为val的元素。
- [attr~=val] : 该选择器仅选择具有attr属性的元素。而且要求val的值是包含被空格分隔的取值列表里的一个。

### 子串值属性选择器 （也称为伪正则选择器） 
- [attr|=val]: 选择attr属性的值是val 或者val- 开头的元素，val- 中的-不是❌，而是用来处理编码语言。
- [attr^=val]:选择attr 属性的值以val开头。
- [attr$=val]:选择attr 属性的值以val结尾。
- [attr*=val]: 选择attr 属性的值包含在字符串val的元素中。


## 盒子模型中的一些问题 

### 盒子外边距重叠问题 （记住重叠只会发生在上下，不会发生在左右）
- 相邻元素之间的重叠。 （可以给第二个盒子浮动）
- 父子元素之间发生重叠，如果父子元素只有单纯的父子元素之间的关系，不存在边框，内边距，行内内容，也没创建BFC上下文，也没有浮动。那么两者是存在外边距会发生重叠的现象。代码可测～
- 空的块级元素。就是说该元素不存在边框，内容，等及浮动的话，有一定宽高，就非常容易的形成外边距重叠的效果。
- 在使用的时候，需要特别注意这个外边距重叠的事情，搞不好就带来布局的紊乱。
- [具体参考，点我](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)