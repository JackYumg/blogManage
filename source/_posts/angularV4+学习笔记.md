#angular学习笔记之组件篇

![](https://i.imgur.com/NQG0KG1.png)

###1注解

####1.1组件注解
`@Component`注解，对组件进行配置。
#####1.1.1注解@Component的元数据

- `selector`
- `template/templateUrl`
- `inputs/outputs`
- `host`
- `styleUrls`


###selector:选择器

页面渲染时，`Angular`组件匹配的选择器,

使用方式：
		
	<ip-address-form></ip-address-form>

采用html标签的方式。
在《Angular权威教程》中，说明另外一种，`<div ip-address></div>`,这种规则与选择器匹配规则一致，也可以为class选择器，根据实际场景而用。***（在Ideal中引入TSLint后，程序能够正常运行，但是编辑器会警告，并提示消除警告方案）***

例如：

	@Component({
	  selector: '.app-single-component',
	  template: `
	  <div>
	    这个是子组件 ：{{name}}
	    <button (click)="sendMessage()" >点我</button>
	  </div>
	  `
	})



###templdate/templdateUrl：模版/模版路径
组件具体的html模版，`template`为模版，`templateUrl`为模版的路径。
`template`中支持`es6`的反引号，进行多行编写，`templdateUrl`用于配置`html`模版的路径。

*注意：在`Typescript`中的构造函数只允许有一个，这也是它与`es6`的一个区别*

###inputs/output:输入/输出

组件中的输入与输出，可以理解为，数据输入到组件中，数据从组件中输出到父组件中。

输入使用方式:`[变量名]`，在父元素页面中使用，`@Input()`，在子组件`class`中使用,代码例子如下：

####single-component.component.ts

	@Component({
  		selector: 'app-single-component',
  		template: `
  			<div>
    			这个是子组件 ：{{name}}
  			</div>
  			`
			})
	export class SingleComponentComponent implements OnInit {
	
	  @Input () name: string ;
	
	  ngOnInit () {
	  }
	
	}

####app.component.ts

	@Component({
	  selector: 'app-root',
	  template: `
	    <div>
	      <app-single-component [name]="simple"></app-single-component>
	  </div>
	  `
	})
	export class AppComponent {
	  simple: string;
	
	  constructor () {
	    this.simple = '我的世界';
	  }
	}
	

其中input作为@Component的元数据，那么还有另外一种写法，之后的输出也一致

*其中一段代码*
	
	@Component({
	  selector: 'app-single-component',
	  inputs:['name'],
	  template: `
	  <div>
	    这个是子组件 ：{{name}}
	  </div>
	  `
	})

输出使用方式：输出的方式或许用广播/订阅来说更加合适。

####single-component.component.ts改

	@Component({
	  selector: 'app-single-component',
	  template: `
	  <div>
	    这个是子组件 ：{{name}}
	    <button (click)="sendMessage()" >点我</button>
	  </div>
	  `
	})
	export class SingleComponentComponent implements OnInit {
	
	  value: string;
	  @Input () name: string ;
	  @Output() emotter: EventEmitter<string>;
	
	  ngOnInit () {
	  }
	
	  constructor () {
	    this.value = '来源于组件的值';
	    this.emotter = new EventEmitter<string>();
	  }
	
	  sendMessage (): void {
	    this.emotter.emit(this.value);
	  }
	
	}


####app.component.ts改

	@Component({
	  selector: 'app-root',
	  template: `
	    <div>
	      <app-single-component [name]="simple" (emotter)="getComponentData($event)"></app-single-component>
	  </div>
	  `
	})
	export class AppComponent {
	  simple: string;
	
	  constructor () {
	    this.simple = '我的世界';
	  }
	
	  getComponentData (message: string): void {
	     console.log(`获取到子组件中的值：${message}`);
	  }
	}


###host:用于在元素行配置元素属性
值为json对象`key-value`，并且作用只做作用于当前组件内部，常用来添加`class`.
###styleUrls:层叠样式表url，值位数组，可以传多个。

当然必要的，在需要用到的`component`的模块中引入：
	
	@NgModule({
	  declarations: [
	    AppComponent,
	    SingleComponentComponent // 引入的指令，放在声明里面
	  ],
	  imports: [
	    BrowserModule // 引入的模块
	  ],
	  providers: [],
	  bootstrap: [AppComponent] //引导应用的根组件
	})
	export class AppModule { }


关于@component的元数据还未完全，所以后面会继续完善。



[源代码git地址][1]


  [1]: http://cqorccmm.cn:8080/gitblit-1.8.0/summary/examples.git