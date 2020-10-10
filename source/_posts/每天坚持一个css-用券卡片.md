## 每天一个CSS-用券卡片

 [卡券效果预览地址](https://stackblitz.com/edit/angular-ivy-j4maow?file=src/app/app.component.less)
### 效果图

![image](https://segmentfault.com/img/bVbROH0)


### 疑问

圆角好画，但是缺圆角怎么画呢？

#### 思路

第一步：卡片背景色填充方便调试。【OK】
第二步：区分元素，按照上、中、下的顺序排序堆叠。【OK】
第三步：画圆缺角，也是比较重要的一步。【？】

### 知识点

#### background: radial-gradient

绘制圆缺角的重要知识点，背景色径向渐变。
[实现思路的源代码地址](https://stackblitz.com/edit/angular-ivy-s2cpr9?file=src/app/app.component.less)

##### 水平方向卡片，右上、右下缺圆角

1. 创建圆形，使用镜像渐变在方块上面创建一个圆。
2. 位置调整，调整圆的位置，将圆的中点调整至方块角处，形成凹圆角。` background:  radial-gradient(circle at right top, transparent 5px, #44C9A1 0);`
3. 绘制第二个圆、与调整位置，`background:  radial-gradient(circle at right bottom, transparent 5px, #44C9A1 0);`，发现得不到想要的效果。
4. 分析原因：background属性，默认100%大小与repeat展开，导致了重叠。
5. 设置背景:background:no-repeat;background-size： 100% 50%（水平方向 垂直方向），（这里设置50%在垂直方向，是因为准备在右上、右下方向设置缺角）。
6. 设置完成后发现得不到效果，最后发现忽略了一个重要的事情，就是背景展开方向，因为是垂直的，所以应该是从上右到左下与下右到左上。所以修改为background:  radial-gradient(circle at bottom right, transparent 5px, #44C9A1 0) bottom right;

##### 参数说明

radial-gradient(circle at bottom right, transparent 5px, #44C9A1 0) bottom right

circle: 圆形 。
at :位于 。
bottom right : 右下角 （可用% ,px等具有单位的数值表示） transparent: 圆的背景颜色以父元素为准。
5px: 圆的半径。  
#44c9A1：方块的背景颜色 
0 ：径向渐变的程度，值为0时，无渐变。当值大于0时，从圆心处径向渐变至方块。
bottom right：表示方块背景的起始位置。

#### filter: drop-shadow

会按照图形（不限于图片，三角形也可以）形状在图片背后进行投影，即当背景透明时按照图形轮廓边界投影阴影。[效果地址](https://angular-ivy-pur2rj.stackblitz.io)

效果图：
![image](https://segmentfault.com/img/bVcgSNR)

`区别 box-shadow,boxShadow是以元素边界轮廓进行投影`。

#### 浏览器支持

Chrome , FirFox, EDge 最新版本都支持,不考虑IE。

