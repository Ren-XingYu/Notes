# NPM

```
# npm全局设置
npm config set prefix D:\nodejs
npm config set registry https://mirrors.huaweicloud.com/repository/npm/
npm cache clean -f

# 安装vue脚手架
npm install -g @vue/cli
vue --version

# 创建vue项目
命令行：vue create project_name
图形界面：vue ui
npm run serve
```

# HTML

```html
<!-- 注释 -->

<h1>
    标题
</h1>

<p>
    段落
</p>

<br>

<hr>

<strong>加租</strong>
<b>加粗</b>

<em>倾斜</em>
<i>倾斜</i>

<ins>下划线</ins>
<u>下划线</u>

<del>删除线</del>
<s>删除线</s>

<img src="" alt="替换文本" title="提示文本" width="" height="">

<a href="https://cn.bing.com/" target="_blank">Bing</a>

<!-- 支持格式:mp3、ogg、wav -->
<!-- 为了提升用户体验,浏览器一般会禁用自动播放 -->
<!-- html5中,属性名和属性值完全一样,可以简写为一个单词 -->
<audio src="" controls loop autoplay></audio>

<!-- 支持格式:mp4、webm、ogg -->
<!-- 为了提升用户体验,浏览器支持在静音状态下自动播放 -->
<video src="" controls loop muted autoplay></audio>

<!-- 无序列表 -->
<ul>
    <li>第一项</li>
    <li>第二项</li>
</ul>

<!-- 有序列表 -->
<ol>
    <li>第一项</li>
    <li>第二项</li>
</ol>

<!-- 定义列表 -->
<dl>
    <dt>列表标题</dt>
    <dd>列表详情一</dd>
    <dd>列表详情二</dd>
</dl>

<table border="1">
    <thead>
        <tr>
            <th>姓名</th>
            <th>语文</th>
            <th>数学</th>
            <th>总分</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>99</td>
            <td rowspan="2">100</td>
            <td>199</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>98</td>
            <td>198</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>总结</td>
            <td colspan="3">全市第一</td>
        </tr>
    </tfoot>
</table>

<!-- label标签可以增大点击范围 -->
<!-- 写法一 -->
1、lable标签只包裹内容,不包裹表单控件
2、设置label标签的for属性值和表单控件的id属性值相同

<!-- 写法二 -->
使用label标签包裹文字和表单控件,不需要属性

<form action="" method="">
    <table>
        <tr>
            <td>用户名:</td>
            <td><input type="text" name="username" placeholder="请输入用户名"></td>
        </tr>
        <tr>
            <td>密码:</td>
            <td><input type="password" name="password" placeholder="请输入密码"></td>
        </tr>
        <tr>
            <td>性别:</td>
            <td>
                <input type="radio" name="gender" value="male" checked id="male"><label for="male">男</label>
                <label><input type="radio" name="gender" value="female">女</label>
            </td>
        </tr>
        <tr>
            <td>兴趣爱好:</td>
            <td>
                <input type="checkbox" name="hobby" value="walk" checked>徒步
                <input type="checkbox" name="hobby" value="bike">骑行
            </td>
        </tr>
        <tr>
            <td>城市:</td>
            <td>
                <select name="city">
                    <option value="beijing">北京</option>
                    <option value="shanghai" selected>上海</option>
                    <option value="shanghai">广州</option>
                    <option value="shenzhen">深圳</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>文件:</td>
            <td>
                <input type="file" multiple>
            </td>
        </tr>
        <tr>
            <td>个人简介:</td>
            <td><textarea name="comment"></textarea></td>
        </tr>
        <tr>
            <td>
                <button type="submit">提交</button>
                <button type="reset">重置</button>
                <button type="button">按钮</button>
            </td>
        </tr>
    </table>
</form>

<div>
    div
</div>

<span>
	span
</span>

<!-- 空格 -->
&nbsp;

<!-- 小于 -->
&lt;

<!-- 大于 -->
&gt;
```

# CSS

## 清除默认样式

```css
* {
	margin: 0;
    padding: 0;
    box-sizing: border-box;
}
li {
    list-style: none;
}
a {
    text-decoration: none;
}
```

## 引入方式

- 内部样式表

  ```html
  <head>
      <title>CSS</title>
      <style>
          p {
              color: red;
          }
      </style>
  </head>
  <body>
      <p>CSS</p>
  </body>
  </html>
  ```

- 外部样式表

  ```html
  <head>
      <title>CSS</title>
      <link rel="stylesheet" href="./index.css">
  </head>
  <body>
      <p>CSS</p>
  </body>
  </html>
  ```

  ```css
  p {
      color: red;
  }
  ```

- 行内样式表

  ```css
  <p style="color: red;">CSS</p>
  ```

## 选择器

