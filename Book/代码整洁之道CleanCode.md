<!-- 代码整洁之道 -->

## 序

大量的也并不在于生产而在于维护

人孰无过 神亦容之

我们坦诚代码状态，因为它永远不完美

沉迷测试~

# 前言

如何用功？阅读代码--大量代码。而且你要去琢磨一段代码好在什么地方，坏在什么地方。

# 第 1 章 整洁代码

勒布朗法则：稍后等于永不。

程序员遵从不了解混乱风险的经理的意愿，是不专业的做法。

---

什么是整洁代码？

往深处说就是细节上花费的心思

整洁代码就是着力照顾的代码。

简单代码规则：

1. 能通过所有测试
2. 没有重复代码
3. 包括尽量少的实体。比如类，方法，函数等。

要为批判你工作的读者写代码。

## 1.6 童子军军规

让营地比你来时更干净。

设计原则
单一权责原则
开放封闭原则

还记得那个关于小提琴家在去表演的路上迷路的老笑话吗？他在街角拦住一位长者，问他怎么才能去卡耐基音乐厅。长着看了看小提琴家，又看了看他手中的琴，说到：“你还得练，孩子，还得练！”

# 第 2 章 有意义的命名

## 2.3 避免误导

比如 o,l 像 0 和 1 避免使用此类命名

## 2.4 做有意义的区分

假设你有个 Product 类 有个 ProductData 类 或者 ProductData 类，那么虽然它们的名称虽然不同，意思却无区别。
Info 和 Data 就像 a an the 一样,是含糊意义的废话。

getActiveAccout();
getActiveAccouts();
getActiveAccoutInfo();

程序员怎么知道该调用哪个函数呢？

如果缺少明确约定，变量 moneyAccount 和 money 没有区别，customerInfo 和 customer 没有区别，theMessage 和 message 没有区别。要区分名称，就要以读者能鉴别不同之处来进行区分。

## 2.5 使用读得出来的名称

避免使用 genymhhms(生成年，月，日，时，分，秒)

## 2.6 使用可搜索名称

int realDayPerIdeaDay = 4;

代码读的越多，眼中越没有前缀。最终前缀变成了不入法眼的边角料，变作了旧代码的标志物。

## 2.9 类名

类名和对象名应该是名词或者名词短语，Customer，WikiPage，Account 和 AddressParser。避免使用 Manager，Processor，Data 和 Info 这样的类名。类名不应该当动词。

## 2.10 方法名

方法名应当是动词或者动词短语 Postpayment,,deletePage 或者 save。

add 添加,insert 插入,append 追加

## 2.16 添加有意义语境

firstName,lastName,stree,houseNumber,city,state,zipcode

很明确构成一个地址。

不过，假使只是在某个方法中看见孤零零一个变量 state 呢？你会理所当然推断那是某个地址的一部分吗？

可以添加前缀 addrFirstName，addrlastName，addrState，更好的方案是创建一个 Address 的类。这样，即便是编译器也会知道这些变量隶属某个更大的概念了。

# 第 3 章 函数

## 3.1 短小

&emsp;&emsp;函数第一规则要短小，第二规则是还要更短小。

## 3.2 只做一件事

## 3.6 函数参数

最理想的参数数量是零，其次是一，再次是二，应尽量避免三。有足够的理由才能用三个以上参数——所以无论如何也不要这么做

## 3.7 无副作用

输出参数

例如

appendFooter(s); // 是调用还是返回值？？？

换而言之

最好的调用是

report.appendFooter(s)

## 3.8 分割指令与询问

好的代码

```js
if (attributeExist("username")){
  setAttribute("uername"){

  }
}
```

## 3.9 使用异常代替返回码

<!-- bad -->

```js
if (deletePage(page) == EOK) {
  if (registry.deleteRefrence(page.name) == EOK) {
    if (configKeys.deleteKey(page.name.makeKey()) == EOK) {
      log("page delete");
    } else {
      log("configKey not deleted");
    }
  } else {
    log("deleteRefrence from registry failed");
  }
} else {
  log("delete failed");
  return ERROR;
}
```

<!-- good -->

```js
try {
  deletePage(page)
  registry.deleteRefrence(page.name)
  configKeys.deleteKey(page.name.makeKey())
} catch(Exception e) {
  log(e.getMessage)
}
```

### 3.9.1 抽离 Try/Catch 代码块

Try/catch 代码块丑陋不堪。他们搞乱了代码结构，把错误处理和正常流程混为一谈。最好把 try 和 catch 代码块的主题抽离出来，另形成函数。

```js
try {
  deletePageAndReference(page)
}catch(Exception e){
  logError(e)
}


function deletePageAndReference(){}
function logError(){}

```

### 3.10 别重复自己

### 3.11 结构化编程

函数中的每个代码块都应该有一个入口，一个出口。遵循这些规则，意味着每个函数只应该有一个 return 语句。

