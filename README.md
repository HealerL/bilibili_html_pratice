# bilibili首页实战总结

花了两天时间将B站首页尝试写了一下，发现B站基本上都是由弹性flex布局搭建的，也借这个机会学习了弹性布局，用起来真是非常方便。

为弹性布局的元素指定了宽度之后，就会按照想要的方式进行排列。



以下为遇到的一些重要知识点总结：



## 表头导航栏部分

![image-20210722172957298](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722172957298.png)

B站首页头部分是用视频做背景的

这里是通过`<video>`插入视频，然后将导航部分进行绝对定位设置一个z-index来实现视频背景的导航栏。

```html
<video src="images/66eef632-c1e7-4a37-8715-171b2d8b8675.webm" height="158" autoplay="autoplay" loop="loop" muted> </video>
```

这里遇到的一个点是，刚开始设置了`autoplay`属性但视频依然没有自动播放，查找资料发现chrome6以及更高的版本不允许媒体自动播放，但可以通过将视频静音，就可以实现自动播放。

[Video在网页和移动端无法自动播放问题]: https://blog.csdn.net/zhouzuoluo/article/details/89336349



因为做这部分没有参照官方的代码，所以没有使用弹性布局来实现，通过将导航栏`<nav>`分为三部分，navleft和input区域，navright三块，给三个部分设定了35%，30%， 35%的宽度并进行绝对定位，并加入了`min-width`属性以保证左右侧导航栏在窗口缩小时不会出现换行问题。

![image-20210722175720008](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722175720008.png)





### 导航栏部分

左侧导航栏通过一堆`<div>`来实现

<img src="https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722175750377.png" alt="image-20210722175750377" style="zoom:50%;" />

使用这个方法是之前做百度首页的时候，它的导航栏就是通过这种一堆`<div>`的结构来实现的。

不过百度是通过`display:inline-block`来将这些块元素排列在同一行，我这里采用了`float`。

中间的搜索区部分通过一个`<input>`和一个`<button>`来实现。

右侧的个人导航部分尝试了一下`<span>`来实现，也是在百度首页练习时，它们的页尾是这样做的也就尝试了一下，省去了进行`float`。



B站官方则是通过弹性布局来实现的。

### 搜索框图标

![image-20210722174359629](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722174359629.png)

B站官方的搜索框图标通过一些不太了解的`<i>`元素来`::before`插入实现，并不是很懂具体是怎么实现的，猜测与官方设计好的的图标库来实现。

网上查了一下，通过为搜索按钮所在的`<button>`添加一个搜索图标的背景图片来实现。

![image-20210722174726016](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722174726016.png)

第二个图为自己实现的版本，

```css
.icon {
    float: left;
    height: 34px;
    border: 1px solid transparent;
    border-radius: 0 4px 4px 0;
    padding: 0;
    width: 40px;
    background: url(images/search.png) center center no-repeat;
    background-color: silver;
}
```

通过float来与左侧的输入框拼接对齐。





最后在表头部分通过绝对布置在指定位置插入logo。

![image-20210722181019584](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722181019584.png)



这里还注意到了当文字在图片上方时，B站会将一个渐变的类似于图层的东西，之后进行了尝试。



## 中间目录部分

这部分尝试了弹性布局，实现起来比较轻松。

![image-20210722181440974](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722181440974.png)

先大布局将这一行分为三部分，进行布局；再对每个部分的内部进行弹性布局。

[Flex布局教程]: https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html



其中有个注意的点，

中间部分这种结构的实现：

![image-20210722181932568](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722181932568.png)

这种上下一一对齐的情况，是通过在弹性布局中，选择按列布置并允许换行来实现的

```css
#catmid {
    display: flex;
    /* 使用列方向布局，可以保证上下一一对齐，并允许换行 */
    flex-direction: column;
    flex-wrap: wrap;
    height: 68px;
    width: 550px;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
```

而对于每个小的![image-20210722182233825](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722182233825.png)通过为数字部分应用背景颜色字体大小等样式来实现。

右侧部分同样通过按列的弹性布局来实现。





### 边界部分

两个部分之间的边界，如下所示

![image-20210722182935969](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722182935969.png)

通过在两个部分之间添加一个

```html
 <span class="tabline"></span>
```

并为其添加一侧的边框

```css
span.tabline {
    margin: 0 8px;
    /* 必须要添加一个高度，才能使边框生效 */
    height: 46px;
    border-left: 1px solid #e7e7e7;
}
```



## 视频推荐部分

视频推荐部分同样通过弹性布局来实现。

![image-20210722182605303](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210722182605303.png)

将这个总体按弹性布局分成左右两个部分，左边为可操作的轮播图，右侧为六个推荐视频的小窗口。

<img src="https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723093239131.png" alt="image-20210723093239131" style="zoom: 67%;" />

### 轮播图

左侧的轮播图部分，查阅资料显示横向的轮播图需要先将图片float横向依次排列好之后，为包括它们的元素设置一个只能容纳一张图片的宽度，再加入动画效果依次向左移动实现，但由于这里使用弹性布局具体操作起来有点问题，尝试了几次没有成功，再加上还没有接触JavaScript，所以暂时没有实现自动播放的效果。



只是实现了一个点击右下角按钮可以切换图片的效果。

![image-20210723093919017](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723093919017.png)

![image-20210723093940049](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723093940049.png)

#### 按钮切换图片

- 首先在上方定义三个单选框input代表三个输入
- 在下方定义三个与input的id对应的label元素

