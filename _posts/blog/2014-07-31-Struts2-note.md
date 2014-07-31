---
layout:     post
title:      Struts2 笔记
category:   blog
description:   Struts2的一些笔记
tags:
 - 笔记
---

## 1. Struts2工作原理
![Struts2详细流程图](/images/2014-07-31-Struts2-note/Struts2-flow.png)

1. 客户发送一个请求，请求经过其他的Filter组件 --> StrutsFilter控制器。
2. StrutsFilter控制器会调用一个ActionMapper组件，检查是否符合Action请求格式,解析请求信息。
3. StrutsFilter获取ActionMapper解析出的信息，如果非action请求直接调用doFilter方法。如果是action请求，调用ActionProxy组件处理。
4. ActionProxy代理根据ActionMapper解析出来的请求信息，根据struts.xml配置信息寻找请求相关的interceptor,action,result处理组件。(struts.xml配置信息利用ConfigurationManager组件负责解析)
5. ActionProxy会根据struts.xml配置信息创建出一个ActionInvocation组件对象。该组件对象负责存储和调用interceptor,action,result组件。
6. ActionInvocation组件首先调用拦截器，然后执行Action，之后再根据Action返回值调用Result生成响应信息，最后执行拦截器后续处理。
7. 将响应信息给客户浏览器输出

## 2. ValueStack原理
![Value Stack 原理](/images/2014-07-31-Struts2-note/Value-Stack.jpg)

当客户发送请求后，Struts控制器会创建一个ValueStack对象，用于存储当前请求相关的数据对象。例如action,request,session等。内部结构主要由以下3部分构成：

1. OGNL引擎  
`通过OGNL表达式才能访问valuestack存储的信息。"#key"格式访问context; "属性"格式访问root栈顶对象属性`
2. root栈存储区  
`默认存储了当前请求对应的Action对象，绝大部分占据栈顶位置。`
3. context Map存储区  
`默认存储了request,session,application等Map封装类型对象。（值栈作用负责存储请求和处理后的相关数据，将来页面可通过struts2标签显示数据，标签采用ognl表达式定位valuestack中操作的数据.`

<pre class="brush:java">
public class SessionMap 
extends AbstractMap{
  private HttpSession session;
  public SessionMap(HttpSession session){
    this.session = session;
  }

  public void put(Object key,Object val){
    session.setAttribute(
       key.toString(),val);
  }
  public Object get(Object key){
     session.getAttribute(key.toString());
  }
}

Map session = new SessionMap(request.getSession());
</pre>
   