函数保持短小，让它更具有表达力。

### 3.12 如何写出这样的函数

&emsp;&emsp;写代码和写别的东西很像。在写论文或者文章时，你先想什么就写什么，然后再打磨它。初稿也许就粗陋无序，你就斟酌推敲，直到达到你心目中的样子。
&emsp;&emsp;我写函数时，一开始都赘长而复杂。有太多缩进和嵌套循环。有过长的参数列表。名称是随意取得，也会有重复的代码。不过，我会配上一套单元测试，覆盖每一行丑陋代码。
&emsp;&emsp;然后我打磨这些代码，分解函数，修改名称，消除重复。我缩短和重新安置方法。有时，我还拆散类。同时保持测试通过。
&emsp;&emsp;最后遵循本章列出的规则，我组装好这些函数。
&emsp;&emsp;我并不是一开始就按照规则写函数。我想没人能做得到。

### 3.13 小结

函数的目标在于讲述系统故事，你编写的函数必须干净利落的瓶装到一起，形成一种精确而清晰的语言，帮助你讲故事。

# 第四章 注释

别给糟糕的代码写注释 ————重写吧

--Brain W.Kernighan 与 P.J.Plaugher

&emsp;&emsp;什么也比不上防止良好的注释来的有用。什么也不会比乱七八糟的注释更有本事搞乱一个模块。什么也不会比陈旧，提供错误信息的注释更有破坏性。

&emsp;&emsp;注释并不像辛德勒名单。它们并不“纯然得好”。实际上，注释最多也就是一种必须的恶。若编程语言足够有表达力，或者我们常用于这些语言来表达意图，那就不那么需要注释————也许根本不需要。

&emsp;&emsp;注释的恰当用法是在弥补我们用代码表达意图时遭遇的失败。注意，我用了“失败”一词，我是说真的。注释总是一种失败，我们总无法找到不用注释就能表达自我的方法，所以总要有注释，这并不直达庆贺。

&emsp;&emsp;如果你发现自己需要写注释，再想想看是否有方法翻盘，用代码来表达。每次用代码，你都应该夸一下自己。每次写注释，你都应该做个鬼脸，感受自己在表达能力上的失败。

&emsp;&emsp;我为什么极力贬低注释？因为注释会撒谎。也不是说总是如此或有意意如此，但出现的是在太频繁。注释存在的时间越久，就距离其描述的代码越远，越来越变得全然错误。原因很简单。程序员不能坚持维护注释。

&emsp;&emsp;更主张把力气用在写清楚代码上，直接保证无需编写注释。

&emsp;&emsp;不准确的注释要比没注释坏多。它们满口胡言。它们预期的东西永远不能实现。它们设定了无需也不应该再遵循的旧规则。

&emsp;&emsp;真是只在一处地方有：代码。只有代码能忠实地告诉你它做的事。那是唯一真正准确的信息来源。所以，尽管有时也需要注释，我们也应该多花心思尽量减少注释量。

## 4.1 注释不能美化糟糕的代码

&emsp;&emsp;与其花时间编写你搞出来的糟糕的代码的注释，不如花点时间清洁那堆糟糕的代码。

## 4.2 用代码来阐述

&emsp;&emsp;有时代码不足以阐释其行为。

```js
// Check tosee if the employee is eligible for full benefits
if (employee.flags & HOURLY_FLAG) && (employee.age>65)
```

还是这个？

```js
if (employee.isEligibleForFullBenefits)
```

&emsp;&emsp;只要想上那么几秒钟，就能用代码解释你大部分的意图。很多时候，简单到只需要创建一个描述与注释所言同一事物的函数即可。

## 4.3 好注释

&emsp;&emsp;好些注释是必须的，也是有利的。来看看一些我认为值得写的注释。不过要记住，唯一真正好的注释是你想办法不去写注释。

### 4.3.1 法律信息

### 4.4.7 可怕的废话

```java
/** The name */
private String name;

/** The version */
private String version;

/** The licenseName */
private String licenseName;
```

&emsp;&emsp;以上都是没有用的注释

### 4.4.12 注释掉的代码

直接把代码注释掉是最讨厌的做法。别这么干

```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(new ByteArrayInputStream(body));
// InputStream resultStream = formatter.getResultStream(response);
// response.setContentType(formatter.getContentType());
// response.setContentLength(body.length);
```

## 第五章 格式

## 第六章 对象和数据结构

面向对象方式编程

## 第七章 错误处理

### 7.8 别传递 null 值

&emsp;&emsp;在方法中返回 null 值是糟糕的做法，但将 null 值传递给其他方法就更糟糕了。除非 API 要求你向它传递 null 值，否则就尽可能避免传递 null 值。

## 第九章 单元测试

## 第十章 类

### 10.2 类应该短小

遵循单一职责
