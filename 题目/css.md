## 1. 如何解决a标点击后hover事件失效的问题?
这个问题其实就是CSS连接的排序问题，我没按规矩来只能点完之后清理IE缓存才又有效果，按下面这种顺序就完美解决这个问题了！

1. ```a:link```未访问的链接
2. ```a:visited``` 已访问的链接
3. ```a:focus```  获取焦点时的链接
4. ```a:hover``` 当有鼠标悬停在链接上
5. ```a:active```被选择的链接


**设置顺序**：link -> visited -> focus -> hover -> active

## 2.DIV+CSS布局的好处
1. 结构与样式分离，易于维护
2. **?** 代码量减少了，减少了大量的带宽，页面加载的也更快，提升了用户的体验
3. 对SEO搜索引擎优化更加友好，且H5又新增了许多语义化标签更是如此
4. 允许更多炫酷的页面效果，丰富了页面
5. 符合W3C标准，保证网站不会因为网络应用的升级而被淘汰

缺点: 不同浏览器对web标准默认值不同，所以更容易出现对浏览器的兼容性问题


## 3. px, em, rem有什么区别？
1. px 像素， 是一个确定的长度
2. em 是一个相对长度单位， 相对于父元素
3. rem 也是一个相对长度单位， 相对于根元素```<html>```

## 4. 写出5个css**伪类**选择器并分别解释
```:``` 单冒号是$\color{red}{伪类}$， 伪类用于向某些选择器添加特殊的效果

1. ```:link``` 未访问的链接
2. ```:visited``` 已访问的链接
3. ```:focus``` 被选中的元素
4. ```:hover``` 悬停在元素之上（在 CSS 定义中，a:hover 必须位于 a:link 和 a:visited 之后，这样才能生效！）
5. ```:active``` 被选择的链接（在 CSS 定义中，a:active 必须位于 a:hover 之后，这样才能生效！）
6. ```:first-child``` 目标元素的第一个子元素

```::``` 双冒号是$\color{red}{伪元素}$，伪元素用于将特殊的效果添加到某些选择器
1. ```::after``` 在目标元素后插入某些内容 
2. ```::before``` 在目标元素前插入某些内容

## 5. 简述一下flex
```flex```是 弹性布局  
基本属性有：
1. ```flex-direction``` 属性决定主轴的方向（即项目的排列方向）
2. ```flex-wrap``` 定义如何换行，默认nowrap；wrap，wrap-reverse（换行，第一行在下方）
3. ```flex-flow``` 上述属性简写
4. ```justify-content``` 主轴的方向
  flex-start（左对齐，默认） / flex-end（右对齐）
  center
  space-between 两端对齐
  space-around 项目两侧间隔相等

5. ```align-items``` 交叉轴的方向
6. ```align-content```

## 6. sass和scss是什么关系？
都是css预处理语言，scss是sass3引入的语法，完全兼容css3  
预处理语言有哪些功能，**变量**，**嵌套**，**混入mixins**，**继承**，**导入**

## 7. css怎么实现动画？
1. transition
2. animation: name duration timing-function delay iteration-count direction;
   1. name:keyframe的名称，也就是定义了关键帧的动画的名称,这个名称用来区别不同的动画。
   2. duration:完成动画所需要的时间（2s 或者 2000ms）
   3. timing-function:完成动画的速度曲线
   4. delay：动画开始之前的延迟
   5. iteration-count：动画播放次数
   6. direction：是否轮流反向播放动画（normal:正常顺序播放，alternate下一次反向播放）如果把动画设置为只播放一次，则该属性没有效果。

```html
<style>
div
{
    width:100px;
    height:100px;
    background:red;
    position:relative;
    animation:mymove 5s infinite;
    -webkit-animation:mymove 5s infinite; /*Safari and Chrome*/
}
 
@keyframes mymove
{
    1% {left:0px;}
    20%{left:200px;}
    50% {left:300px;}
    100%{left:200px;}
}
 
@-webkit-keyframes mymove /*Safari and Chrome*/
{
    1% {left:0px;}
    20%{left:200px;}
    50% {left:300px;}
    100%{left:200px;}
}
</style>
```

## 8. css居中有哪些方法？
1. 通过flex
2. 设置伪元素
```html
<div id="test">
  <div class="box"></div>
</div>
```
```css
 #test {
    background-color: blue;
    width: 100%;
    height: 600px;
  }
  .box {
    display: inline-block;
    vertical-align: middle;
    width: 100px;
    height: 100px;
    background-color: red;
    text-align:center;
  }
  #test::before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    height: 100%;
  }
  
```

3. margin:auto
```css
div{
  width: 400px;
  height: 400px;
  position: relative;
  border: 1px solid #465468;
 }
 img{
  position: absolute;
  margin: auto 0;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
 }
```

## 怎样才能形成BFC

float的值不能为none
overflow的值不能为visible
display的值为table-cell, table-caption, inline-block中的任何一个
position的值不为relative和static 



## CSS实现宽度自适应100%，宽高16:9的比例的矩形

```css
.box { width: 80%; }
/* padding撑开高度（根据宽度100%） */
.scale { width: 100%;  padding-bottom: 56.25%; height: 0; position: relative; }
.item { width: 100%; height: 100%; background-color: aquamarine; position: absolute; }
```
```html
<div class="box">
  <div class="scale">
    <p class="item">这是一个16：9的矩形</p>
  </div>
</div>
```