- 标签选择器

  ```html
  <head>
      <title>CSS</title>
      <style>
          p {
              color: red;
          }
      </style>
  </head>
  <body>
      <p>CSS</p>
  </body>
  </html>
  ```

- 类选择器

  ```html
  <head>
      <title>CSS</title>
      <style>
          .red {
              color: red;
          }
      </style>
  </head>
  <body>
      <p class='red'>CSS</p>
  </body>
  </html>
  ```

- id选择器(一般配合JavaScript使用,很少用来设置CSS样式)

  ```html
  <head>
      <title>CSS</title>
      <style>
          #red {
              color: #ff0000;
          }
      </style>
  </head>
  <body>
      <p id='red'>CSS</p>
  </body>
  </html>
  ```

- 通配符选择器(页面的所有标签)

  ```html
  <head>
      <title>CSS</title>
      <style>
          * {
              color: #ff0000;
          }
      </style>
  </head>
  <body>
      <p>p</p>
      <div>div</div>
  </body>
  </html>
  ```

- 复合选择器

  ```css
  /* 后代选择器：选中div下面的所有span,包括儿子、孙子、重孙子... */
  div span {
      color: red;
  }
  
  /* 子代选择器：只选中div下面的儿子span */
  div > span {
      color: red;
  }
  
  /* 并集选择器：多组标签设置相同样式 */
  div,p {
      color: red;
  }
  
  /* 交集选择器：选中同时满足多个条件 */
  div.box {
      color: red;
  }
  
  /* 伪类选择器：伪类表示元素状态,选中元素的某个状态设置样式 */
  a:hover {
      color: red;
  }
  
  /* 超链接的伪类: 需要按照LVHA的顺序书写 */
  :link 访问前
  :visited 访问后
  :hover 鼠标悬停
  :active 点击时(激活)
  ```

- 结构伪类选择器

  ```html
  <head>
      <title>CSS</title>
      <style>
          li:first-child {
              background-color:red;
          }
          li:last-child {
              background-color:green;
          }
          /* 选中第3个 */
          li:nth-child(3) {
              background-color:blue;
          }
          /* 选中偶数标签 */
          li:nth-child(2n) {
              background-color:yellow;
          }
      </style>
  </head>
  <body>
      <ul>
      	<li>li 1</li>
          <li>li 2</li>
          <li>li 3</li>
          <li>li 4</li>
          <li>li 5</li>
      </ul>
  </body>
  </html>
  ```

- 伪元素选择器

  ```html
  <head>
      <title>CSS</title>
      <style>
          div::before {
              content: "老鼠"
          }
          div::after {
              content: "大米"
          }
      </style>
  </head>
  <body>
      <div>
          爱
      </div>
  </body>
  </html>
  ```

  

## 字体修饰属性

```css
字体大小	font-size	chrome浏览器默认字体大写为16px
字体粗细	font-weight   400:正常(normal) 700:加粗(bold)
字体倾斜	font-style	normal:正常 italic:倾斜
行高		line-height	写法1:数字+px 写法2:数字(当前标签font-size属性值的倍数)  行高=上间距+文本高度+下间距
字体族		font-family 楷体
字体符合属性	font	写法:是否倾斜 是否加粗 字号/行高 字体  例子: italic 700 30px/2 楷体;
文本缩进	text-indent 写法1:数字+px 写法2:数字+em(推荐: 1em=当前标签的字号大小)
文本对齐	text-align 取值:left|center|right  本质是控制内容的对齐方式,属性要设置给内容的父级
修饰线		text-decoration 取值:none|underline|line-through|overline
颜色		color 取值:red|rgb(255,0,0)|rgba(255,0,0,0.5)|#FF0000
```

## 三大特性

- 继承性：子级默认继承父级的**文字控制属性**。

- 层叠性：相同的属性会覆盖，不同的属性会叠加。

- 优先级：通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < !important(提高权重或优先级到最高)

  ```css
  * {
      color: red !important;
  }
  ```

  - 叠加计算规则：如果有多个复合选择器，则需要权重叠加计算。

    ```
    (行内样式,id选择器个数,类选择器个数,标签选择器个数)
    ```

    - 从左向右依次比较选择器个数，同一级个数多的优先级高，如果个数相同，则向右比较。
    - !important 权重最高
    - 继承权重最低

## 背景属性

```css
背景色		background-color
背景图		background-image
背景图平铺方式		background-repeat 取值:no-repeat|repeat|repeat-x|repeat-y
背景图位置	background-position 属性值:水平方向位置  垂直方向位置 取值:left|right|center|top|bottom  或者:xxpx yypx 
背景图缩放	background-size 取值:cover|contain
背景图固定	background-attachment 取值:fixed
背景图复合属性		background 取值: 背景色 背景图 背景图平铺方式 背景图位置/背景图缩放 背景图固定
```