```html
 	<input type="radio" name="button" value="0" id="pic1" checked />
	<input type="radio" name="button" value="1" id="pic2" />
	<input type="radio" name="button" value="2" id="pic3" />
	
	三个结构储存三个图片
	<div id="change_pic">
    	<div id="change1" class="change">
             ...               
        </div>
        <div id="change2" class="change">
             ...             
        </div>
        <div id="change3" class="change">
             ...        
        </div>

     </div>
                   
      <div id="tragger">
            <label for="pic1"></label>                    
            <label for="pic2" id="label2"></label>     
            <label for="pic3"></label>                      
	   </div>
```

- 将label元素显示成圆形形状

  ```css
  #tragger label {
      display: inline-block;
      width: 10px;
      height: 10px;
      background-color: #fff;
      border-radius: 50%;
      margin: 0 3px;
      border: 2px solid #fff;
  }
  ```

  此时效果：

  ![image-20210723094845310](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723094845310.png)

- 将input的单选框进行隐藏，然后将input的选择与label的样式进行关联，当选择一个选项时，将对应的label的背景颜色进行改变。

```css
#midwrap_left input[type="radio"]:nth-of-type(1):checked ~ #tragger label:nth-of-type(1) {
    background-color: #FB7299;
}
#midwrap_left input[type="radio"]:nth-of-type(2):checked ~ #tragger label:nth-of-type(2) {
    background-color: #FB7299;
}
#midwrap_left input[type="radio"]:nth-of-type(3):checked ~ #tragger label:nth-of-type(3) {
    background-color: #FB7299;
}
```

- 最后将input的选择与图片的层叠顺序进行关联

  ```css
  #midwrap_left input[type="radio"]:nth-of-type(1):checked ~ #change_pic div:nth-of-type(1) {
      /* transform: translate3d(0px, 0px, 0px); */
      z-index: 20;
  }
  
  
  #midwrap_left input[type="radio"]:nth-of-type(2):checked ~ #change_pic div:nth-of-type(2) {
      /* transform: translate3d(0px, 0px, -2px); */
      z-index: 20;
  }
  
  #midwrap_left input[type="radio"]:nth-of-type(3):checked ~ #change_pic div:nth-of-type(3) {
      /* transform: translate3d(0px, 0px, -2px); */
      z-index: 20;
  }
  ```

  图片原本是绝对定位到同一个位置，然后为他们设置了不同的z-index，所以这里就提高选中图片的z-index来显示选中图片。



#### 文字的阴影背景

![image-20210723100047646](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723100047646.png)

图片下方有一层阴影，增加对比度，字更加清晰可见。

之前尝试加一个内部阴影来实现，但阴影却不显示，原因未知。



参照B站官方通过添加一个宽度1像素的渐变图来实现，该图片如下所示，一直重复延伸。

![bg](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/bg.png)



```css
.change::before {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 48px;
    background-image: url(images/bg.png);
    background-size: contain; 重复延伸
    border-radius: 2px;
    content: "";
}
```



### 右侧推荐栏

同样使用弹性布局，并且按列对齐。



设置了一个鼠标悬停时，弹出的一个元素

![image-20210723102418799](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723102418799.png)

通过设置一个隐藏的元素，它的z-index更高但只在鼠标悬停时显示，并且设置一个透明度。

这里注意到B站的效果是鼠标悬停时，下方的标题弹上来占据整个图片，这个效果有点复杂，没有进行尝试。





## 专栏部分

专栏部分同样用弹性布局分为两块，再进行内部的布置。

![image-20210723103532550](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723103532550.png)



左侧的动画区跟之前做法类似，弹性布局按列排列。



### 超出部分的文本省略号表示



这里遇到一个点是实现`text-overflow: ellipsis`效果，类似于推荐部分中将超出的文本用省略号表示

![image-20210723103828400](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723103828400.png)

推荐部分采用的方法是

```css
  /* 要使段落不换行，并且设置一定的宽度，才能进行截断，并且截断属性只对块元素有用 */
    width: 100%;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
```

但这种做法对于弹性布局的元素不起作用，查阅资料发现有两种方法可以实现：

1. 在设定宽度的时候，设置一个为0的最小宽度，如下：

   ```css
   .rank_num p.rank_list_title {
       margin: 0;
       /* flex布局下显示省略号效果需要补充设置一个宽度和为0的最小宽度，或者之前那个限制行数的方法 */
       text-overflow: ellipsis;
       white-space: nowrap;
       overflow: hidden;
       width: 235px;
       min-width: 0;
   }
   ```

   实现了排行榜中的省略号效果：

   ![image-20210723104239308](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723104239308.png)

2. 第二个方法则是参照B站官方，声明最多显示两行的方法:

   ```css
   display: -webkit-box;
   -webkit-line-clamp: 2; 最多两行
   -webkit-box-orient: vertical; 按垂直方向计算行数
   ```

   这三个属性要同时使用

![image-20210723104543897](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723104543897.png)



排行榜部分同样使用弹性布局来实现，设定好宽度后会自动对齐。





# 最终效果

![image-20210723104912130](https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723104912130.png)





## 问题

注意到有些图标是通过`<path>`元素画出来的，没有做进一步的了解。



<img src="https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723105120652.png" alt="image-20210723105120652" style="zoom:50%;" />



还有搜索框在窗口变小时的隐藏也还不知道原理。

<img src="https://raw.githubusercontent.com/HealerL/Typora_img/main/images/image-20210723105337174.png" alt="image-20210723105337174" style="zoom:67%;" />

