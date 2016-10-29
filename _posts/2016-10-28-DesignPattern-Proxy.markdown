---
layout: post
title:  "代理模式"
date:   2016-10-28
categories: designpattern
tag: designpattern
---
###  定义 ###
是指为另一个对象提供一个替身或占位符以控制对这个对象的访问。一般来说，代理和被代理对象需要实现同一个接口。


###  分类 ###
####  1.远程代理 ####
当本地代理对象需要调用不在同一个JVM内的对象(即控制对远程对象的访问)<br/>
它的流程如下：<br/>
- 客户对象－－远程方法调用－－>客户辅助对象(Stub)－－(网络)－－>服务辅助对象(Skeleton)－－>服务对象
- 客户对象需要调用某服务，自以为是直接向服务对象调取
- 实际是客户辅助对象假装成服务对象，并向服务器堆发送请求
- 服务辅助对象从客户辅助对象取得请求，对它解包，调用真正服务对象
- 服务对象处理真正的逻辑，把结果返回给服务辅助对象
- 服务辅助对象把调用结果打包返回给客户辅助对象
- 客户辅助对象把返回结果解包，返回给客户对象

在Java中，可以利用RMI(Remote Method Invoke)，为客户辅助对象创建和服务对象相同的服务方法
提供网络或IO代码和查找服务<br/>
实现步骤：<br/>
- Step1: 制作远程接口，客户辅助对象和服务对象必须实现远程接口<br/>
- Step2: 制作远程实现，并注册此服务，RMI实际把stub放到registry中<br/>
	`Naming.rebind("ServiceName", serviceImplObj)`
- Step3: 产生Stub和Skeleton对象<br/>
	`rmic MyRemoteServiceImpl`
- Step4: 执行remiregistry, 确保注册服务运行，否则客户无法查找相关服务<br/>
- Step5: 启动服务<br/>
	`java MyRemoteServiceImpl`
- Step6: 客户到RMI registry查找远程服务Stub对象，调用Stub对象的方法<br/>

JDK5之后，RMI和动态代理搭配使用，动态产生Stub(是个Proxy实例)

####  2.虚拟代理 ####
定义：直到我们真正需要一个对象的时候才创建它，一般用在需要创建开销大的对象

#### 3.保护代理 ####
只提供给客户部分接口，根据客户的角色决定是否允许客户访问特定的方法，适用于需要安全控制的对象<br/>
利用Java动态代理创建保护代理(运行时，通过Proxy.newProxyInstance方法才把代理类创建出来)<br/>
提供InvocationHandler, 由Java产生Proxy上的任何方法调用都会被传入此类，控制对被代理对象方法的访问。<br/>
handler决定如何处理这个请求，可通过method.invoke(realSubject, args)把请求转发给被代理对象，或者进一步处理<br/>

动态代理，利用动态编译＋反射技术，把对实际对象的方法调用转换成对传入的InvocationHandler接口实现类的invoke方法调用
{% highlight ruby %}
public class MyInvocationHandler implements InvocationHandler { 
	SubInterface sub; 
	public MyInvocationHandler(SubInterface sub) { this.sub = sub; }
	public Object invoke(Object proxy, Method method, Object[] args) throws IllegalAccessException {...}
}
SubInterface sub = new SubInterfaceImpl();
SubInterface proxy = (SubInterface)Proxy.newProxyInstance(sub.getClass().getClassLoader(), // 动态加载					
				sub.getClass().getInterfaces(),  // 只能代理实现了接口的类
				new MyInvocationHandler(sub));
{% endhighlight %}														  

还可以利用第三方库CGLIB实现动态代理
{% highlight ruby %}
public class CglibProxy implements MethodInterceptor {
	private Enhancer enhancer = new Enhancer();
	public Object getProxy(Class cla) {
		enhancer.setSuperClass(cla);
		enhancer.setCallback(this);		
		return enhancer.create();
	}
	// 拦截所有目标类方法的调用
	@Override
	public Object intercept(Object obj, Method m, Object[] args, MethodProxy proxy) throws Throwable {
		proxy.invokeSuper(obj, args); // 代理类调用父类的方法
	}
}
CglibProxy proxy = new CglibProxy();
SubInterface proxy = (SubInterface) proxy.getProxy(SubInterface.class);
{% endhighlight %}

JDK动态代理：只能代理实现了接口的类<br/>
CGLIB：针对类来实现代理; 对指定目标类产生一个子类，通过方法拦截技术拦截所有父类方法的调用<br/>

常见应用利动态代理的地方：<br/>
- 添加调用日志
- 事务控制
- spring aop
- Java标准库的RMI

#### 4.其它变体的代理 ####
- 防火墙代理：控制网络资源的访问，保护被代理者免于侵害；常用于公司的防火墙系统。<br/>
- 智能引用代理：当被代理者被引用时，进行额外的动作；例如计算一个对象被引用的次数。<br/>
- 缓存代理：为开销大的运算结果提供暂时存储，也允许多个用户共享结果，减少计算或网络延迟；常用于Web服务器代理，以及内容管理和出版系统。<br/>
- 同步代理：在多线程的情况下为被代理者提供安全的访问；常用于Javaspaces，为分散式环境内的潜在对象集合提供同步访问控制。<br/>
- 复杂隐藏代理：用来隐藏一个类的复杂集合的复杂度，并进行访问控制，类似外观代理。<br/>
- 写入时复制代理；用来控制对象的复制，方法是延迟对象的复制，直到用户真的需要为止；例如JDK5的CopyOnWriteArrayList

### 代理和其它模式比较 ###
代理和装饰者模式的区别<br/>
- 装饰者为对象添加行为，不会实例化对象
- 代理是控制对象的访问，会创建对象

代理和适配器模式的区别<br/>
- 适配器会改变对象适配的接口
- 代理实现相同的接口<br/><br/>
