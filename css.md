# css篇

### 1. 盒模型
- IE盒模型 `boredr-box` ； 宽度 = 内容 + padding + border
- 标准盒模型 `content-box` ；宽度 = 内容
- 通过 `box-sizing`设置
</br>

### 2. BFC
- 什么是BFC
    * 意思就是块级格式化上下文，相当于一个容器，容器内部的元素不会影响到容器外部的元素
- 有什么特点
    * 是一个块级元素，在垂直方向依次排序
    * 属于同一个BFC的两个盒子，margin会发生重叠
- 如何创建BFC
    * 给父元素设置以下任意样式
    * overflow：hidden
    * display：flex \ inline-flex \ inline-block
    * position: absolute \ fixed
- BFC有什么作用
    * 解决父元素没有高度时，子元素浮动会使父元素高度塌陷
    * 解决子元素外边距使父元素塌陷问题
</br>

### 3. css垂直居中
- flex布局
```
    display:flex;
    justify-content:center;
    align-items:center;
```
- 定位
```
    position:relative;
    left:50%;
    right:50%;
    transform:translate(-50%,-50%);
```
</br>

### 4.Flex布局
- 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
- 父容器相关属性
    * `flex-direction` 主轴方向，row、column 
    * `justify-content` 水平对齐方式 flex-statr\flex-end\center\space-between
    * `align-items` 竖直方向对齐
    * `flex-wrap`
- 子元素相关属性
    * `align-self` 子元素在交叉轴上的对齐方式
    * `flex-grow` 子元素的放大比例，如果是 0 表示存在剩余空间也不放大
    * `flex-shrink` 子元素缩小比例，默认1，如果空间不够，项目缩小，如果是0，不缩小
    * `flex-basis` 定义在分配剩余空间之前，占据的空间，浏览器根据这个属性，计算是否有多余空间，默认为`auto`，即本来的大小
- flex：属性是 `flex-grow` 、`flex-shrik`、`flex-basis`，默认是 0 1 auto
- flex:0 ==> flex: 0 1 0%
- flex: none ==> flex: 0 0 auto
- flex: 1具体代表什么，有什么利用场景
    * ==> flex: 1 1 0%
- flex: 0、flex: 1、flex: none、flex: auto，示意什么意思，并利用在什么场景下应用？
</br>

### 5.伪类和伪元素
- 伪元素：是用来创建一些不存在在文档树中的元素，并为其添加样式。比如说，我们可以通过`:before`来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。
- 伪类：用于对与已经存在的元素处于某个状态时为其添加对应的样式。比如说，当用户悬停在指定的元素时，我们可以通过`:hover`来描述这个元素的状态。虽然它和普通的css类相似，可以为已有的元素添加样式，但是它只有处于dom树无法描述的状态下才能为元素添加样式，所以将其称为伪类。
- 区别：
    * 伪元素是创建一个文档外的元素
    * 伪类的操作对象是文档中存在的元素
    * 主要区别是：有没有创建一个文档外的元素

- 伪元素
    * ::after / ::before / ::first-line / ::placeholder / ::section
```
    .one::after{
        content:"插入的内容";
        color:red;
        font-size:14px;
    }
```
- 伪类
    * :active / :hover / :link / :visited / :nth-child() / :focus / :first-child
</br>

### 6.px、em、rem、rpx
- px 绝对长度单位
- em 相对长度，相对于父元素
- rem 相对长度，相对于html根元素
- vh 相对于屏幕高度的大小，相对单位
- vw 相对于屏幕宽度的大小
</br>