## 标签的显示模式

- 块级(block)：独占一行；宽高属性**生效**；默认宽度是父级的100%。例：div
- 行内(inline)：一行共存多个；宽高属性**不生效**；宽高由内容撑开。例：span
- 行内块(inline-block)：一行共存多个；宽高属性**生效**；宽高默认由内容撑开。例：img

```css
display: block;
```

## 盒子模型

- 内容区域 - width & height

  - **盒子尺寸=内容尺寸(width&height)+border尺寸+内边距尺寸**

  - 加border和内边距会撑大盒子，解决方式

    - 手动做减法 

    - 使用内减模式

      ```css
      box-sizing:border-box
      ```

- 内边距 - padding

  ```css
  padding: 上 右 下 左
  padding-top
  padding-right
  padding-bottom
  padding-left
  ```

- 边框线 - border

  ```css
  属性值：边框线粗细 线条颜色 颜色 (1px solid #000)
  border: 1px solid #000;
  border-top
  border-right
  border-bottom
  border-left
  ```

- 外边距 - margin

  ```css
  margin: 上下(0) 左右(auto)
  margin-top
  margin-right
  margin-bottom
  margin-left
  margin: 上 右 下 左
  ```
    ```
  外边距问题-合并现象
  场景：垂直排列的兄弟元素，上下margin会合并
  现象：取两个margin中的较大值
  
  外边距问题-塌陷问题
  场景：父子级的标签,子级标签添加上边距会产生塌陷问题
  现象：导致父级一起向下移动
  解决方式：
  	1、取消子级margin,父级设置padding
  	2、夫级设置overflow:hidden
  	3、父级设置border-top
  	
  行内元素-内外边距问题
  场景：行内元素添加margin和padding，无法改变元素垂直位置(可以改变水平位置)
  解决方法：给行内元素添加line-height可以改变垂直位置
    ```


- 元素溢出

  ```css
  overflow: hidden|scroll(无论是否溢出都显示滚动条)|auto(溢出才显示滚动条)
  ```


- 圆角

  ```css
  /* 如果分别设置:从左上角顺时针赋值，没有取值的角与对角取值相同 */
  border-radius: 数字+px|百分比(最大值50%)
  ```

- 阴影

  ```css
  box-shadow: x轴偏移量 y轴偏移量 模糊半径 扩散半径 颜色 内外阴影
  ```

## 布局

标准流：也叫文档流，指的是标签在页面中默认的排布规则。例如：块元素独占一行，行内元素可以一行显示多个。

- 浮动：浮动之后的盒子会脱离标准流。

  ```css
  float:left|right
  ```

  清除浮动的方式：

  - 额外标签法：在**父**元素**内容的最后**添加一个**块**级元素，设置CSS属性`clear:both`

  - 单伪元素法

    ```css
    .clearfix::after {
        content: "";
        display: block;
        clear: both;
    }
    ```

  - 双伪元素法(推荐)

    ```css
    .clearfix::before,
    .clearfix::after {
        content: "";
        display: table;
    }
    
    .clearfix::after {
        clear: both;
    }
    ```

  - overflow：**父**元素添加CSS属性`overflow:hidden`

- Flex布局：也叫弹性布局，是浏览器提倡的布局模型，非常适合结构化布局，提供了强大的空间分布和对齐能力。Flext模型不会产生浮动布局中脱标现象，布局网页更简单、更灵活。

## Flex布局

设置方式：给父元素设置`display:flex`，子元素可以自动挤压或拉伸

默认情况下，主轴方向尺寸靠内容撑开；侧轴默认拉伸。

组成部分：

- 弹性容器
- 弹性盒子
- 主轴：默认在水平方向
- 侧轴 / 交叉轴：默认在垂直方向

| 描述                     | 属性            | 取值                                                         |
| ------------------------ | --------------- | ------------------------------------------------------------ |
| 创建flex容器             | display         | flex                                                         |
| 主轴对齐方式             | justify-content | flex-start \| flex-end \| center \| space-between \| space-around \| space-evenly |
| 侧轴对齐方式             | align-items     | stretch \| center \| flex-start \| flex-end                  |
| 某个弹性盒子侧轴对齐方式 | align-self      | stretch \| center \| flex-start \| flex-end                  |
| 修改主轴方向             | flex-direction  | row \| column \| row-reverse \| column-reverse               |
| 弹性伸缩比               | flex            | 作用：控制弹性盒子的主轴方向的尺寸。属性值：整数数字，表示占用父级剩余尺寸的份数。 |
| 弹性盒子换行             | flex-wrap       | wrap(换行) \| nowrap(不换行,自动挤压或拉伸,默认值)           |
| 行对齐方式               | align-content   | flex-start \| flex-end \| center \| space-between \| space-around \| space-evenly |

