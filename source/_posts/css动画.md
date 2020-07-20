## css 动画效果

### 动画效果1



效果如图所示
![](https://image-static.segmentfault.com/343/278/3432781490-5e96bf0e37e05_articlex)


[https://angular-t4cuvf.stackblitz.io/](https://angular-t4cuvf.stackblitz.io/)

这个加载动画在其他地方看到过

### 查看动态效果请查看

实现思路：

1. 拆分结构

	将9个立方体才分为一个进行分析，最终得出单个立方体的结构，由三个面组成，代码如下

        li{
	      height: @boxS / 2;
	      width:@boxS;
	      display: inline-block;
	      background: @theme;
	      position: relative;
	      transform: skew(-@shape, 0deg);
	      margin: @pads;
	      box-shadow: 130px 140px 10px 0px #0000002e;
	      transition: all 1s;
	      &::after{
		    content: "";
		    height: @boxS / 2;
		    width: @boxS;
		    display: inline-block;
		    position: absolute;
		    background: darken(@theme , 10%);
		    transform: skew(0deg, @shape);
		    top: @boxS / 2;
		    left: 100%;
	      }
	    
	      &::before{
	      content: "";
	      height: @boxS;
	      width: @boxS;
	      display: inline-block;
	      position: absolute;
	      background: lighten(@theme , 10%);
	      transform: skew(@shape, 0deg);
	      top: @boxS / 2;
	      left: @boxS / 2;
	      } 
	    }

2.组合结构

其三行三列的结构，在DOM上可以采用3 * 3的结构，使用标签`ul`与`li`，在组合九个方块时会发现就方块时垂直排列，效果如图所示

![](https://image-static.segmentfault.com/175/697/1756972811-5e96fd9530bce_articlex)
	
这就时我把它Dom结构才分为3个ul的原因，直接分为f_1、f_2，f3进行操作，进行微调位置，调整完成后就有了上图图一的效果。

	<div class="box-loading">
	    <ul class="f_1">
	        <li></li>
	        <li></li>
	        <li></li>
	    </ul>
	    <ul class="f_2">
	        <li></li>
	        <li></li>
	        <li></li>
	    </ul>
	    <ul class="f_3">
	        <li></li>
	        <li></li>
	        <li></li>
	    </ul>
	<div>

3.开始写动画

首先，设计图形运动轨迹，得到运动的方式，上下运动，由于运动方式采用的时`transform`，与之前调整单个立方体的样式有冲突，最终得出以下代码。

	@keyframes  box-load-run{
	  0%{
	    transform: translate(0px , 0px) skew(-45deg, 0deg);
	  }
	
	  50% {
	    transform: translate(0px , 12px) skew(-45deg, 0deg);
	  }
	
	  100% {
	    transform: translate(0px , 0px) skew(-45deg, 0deg);
	  }
	}


然后，为了九个立方块运动时达到波浪运动的效果，那么需要计算一下动画的延迟与运动时间。为此，我设计了一个函数，该函数运行于 `li`标签下。
	
	.runs(@time , @dey){
	 	&:first-child{
        animation: box-load-run @time infinite ease-in-out;
        animation-delay: @dey * 1;
      }

	  &:nth-child(2){
        animation: box-load-run @time infinite ease-in-out;
        animation-delay: @dey * 2;
      }

      &:last-child{
        animation: box-load-run @time infinite ease-in-out;
        animation-delay: @dey * 3;
      }
	}

参数说明：`@time`：运行时间 `@dey`：运动延迟。

为了达到比较好的动画效果，我们需要动画有来有回，平缓过度，所以采用的时`ease-in-out`。


最后配合一个启动样式完成动画效果。

	.f_1{
	  li{
	     .runs(1.2s , 0.1s );
	  }
	}
	
	.f_2{
	  li{
	    .runs(1.2s , 0.2s);
	  }
	}
	
	.f_3{
	   li{
	    .runs(1.2s , 0.3s );
	  }
	}

## 总结

这种动画效果早已经被人写出来，但是我依然要自己实现一遍，主要目的不是为了写动画效果，而是为了巩固自己的知识点、查看自己的不足。举个例子，在写这个动画的时候自己还对less的函数不熟悉。

第一点：`.runs`这个函数，里面的内容仍然可以优化。使用`less`的`for`循环。

第二点: 页面结构只需要采用一个`ul`标签也可以完成，配合上css的伪元素选择器、less的循环与条件语句。

第三点：作为前端开发更需要一个自己的积累，积累到一定程度一定会发生质的改变，希望能够形成自己的规范或者技术圈子，并让大家探讨。


动画效果地址：[https://angular-t4cuvf.stackblitz.io/boxLoad](https://angular-t4cuvf.stackblitz.io/boxLoad)

**本示例使用的css预编译语言 less。**