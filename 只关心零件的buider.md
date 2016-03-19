##只关心零件的Buider

建造者模式的初衷是为了将对象的构建与表示分离，封装一个对象的不懂表现方式。但往往实习使用中，我们的需求并没有那么多的表现方式，且遇到最多的Builder也不是定义中的Builder。

==============


###定义

**传统定义：**将复杂对象的构建与它的表示分离，使得同样的构建有着不同的表现方式。


**实际使用的Builder：**依照赋予的不同参数，去创建不同表现的对象。

这是我们最常见到的Builder形式。

	embedManager = new EmbedManager.Builder(this, layoutResID)
                .addToolbar(R.layout.widget_toolbar, R.id.id_tool_bar)
                .addTopWidget(LayoutInflater.from(this).inflate(R.layout.widget_top_view, null))
                .addBottomWidget(LayoutInflater.from(this).inflate(R.layout.widget_bottom_view, null))
                .addCenterTipView(new NoDataTips(this))
                .addLoadView(LayoutInflater.from(this).inflate(R.layout.widget_loading_view, null))
                .build();
                
                
              

###演化

实际过程中，普通建造者模式的使用场景并不是特别多。在实际需求中，最多的需求反而是参数的不确定。**什么是参数的不确定呢？**


通常我们给一个类传参时，使用这样的方式：

	obj.setParam(int arg1,int arg2,int arg3,int arg3);
	
那么问题来了，如果我只有arg1,和arg2需要传递怎么办？

arg3传null? 增加新的重载方法？ 哦~这样的体验说实话并不好。有更好的替代方式吗？有的


就是上面你看到的形式。通常我们这样定义,依照代码注释，你可以很轻松的学会这样的设计技巧。


	class EmbedManager {
	
		private Object arg1;
		private Object arg2;
		private Object arg3;
		
		public EmbedManager(Object arg1,Object arg2,Object arg3){
			if(arg1 != null){
				。。。
			}
			
			if(arg2 != null){
				。。。
			}
			
			if(arg3 != null){
				。。。
			}
			。。。。。
		}
		
		
		//负责创建EmbedManager的建造者
		public static class Builder{
			
			//arg1是必须参数
			private Object arg1;
			//arg2，arg3是可选参数
			private Object arg2;
			private Object arg3;
			
			//必须参数通过构造函数传递
			public Builder(Object arg1){
				this.arg1 = arg1;
			
			}
			
			public Builder setArg2(Object arg2){
				this.arg2 = arg2;
				//设置成功后返回this本对象
				return this;
			}
			
			public Builder setArg3(Object arg3){
				this.arg3 = arg3;
				//设置成功后返回this本对象
				return this;
			}
			
			//最后负责创建的类
			public EmbedManager build(){
				EmbedManager em = new EmbedManager(arg1,arg2,arg3);
				...
				em.setTitle("xxxx");
				em.set~
				em.set~
				return em;
			
			}
		}
	}


###优点

- 你可以很轻易的发现它的优点，将对象复杂创建过程进行封装，实现了解耦。

- 使用体验的优化，参数控制更加容易。

- 更灵活的配置化实现


###误区

无论是原本的建造者模式，还是演化后的建造者模式，在合适的场景下都非常优秀，很难找到缺点。

这里就说一个演化后的建造者使用误区：

有些人因为喜欢演化后的建造者模式清晰的写法而使用了它。但它却用文档形式硬性的告诉你，我的这些个参数你必须都有，这并不是一个很好的使用。

既然使用了这样形式的配置化写法，就隐式的告诉了用户我的这些参数是可有可无的，这样用文档形式规定是非常容易出错的。














