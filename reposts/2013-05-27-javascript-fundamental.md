---
layout:     post
title:      JavaScript
category:   JavaScript
description:   JavaScript基础
tags: 
 - JavaScript
---
##什么是JavaScript
JavaScript是一种基于对象和事件驱动的解释性脚本语言，内嵌到HTML页面上，由浏览器解释运行

###JavaScript发展史
- JavaScript的正式名称是“ECMAScript”，此标准由ECMA组织发展和维护
- ECMA-262是正式的JavaScript标准
- 此标准基于JavaScript(Netscape)和JScript(Microsoft)

###JavaScript的特点
- 可以使用任何文本编辑工具编写，只需要浏览器就可以执行程序
- 解释执行：事先不编译，逐行执行
- 基于对象：内置大量现成对象
- 适宜：
  - 客户端数据技术
  - 客户端表单合法性验证
  - 浏览器事件的触发
  - 网页特殊显示效果制作

###如何使用JavaScript
1. 事件定义方式
<pre class="brush:js">
    <input onclick="alert('Hello World');" type="button" value="第一个按钮" />
</pre>
2. 嵌入式
<pre class="brush:js">
    <script type="text/javascript" charset="utf-8">
        function firstMethod(){
            alert("Hello World1");
            alert("Hello World2");
            alert("Hello World3");
        }
    </script>
</pre>
3. 文件调用式
<pre class="brush:js">
    <script type="text/javascript" src="js_helloworld.js" ></script>
</pre>

###JavaScript的代码错误
- 解释性代码，代码错误，则无效果
- IE浏览器可以使用开发工具调试
- Firefox浏览器使用错误控制台查看

##JavaScript基础语法
###编写JavaScript代码
- 由Unicode字符集编写
- 注释（单行：//  多行：/*  */）
- 语句
  - 表达式、关键字、运算符组成
  - 大小写敏感
  - 使用分号或者换行结束

###常量、标识符与关键字
- 常量
  - 直接在程序中出现的数据值
- 标识符
  - 由不以数字开头的字母、数字、下划线、美元符号组成
  - 常用于表示函数、变量等的名称
  - 不能使用保留关键字，如break、if等

###变量
- 变量声明
  - 使用关键字`var`声明变量
- 变量初始化
  - 使用`=`赋值
  - 没有初始化的变量则自动取值为undefined
- 变量命名标识符的规则，大小写敏感

###数据类型
!(数据类型overview)[]

####string数据类型
- 表示文本
  - 由Unicode字符、数字、标点符号组成的序列
- 首尾由单引号或双引号括起
- 特殊字符需要转义符
  - 转义符`、`，如：`\n`,`\\`,`\'`,`\"`
- `\u4e00`到`\u9fa5`为汉字的范围

####number数据类型
- 不区分整型数值和浮点型数据
  - 所有数字都采用64位浮点格式存储，类似于double格式
- 整数
  - 10进制的整数由数字的序列组成
  - 16进制数据前面加上0,吧进制前面加0
- 浮点数
  - 使用小数点记录数据，如3.4
  - 使用指数记录数据，如4.3e23 = 4.3 × 10^23

####boolean数据类型
- 仅有两个值：`true`和`false`
- 实际运算中true=1,false=0

###数据类型的隐式转换
- JavaScript属于松散类型的程序语言
  - 变量在声明时不需要指定数据类型
  - 变量由赋值操作确定数据类型
- 不同类型数据在计算过程中会自动进行转换
!(隐式转换小结)[]

###数据类型转换函数
- toString
  - 转换成字符串
  - 所有数据类型均可转换为string类型
- parseInt
  - 强制转换成整数
  - 如果不能转换，则返回`NaN`(Not a Number)
  - 例如：parseInt("6.12")=6
- parseFloat
  - 强制转换成浮点数
  - 如果不能转换，则返回`NaN`
  - 例如：parseFloat("6.12")=6.12
- typeof
  - 查询数值当前类型，返回string/number/boolean/object
  - 例如：typeof("test"+3) = "string"

###null和undefined
- null
  - null在程序中代表“无值”或者“无对象”
  - 可以通过给一个变量赋值null来清除变量的内容
- undefined
  - 声明了变量但从未赋值或者对象属性不存在

###运算符
####算术运算符
- `+`、`-`、`*`、`/`、`%`(取余)
  - `-`可以表示减号，也可以表示负号，如：x = -y
- 递增(`++`)、递减(`--`)

####关系运算符
- 判断数据之间的大小关系
  - `>`(大于),`<`(小于),`>=`(大于等于),`<=`(小于等于),`==`(等于)，`！=`(不等于)
- 关系表达式的值为boolean类型
- 严格相等：`===`
  - 类型相同
  - 数值相同
- 非严格相等：`!==`

####逻辑运算符
- 逻辑非(`!`)、逻辑与(`&&`)、逻辑或(`||`)

####三目运算符
- boolean表达式 ? 表达式1 : 表达式2



