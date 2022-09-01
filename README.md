## <center> 我认识的设计模式
---

## 背景
2021 年下半年的一个夜里，居家办公的我接到同事打来的电话，他希望我帮忙解决一个技术问题。问题本身并不复杂，麻烦是他在翻看底层源码时，被里面的复杂关系搞的晕头转向。虽然作者已经使用`Decorator`结尾来表明相关类使用了装饰器模式，但显然我工作四年多的同事并不知道这一点。我问他之前有学习过设计模式吗？他告诉我之前尝试学习过，但书籍里的理论过于深奥，网上的资料又是抄来抄去，慢慢的就放弃了。这样的情况我已经遇到不止一次了，很多有工作经验的同事在面向对象设计能力上的欠缺让我有些惊讶，他们要么不愿意花时间丰富自己，要么就抱怨设计模式是那么隐晦、那么难以理解，以至于他们总是被拒之门外。

2022 年 3 月份，我一个在培训机构授课的朋友委托我写两份上课时使用的课件，分别关于 MySQL 和设计模式。钱很少，但我还是把活儿揽了下来，我想着课件是针对培训生的，所以可以比较简略。尽管这样我还是想得太乐观了，正如我的同事所说，网上有用的资料太少，光是设计模式的课件，我都花费两个月的时间（休息时间和周末）才整理好课件并交付。庆幸的是，在这之前我已经多次通读过《Design Patterns - Elements of Reusable Object-Oriented Software》一书，这也是我揽这份活儿的底气，也实实在在的帮到了我。

课件交付后，我决定对设计模式部分的内容进行完善，补充更多细节，并将其开源出来。

## 项目介绍

本项目主要探讨《Design Patterns - Elements of Reusable Object-Oriented Software》一书中提及的 23 种设计模式。这其中包括以我个人的理解和经验，对那些深奥的理论进行解析，并以实际的案例降低门槛。对一种模式的讲解可能包括如下几个方面的内容：
- 问题引入 => 引入实际案例，抛出在案例中遇到的问题；
- 分析解决方案 => 反复分析案例中的问题，循序渐进，逐步推导出解决方案；
- 实现解决方案 => 实现解决方案，包括解决方案的类图分析及代码附录等；
- 模式的意图分析 => 正式进入该设计模式的内容，解释模式的意图定义；
- 模式的通用结构 => 引入该模式的通用类图结构，对该模式中的每个参与者角色进行职责划分；
- 模式特点 => 罗列该模式的特点，如果需要，结合例子剖析；
- 适用场景 => 部分文章中对该模式的使用场景进行了阐述，为读者指明在哪些时候可以考虑使用该模式；
- 使用技巧 => 结合我个人的工作经验，部分文章中讨论了模式的使用技巧；
- 扩展内容 => 其他相关的内容；
- 源码中的示例 => 列举源码中的示例，包括我们熟悉的 JDK、Spring 等；

> 所有案例的实现代码都使用的 Java 语言，如果你不熟悉 Java 语言，或许可以从类图结构中获益。

## 开始前必读

**（1）适宜人群**

尽管在每种模式的文章中，我们将会通过一些案例来降低理解的难度，但这并不是说任何人都能无障碍的阅读文档，阅读该系列文章是需要一定的面向对象基础的。所以在此之前，你需要提前巩固一些面向对象基础，包括抽象、接口、封装、继承、多态、组合、线程安全等等。你也可以阅读面向对象七大原则相关内容，那有助于你更轻松的搞懂设计模式的目的。

**（2）文档中约定**

在阅读文档文档之前，你需要了解在类图中我是如何描述类与类之间的关系的。内容正在更新中...

**（3）远离那些阻止你的人**

只要你有足够的面向对象基础，每一种设计模式都并不复杂，设计模式并没有很神秘。我们不得不承认，不管学什么，总有人喜欢以过来人的身份给你一些所谓的忠告。趁着他们还没有给你忠告，我先送你一条：远离那些阻止你学习的人，这很重要！

## 模式目录

### 【B】行为型模式

**【B-1】责任链-Chain Of Responsibility**
> **流行指数**：★★☆☆☆
> <br>**难度等级**：★★★★★
> <br>**助记关键词**：过滤器
> <br>**摘要**：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止...[**查看更多**](/doc/b/Chain-Of-Responsibility.md)
> <br>

**【B-2】命令-Command**
> **流行指数**：★★★☆☆
> <br>**难度等级**：★★★★★
> <br>**助记关键词**：封装请求
> <br>**摘要**：将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤消的操作...[**查看更多**](/doc/b/Command.md)
> <br>

