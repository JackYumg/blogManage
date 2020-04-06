# less学习之`Bootstrap`按钮篇）

如我上一篇[less学习之Bootstrap](https://segmentfault.com/a/1190000017904706)里面，介绍了Bootstrap的目录结构，说明了在`variables.less`这个文件里面，定义了主题色，也包括了按钮的主题色。接下来看一看 `buttons.less`与 `mixins/buttons.less`.

## 文件 `buttons.less` 与 `mixins/buttons.less`

内容不是很多，总结下来就是：

1、“`.btn`”的基础样式定义。

2、按钮的各种状态含义的样式定义，例如：btn-primary、btn-success等。

3、伪连接，按钮的样式显示为连接的样式。

4、按钮尺寸的class：lg、sm、xs。

5、input类型的按钮定义。

### 基础样式定义
#### 代码：

	.btn {
	  display: inline-block;
	  ...
	  ...
	
	  &,
	  &:active,
	  &.active {
	    &:focus,
	    &.focus {
	      .tab-focus();
	    }
	  }
	
	 ...
	 ...// 余下的为hover、disabled时的样式
	}
 
#### 说明：

    知识点1：`&`在less与scss的语法中，表示同父级，就上一个例子来说，就是编译之后`&`将会被`.btn`替换，如果是多层时也是相同的道理。

	提示1：运用了函数tab-focus。此函数定义在mixins/tab-focus.less中，代码很短

	.tab-focus() {
	  outline: 5px auto -webkit-focus-ring-color;
	  outline-offset: -2px;
	}

	修改浏览器默认的大纲样式：表现在按钮、form表单等原生组件上。

	提示2：	运用了函数user-select。此函数定义在mixins/vendor-prefixes中。
	
	.user-select(@select) {
  	-webkit-user-select: @select;
     -moz-user-select: @select;
      -ms-user-select: @select; // IE10+
          user-select: @select;
	}

	作用，让文本是否能够选择。
	
	提示3： 运用了函数opacity。此函数定义在mixins/opacity中。

	.opacity(@opacity) {
	  opacity: @opacity;
	  @opacity-ie: (@opacity * 100);
	  filter: ~"alpha(opacity=@{opacity-ie})";
	}
	
	此处做了IE的兼容IE，IE的透明度采用 filtger:alpha(opacity=value),其中 0 <= value && value <= 100.
	符号“~”，可以意为JavaScript里面的 evel ，可以将字符串转化为表达式。所以说一些复杂的选择器也能够作为变量定义。

#### 特别说明
##### 函数`button-variant`
	Bootstrap里面抽象出来的函数，作用于按钮不同状态下的颜色变化。例如：hover、focus、active等状态。
	
函数说明

	参数：@color; @background; @border // 分别时字体颜色、背景颜色、边框颜色

结构如下：

	.button-variant(@color; @background; @border) {
	  color: @color;
	  background-color: @background;
	  border-color: @border;
	
	  &:focus,
	  &.focus {
	    color: @color;
	    background-color: darken(@background, 10%);
	        border-color: darken(@border, 25%);
	  }
	  
	 ...
	 ...// 余下的为hover、disabled时的样式

	  .badge {
	    color: @background;
	    background-color: @color;
	  }
	}

由上面两个文件可以总结出的结论是：*.less里面写的是选择器定义、变量定义，而mixins/*.less里面写的是函数。

### 本篇总结
关于Bootstrap听到过不少的见闻，有好有坏，我有身边也有人说这个框架很垃圾。但是对于Bootstrap这个框架怎么样，我不做评价，但是Bootstrap用来作为学习的资料时非常合适的，Less的语法糖都了知道，那么如何才能让Less用起来得心应手？无疑，源码是一种途径。

接下来的安排，自己写的文章自己也会去实现它，另外关于Less的学习也不会停止。