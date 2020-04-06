### less学习笔记之bootstrap

#### 目录说明

##### 源代码里面的目录是这样的(只给出部分)：

	 .csscomb.json
	│  .csslintrc
	│  alerts.less
	│  badges.less
	│  bootstrap.less
	│  breadcrumbs.less
	│  button-groups.less
	│  buttons.less
	│  carousel.less
	│  close.less
	│  ...
	│
	└─mixins
		alerts.less
		background-variant.less
		border-radius.less
		buttons.less
		center-block.less
		clearfix.less
		forms.less
		gradients.less
		...

#### 文件 `variables.less`

顾名思义，为整个Bootstrap定义的全局变量。

    知识点一：
    Less的作用域和css很相似，变量和混合（mixins）在当前文件无法找到时会继承父级作用，如果任然没有找到则会编译抛出异常。

##### 定义在Bootstrap中使用的灰色和品牌颜色。

	@gray-base:  #000;
	@gray-darker:lighten(@gray-base, 13.5%);
	@gray-dark:  lighten(@gray-base, 20%); 
	@gray:   lighten(@gray-base, 33.5%); // #555
	@gray-light: lighten(@gray-base, 46.7%); 
	@gray-lighter:   lighten(@gray-base, 93.5%); 

	// 这部分定义的主要色：成功、失败、警告等等。
	@brand-primary:         darken(#428bca, 6.5%); // #337ab7
	@brand-success:         #5cb85c;
	@brand-info:            #5bc0de;
	@brand-warning:         #f0ad4e;
	@brand-danger:          #d9534f;

具体体现：

![](https://i.imgur.com/TrXTQaW.png)

    知识点2：函数 lighten与darken
    描述： 参数：`color, amount, method`
    功能： `color + amount` 表示在`color`的基础上，
    变得更淡或者更深,加上method后表示在`color * method` 的基础上，变淡/深 amount，
    这里先不解释less中色值是怎么计算的。

之后也定义了默认背景色。

##### 定义字体风格

###### 字体大小定义
Bootstrap的基础字体为14px。

    知识点3：函数ceil和floor
    简单说明：分别为向上取整和向下取整

###### 输入框风格定义
	
	cursor：not-allowed //当button与input处于disabled时，鼠标指针的样式

##### 优先级定义
设置一些默认层级优先级z-index，用于特定的组件，例如：navbar、dropdown、popover、modal-background、tooltip、navbar-fixed。值都为1000+，这也就是有时候我们自己自定义了一些优先级，但是还是达不到效果，可以想一想是不是值不够？

###less的坑
在实际的运用less中，遇到的问题。
1、就是使用`calc(100% - 50px)`，发现有时候通过调试看到不是像自己写的一样，这时候需要使用`~`进行处理 如： `calc(~"100% - 50px")`(一定要用双引号)。

2、在页面调试的时候，有时候对颜色增加透明度或者其操作，得到的色值不是#六位，而是#八位，对于#八位，在编译后不一定能够正确展示在页面，这个时候就需要使用less的函数`fade`。fade(#******, n%).