**【B-3】解释器-Interpreter**
> **流行指数**：★☆☆☆☆
> <br>**难度等级**：★★★★★
> <br>**助记关键词**：谷歌翻译
> <br>**摘要**：给定一种语言，定义它的文法的一种表示，以及一个解释器，这个解释器使用这个表示来解释该语言中的句子...[**查看更多**](/doc/b/Interpreter.md)
> <br>

**【B-4】迭代器-Iterator**
> **流行指数**：★★★★★
> <br>**难度等级**：★★☆☆☆
> <br>**助记关键词**：遍历
> <br>**摘要**：提供一种方法顺序访问一个聚合对象中各个元素，而又不需暴露该对象的内部表示...[**查看更多**](/doc/b/Iterator.md)
> <br>

**【B-5】中介者-Mediator**
> **流行指数**：★☆☆☆☆
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：中介、消息队列
> <br>**摘要**：用一个中介对象来封装一系列的对象交互。中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互...[**查看更多**](/doc/b/Mediator.md)
> <br>

**【B-6】备忘录-Memento**
> **流行指数**：★★☆☆☆
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：撤销、回滚
> <br>**摘要**：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可以将该对象恢复到原先保存的状态...[**查看更多**](/doc/b/Memento.md)
> <br>

**【B-7】观察者-Observer**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：你不必找我，我来找你
> <br>**摘要**：定义对象间的一种一对多的依赖关系 ,当一个对象的状态发生改变时,所有依赖于它的对象都得到通知并被自动更新...[**查看更多**](/doc/b/Observer.md)
> <br>

**【B-8】状态-State**
> **流行指数**：★★★☆☆
> <br>**难度等级**：★☆☆☆☆
> <br>**助记关键词**：上坡加速，下坡减速
> <br>**摘要**：允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类...[**查看更多**](/doc/b/State.md)
> <br>

**【B-9】策略-Strategy**
> **流行指数**：★★★★★
> <br>**难度等级**：★☆☆☆☆
> <br>**助记关键词**：公交/地铁/共享单车都能回家
> <br>**摘要**：定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化...[**查看更多**](/doc/b/Strategy.md)
> <br>

**【B-10】模板方法-Template Method**
> **流行指数**：★★★★★
> <br>**难度等级**：★☆☆☆☆
> <br>**助记关键词**：封装公用部分，扩展变化部分
> <br>**摘要**：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤...[**查看更多**](/doc/b/Template-Method.md)
> <br>

**【B-11】访问者-Visitor**
> **流行指数**：★☆☆☆☆
> <br>**难度等级**：★★★★★
> <br>**摘要**：表示一个作用于某对象结构中的各元素的操作，它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作...[**查看更多**](/doc/b/Visitor.md)
> <br>

### 【C】创建型模式

**【C-1】单例-Singleton**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：全局唯一、工具类
> <br>**摘要**：单例模式应该是所有设计模式中结构最简单的一个了，同时它也是面试中被考的最多的设计模式...[**查看更多**](/doc/c/Singleton.md)
> <br>

**【C-2】原型-Prototype**
> **流行指数**：★★★★☆
> <br>**难度等级**：★★☆☆☆
> <br>**助记关键词**：细胞分裂
> <br>**摘要**：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象...[**查看更多**](/doc/c/Prototype.md)
> <br>

**【C-3】建造者-Builder**
> **流行指数**：★★★★★
> <br>**难度等级**：★★☆☆☆
> <br>**助记关键词**：外卖套餐，车间流水线
> <br>**摘要**：建造者模式我们都接触过，在开发中，经常见到 XXXBuilder 这样的类，通常以这种方式命名的类就使用了建造者模式...[**查看更多**][builder_doc]
> <br>

**【C-4】工厂方法-Factory Method**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★★☆
> <br>**助记关键词**：煤厂产煤，鞋厂造鞋
> <br>**摘要**：我们都知道设计模式实际上是一些指导思想，这些指导思想是由前人总结和提炼出来的，主要目的是为了解决在代码设计和维护时暴露出来的问题。这些问题往往围绕着耦合性、扩展性等展开...[**查看更多**](/doc/c/Factory-Method.md)
> <br>

**【C-5】抽象工厂-Abstract Method**
> **流行指数**：★★☆☆☆
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：主题切换
> <br>**摘要**：很多时候，我们不应该被一个看起来很复杂的名词或概念所绊倒，因为往往看起来越复杂的东西其本质越简单。就像是抽象工厂模式...[**查看更多**](/doc/c/Abstract-Method.md)
> <br>

### 【S】结构型模式

**【S-1】适配器-Adaptor**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：转接头、不同国家的标准电压不一样
> <br>**摘要**：将一个类的接口转换成客户希望的另外一个接口，适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作...[**查看更多**](/doc/s/Adapter.md)
> <br>

**【S-2】桥-Bridge**
> **流行指数**：★★☆☆☆
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：拱桥形状、解耦多个维度
> <br>**摘要**：当需要从多个维度对一个对象进行扩展时，我们可以使用桥模式来让各个维度分离，进而实现各自独立的变化...[**查看更多**](/doc/s/Bridge.md)
> <br>

**【S-3】组合-Composite**
> **流行指数**：★★★★☆
> <br>**难度等级**：★★☆☆☆
> <br>**助记关键词**：树形结构
> <br>**摘要**：如果一个应用的核心模型是树形结构，那么我们就能用组合模式来表示它，组合模式就是为树形结构量身定制的...[**查看更多**](/doc/s/Composite.md)
> <br>

**【S-4】装饰器-Decorator**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★★☆
> <br>**助记关键词**：俄罗斯套娃
> <br>**摘要**：在不改变原有对象结构的基础情况下，动态地给该对象增加一些额外功能的职责...[**查看更多**][decorator_doc]
> <br>

**【S-5】门面-Facade**
> **流行指数**：★★★☆☆
> <br>**难度等级**：★☆☆☆☆
> <br>**助记关键词**：景区检票口、银行业务接待员
> <br>**摘要**：为子系统中的一组接口提供一个一致的界面，门面模式定义了一个高层接口，这个接口使得这一子系统更加容易使用...[**查看更多**](/doc/s/Facade.md)
> <br>

**【S-6】享元-Flyweight**
> **流行指数**：★★☆☆☆
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：对象共享
> <br>**摘要**：运用共享技术有效地支持大量细粒度的对象...[**查看更多**](/doc/s/Flyweight.md)
> <br>

**【S-7】代理-Proxy**
> **流行指数**：★★★★★
> <br>**难度等级**：★★★☆☆
> <br>**助记关键词**：全局唯一、工具类
> <br>**摘要**：代理模式为其他对象提供一种代理以控制对这个对象的访问...[**查看更多**](/doc/s/Proxy.md)
> <br>

## 模式对比

**【1】组合模式 VS 装饰器模式**
<br>
> 正在努力更新中...
<br>

**【2】中介模式 VS 门面模式**
<br>
> 正在努力更新中...
<br>

## 进阶建议

**（1）学无止境，温故知新**

如果你已经学习过某一种模式，想知道更多关于该模式的内容，可以参阅设计模式相关的书籍，例如《Head First 设计模式》等（此处并不是推荐这本书，如有购书需求，请自行斟酌）。我相信书籍中的内容会比本项目更细致，更全面。

多看书，看好书。不管处于哪个阶段，温故知新总是没错的。拿《Design Patterns - Elements of Reusable Object-Oriented Software》来说，这本书不像小说，不能在阅读过一遍之后就束之高阁。这本书更适合反复阅读，反复理解，反复消化，因为每一次阅读我们都能有些不一样的感受，也能有些不一样的收获。

**（2）不断实践，不断总结**

对于大多数设计模式来说，唯一的提升途径就是：一边实践，一边总结。尝试在实践中使用设计模式，并不断的进行总结，总结引入设计模式后的优缺点、寻找是否还有更好的方案。（**友情提示，如果你的项目隶属于公司，如果没有十足的把握和必要性，不要提交任何修改到远程仓库**）

**（3）不必为选择错误而感到担心**

设计模式的最终目的是为了规划合理的结构和改善既有的代码，为了这样的目标我们借助于设计模式所提供的一些套路。但在面对实际需求时，最困难的部分是选择合适的模式（甚至是应不应该引入设计模式）。有时候我们发现可以套用一个设计模式（或几个模式的组合），又觉得另一个设计模式（或其他模式的组合）同样能套用在此处。而此时，就是你总结经验的最佳时机。
或许你会因为选择错误而付出代价，但在下次遇到类似问题的时候，你已经有失败过一次的经验了。不必为选择错误而感到担心，没有人是完美的，选错了大不了重来一次。

## 写在最后

**原创声明**

本项目中的文档均属原创，每一个字、每一行代码都是从休息时间挤出来的，文档中有错误或者不详尽的地方请谅解。如果你觉得项目中有不合适的地方，欢迎指正。

**发现错误** 

发现错误可在 issue 中告诉作者，我将尽快处理；如果有问题或者疑惑的地方，欢迎在评论中留言讨论。

**联系作者** 

如果有需要，扫描以下二维码，添加作者 QQ。

<div align="center">
   <img src="https://s2.loli.net/2022/06/13/usw2GdZz7YfCrqW.jpg" width="35%"  />
</div>

---
更新不易，如果觉得该文档帮到了你，请点个 star 吧~


[builder_doc]:https://www.yuque.com/coderran/pd/dkzsxv
[decorator_doc]:https://www.yuque.com/coderran/pd/xgff2o
[strategy_doc]:https://www.yuque.com/coderran/pd/mgbgzd