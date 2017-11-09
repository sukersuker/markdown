# 数据库设计 Step By Step

[TOC]

## 一、扬帆起航

***

### 数据库是大楼的根基

大多数程序员都很急切，在了解基本需求之后希望很快的进入到编码阶段（可能只有产出代码才能反映工作量），对于数据库设计思考得比较少。

这给系统留下了许多隐患。许多软件系统的问题，如：输出错误的数据，性能差或后期维护繁杂等，都与前期数据库设计有着密切的关系。到了这个时候再想修改数据库设计或进行优化等同于推翻重来。

需求>>说明书

数据>>E-R图

业务>>原型图

活动>>时序图

### 数据库不良设计造成的场景

1. 数据一致性的丧失

   一个订单管理系统，维护着客户和客户下的订单信息。使用该系统的用户在接到客户修改收货地址的电话后，在系统的客户信息页面把该客户的收货地址进行了修改，但原先该客户的订单还是送错了地址。


2. 数据完整性的丧失

   公司战略转移，准备撤出某地区。系统操作人员顺手把该地区的配置信息在系统中进行删除，系统提示删除成功。随后问题就来了，客服人员发现该地区的历史订单页面一打开就出错。


3. 性能的丧失

   一个库存管理系统，仓库管理员使用该系统记录每一笔进出货情况，并能查看当前各货物的库存情况。在系统运行几个月后，仓库管理员发现打开当前库存页面变得非常慢，而且整个趋势是越来越慢。

### 关系型数据库之外的数据存储

**1、平面文件（Flat File）**

包括以.txt和.ini结尾的文件。

eg: 一个.ini文件的内容：

\------------------------------------------------------------

[WebSites]

MyBlog=[http://www.cnblogs.com/DBFocus](http://www.cnblogs.com/DBFocus)

[Directorys]

Image=E:\DBFocus Project\Img

Text=E:\DBFocus Project\Documents

Data=E:\DBFocus Project\DB

\------------------------------------------------------------

优点：

文件的存储形式非常简单，普通的编辑器都能对其进行打开、修改

缺点：

无法支持复杂的查询

没有任何验证功能

对平面文件中间的内容进行插入、删除操作其实是重新生成了一个新文件

适用场景：

存放小量，修改不频繁的数据，如应用配置信息

**2、Windows注册表**

错误的修改Windows注册表会引起系统的紊乱，故不建议把很多数据存放在注册表中。

Windows注册表为树形结构，存放着一些系统配置信息和应用配置信息。

通过把不同的配置存放在注册表的不同分支上，使得应用程序公共配置信息与用户个人配置信息分离。

eg：某文档版本管理系统，能通过配置与本主机上安装的文件比较器建立关联进行文档比较。这是一个公共配置信息，文件比较器路径可以存放在注册表的HKEY_LOCAL_MACHINE\SOFTWARE分支下。

同时该文档版本管理系统能记录用户最近打开的10个文档路径。这是用户个人配置信息，对于不同的Windows用户最近打开的10个文档可以不同，这些配置信息可存放在注册表的HKEY_CURRENT_USER\Software分支下。

**3、Excel表单（Spreadsheets）**

优点：

Excel 非常普及，用户对于Spreadsheet的表现形式非常熟悉

可以进行简单统计，方便出各种图表

缺点：

不适用于许多Spreadsheet之间关系复杂的情况

无法应对复杂查询

数据验证功能弱

适用场景：

数据量不是非常大的办公自动化环境

**4、XML**

XML是一种半结构化的数据。相比于超文本标记语言（HTML），其标签是可以自行定义的，即可扩展的。

eg：一个XML文件内容

><?xml version=”1.0” encoding=”UTF-8” ?>
>
><ClassSchedule>
>
>​     <Class Name=“Psychology” Room=”Field 3”>
>
>​          <Instructor>Richard Storm</Instructor>
>
>​          <Students>
>
>​               <Student>
>
>​                     <FirstName>Ben</FirstName>
>
>​                     <LastName>Breaker</LastName>
>
>​               </Student>
>
>​               <Student>
>
>​                     <FirstName>Carol</FirstName>
>
>​                     <LastName>Enflame</LastName>
>
>​                     <NickName>Candy</NickName>
>
>​               </Student>
>
>​          </Students>
>
>​     </Class>
>
></ClassSchedule>

XML文件有几个特点。

首先，XML标签要求严格对应，且不能出现交错的现象。

其次，XML文件必须有一个根节点，该节点包含所有其他元素。

第三，同级别的不同节点内不必包含相同的元素，如上例中第二个学生Carol有一个特别的节点NickName。这个特性使得在某些场景中XML比关系数据库更能应对变化。

优点：

自然的层次型结构

文本内容通过标签是自解释的

通过XSD（XML Schema语言）可以验证XML的结构

有许多辅助型技术如：XPath, XQuery, XSL, XSLT等

一些商业数据库（如Oracle，SQL Server）已支持XML数据的存储与操作

缺点：

数据的冗余信息较多

无法支持复杂的查询

验证功能有限

对XML中间的内容进行插入、删除操作其实是重新生成一个新文件

适用场景：

适合存放数据量不大，具有层次型结构的数据，如树形配置信息

**5、NoSQL数据库**

非关系型数据库我接触的不是很多，除了给出一些产品名称之外不做很多展开。园子里已有一些文章，本文最后也给出了链接供大家学习、研究。

1. Key-Value数据库

   Redis, Tokyo Cabinet, Flare

2. 面向文档的数据库

   MongoDB, CouchDB

3. 面向分布式计算的数据库

   Cassandra, Voldemort

这几年NoSQL非常热。我认为NoSQL并不是“银弹”，在某些SNS（Social Networking Site）应用场景中NoSQL显示了其优越性，但在如金融行业等对数据的一致性、完整性、可用性、事务性高要求的场景下，现在的NoSQL就未必适用。我们应充分分析应用的需求，非常谨慎地选择技术和产品。



## 二、数据库生命周期

***

![image](C:\Users\dzhang\Pictures\image.png)

数据库的生命周期主要分为四个阶段：需求分析、逻辑设计、物理设计、实现维护。

这里主要关注数据库生命周期中的前两个阶段（需求分析、逻辑设计）。

数据库的物理设计，包括索引的选择与优化、数据分区等内容。

### 阶段1 需求分析

数据库设计与软件设计一样首先需要进行需求分析。

我们需要与数据的创造者和使用者进行访谈。对访谈获得的信息进行整理、分析，并撰写正式的需求文档。

需求文档中需包含：需要处理的数据；数据的自然关系；数据库实现的硬件环境、软件平台等；

### 阶段2 逻辑设计

使用ER或UML建模技术，创建概念数据模型图，展示所有数据以及数据间关系。最终概念数据模型必须被转化为范式化的表。

数据库逻辑设计主要步骤包括：

a) 概念数据建模

在需求分析完成后，使用ER图或UML图对数据进行建模。使用ER图或UML图描述需求中的语义，即得到了数据概念模型（Conceptual Data Model），例如：三元关系（ternary relationships）、超类（supertypes）、子类（subtypes）等。

b) 多视图集成

当在大型项目设计或多人参与设计的情况下，会产生数据和关系的多个视图。这些视图必须进行化简与集成，消除模型中的冗余与不一致，最终形成一个全局的模型。多视图集成可以使用ER建模语义中的同义词(synonyms)、聚合(aggregation)、泛化(generalization)等方法。多视图集成在整合多个应用的场景中也非常重要。

c) 转化概念数据模型为SQL表

根据映射规则，把ER图中的实体与关系转化为SQL表结构。在这一过程中我们将识别冗余的表，并去除这些表。

d) 范式化

范式化是数据库逻辑设计中的重要一步。范式化的目标是尽可能去除模型中的冗余信息，从而消除关系模型更新、插入、删除异常（anomalies）。

### 阶段3 物理设计

数据库物理设计包括选择索引，数据分区与分组等。

逻辑设计方法学通过减少需要分析的数据依赖，简化了大型关系数据库的设计，这也减轻了数据库物理设计阶段的压力。

1. 概念数据建模和多视图集成准确地反映了现实需求场景

2. 范式化在模型转化为SQL表的过程中保留了数据完整性

   数据库物理设计的目标是尽可能优化性能。

   物理设计阶段，全局表结构可能需要进行重构来满足性能上的需求，这被称为反范式化。

   ​

反范式化的步骤包括：

1. 辨别关键性流程，如频繁运行、大容量、高优先级的处理操作
2. 通过增加冗余来提高关键性流程的性能
3. 评估所造成的代价（对查询、修改、存储的影响）和可能损失的数据一致性

### 阶段4 数据库的实现维护

当设计完成之后，使用数据库管理系统（DBMS）中的数据定义语言（DDL）来创建数据结构。

数据库创建完成后，应用程序或用户可以使用数据操作语言（DML）来使用（查询、修改等）该数据库。

一旦数据库开始运行，就需要对其性能进行监视。当数据库性能无法满足要求或用户提出新的功能需求时，就需要对该数据库进行再设计与修改。这形成了一个循环：监视 –> 再设计 –>  修改 –> 监视…。

### 关系数据库基础

**表、行、列**

关系数据库可以想象成表的集合，每个表包含行与列。（可以想象成一个Excel workbook，包含多个worksheet）。

表在关系代数中被称为关系，这也是关系数据库名称的起源（不要与表之间的外键关系混淆）。

列在关系代数中被称为属性（attribute）。列中允许存放的值的集合称为列的域（域与数据类型密切相关，但并不完全相同）。

行在关系代数中的学名是元组（tuple）。

关系数据库的理论基础来自于“关系代数”。但在关系代数中，一个集合的各个元组没有次序的概念，在关系数据库中为了方便使用，定义了行的次序。

**键、索引**

键是一种约束，目的是保证数据完整性

1. 复合键（Compound key）：由多个数据列组成的键
2. 超键（Superkey）：列的集合，其中任何两行都不会完全相同
3. 候选键（Candidate key）：首先是一个超键，同时这个超键中的任何列的缺失都会破坏行的唯一性
4. 主键（Primary key）：指定的某个候选键

索引是数据的物理组织形式，目的是提高查询的性能

**约束**

基本约束

not null constraint, domain constraint

检查约束（Check Constraints）

eg: Salary > 0

主键约束（Primary Key Constraints）

实体完整性（entity integrity），没有两条记录是完全相同的，组成主键的字段不能为null

唯一性约束（Unique Constraints）

外键约束（Foreign Key Constraints）

也被称为引用完整性约束，

**关系数据库操作**

1.选择（Selection）

2.映射（Projection）

3.联合（Union）

4.交集（Intersection）

5.差集（Difference）

6.笛卡尔积（Cartesian Product）

7.连接（Join）

上述7种是最基本的关系数据库操作，对应于集合论中的关系运算。



## 三、基本ER模型构件

***

基本实体关系模型构件

实体关系（ER）模型的目标是捕获现实世界的数据需求，并以简单、易理解的方式表现出来。ER模型可用于项目组内部交流或用于与用户讨论系统数据需求。

### ER模型中的基本元素

基本的ER模型包含三类元素：实体、关系、属性

![201104241145327378](C:\Users\dzhang\Pictures\201104241145327378.png)

实体（Entities）：实体是首要的数据对象，常用于表示一个人、地方、某样事物或某个事件。一个特定的实体被称为实体实例（entity instance或entity occurrence）。实体用长方形框表示，实体的名称标识在框内。一般名称单词的首字母大写。

关系（Relationships）：关系表示一个或多个实体之间的联系。关系依赖于实体，一般没有物理概念上的存在。关系最常用来表示实体之间，一对一，一对多，多对多的对应。关系的构图是一个菱形，关系的名称一般为动词。关系的端点联系着角色（role）。一般情况下角色名可以省略，因为实体名和关系名已经能清楚的反应角色的概念，但有些情况下我们需标出角色名来避免歧义。

属性（Attributes）：属性为实体提供详细的描述信息。一个特定实体的某个属性被称为属性值。Employee实体的属性可能有：emp-id, emp-name, emp-address, phone-no……。属性一般以椭圆形表示，并与描述的实体连接。属性可被分为两类：标识符（identifiers），

描述符（descriptors）。Identifiers可以唯一标识实体的一个实例（key），可以由多个属性组成。ER图中通过在属性名下加上下划线来标识。多值属性（multivalued attributes）用两条线与实体连接，eg：hobbies属性（一个人可能有多个hobby，如reading，movies…）。

复合属性（Complex attributes）本身还有其它属性。

辨别强实体与弱实体：强实体内部有唯一的标识符。弱实体（weak entities）的标识符来自于一个或多个其它强实体。弱实体用双线长方形框表示，依赖于强实体而存在。

### 理解关系

关系在ER模型中扮演了非常重要的角色。通过ER图可以描述实体间关系的度、连通数、存在性信息。

![201104241145548688](C:\Users\dzhang\Pictures\201104241145548688.png)

**关系的度（Degree of a Relationship）**

表示关系所关联的实体数量。二元关系与三元关系的度分别为2和3，以此可以类推至n元。二元关系是最常见的关系。

**关系的连通数（Connectivity of a Relationship）**

表示关系所关联的实例数量的约束。

连通数的值可以是“一”或“多”。“一”这一端，在ER图中通过在实体与关系间标记“1”表示。“多”一端标记“N”表示。

### 其他概念数据模型标记法

![201104241146144066](C:\Users\dzhang\Pictures\201104241146144066.png)



## 四、高级ER模型构件

***

### 泛化（Generalization）

原始的ER模型已经能描述基本的数据和关系，但泛化（Generalization）概念的引入能方便多个概念数据模型的集成。

泛化关系是指抽取多个实体的共同属性作为超类实体。泛化层次关系中的低层次实体——子类型，对超类实体中的属性进行继承与添加，子类型特殊化了超类型。

泛化可以表达子类型的两种重要约束，**重叠性约束（disjointness）**与**完备性约束（completeness）**。

**重叠性约束**表示各个子类型之间是否是排他的。若为排他的则用字母“d”标识，否则用“o”标识（o -> overlap）。

**完备性约束**表示所有子类型在当前系统中是否能完全覆盖超类型。若能完全覆盖则在超类型与圆圈之间用双线标识（可以把双线理解为等号）。

### 聚合（Aggregation）

聚合是与泛化抽象不同的另一种超类型与子类型间的抽象。

泛化表示“is-a”语义，聚合表示“part-of”语义。聚合中子类型与超类型间没有继承关系。

### 三元关系（Ternary Relationships）

当通过二元关系无法准确描述三个实体间的联系时，我们需要使用三元关系。

三元关系中“连通数”的确定方法：

a) 以三元关系中的一个实体作为中心，假设另两个实体都只有一个实例

b) 若中心实体只有一个实例能与另两个实体的一个实例进行关联，则中心实体的连通数为“一”

c) 若中心实体有多于一个实例能与另两个实体实例进行关联，则中心实体的连通数为“多”

### n元关系（General n-ary Relationships）

三元关系可以扩展到n元关系，描述n个实体之间的关系。
一般而言，n元关系中每一个连通数为“一”的实体的键都会出现在一个函数依赖表达式的右侧。

### 排他性约束（Exclusion Constraint）

一般（默认）情况下，多种关系之间是兼容的“或”关系，即允许任意或所有实体参与这些关系。
在某些情况下，多种关系之间是非兼容性“或”关系，即参与关系的实体只能选择其中一种关系，不能同时选择多种关系。



## 五、理解用户需求

***

设计定制化产品——无论是一个数据库、一幅平面广告或一个玩具，都是一个“翻译”的过程。我们需要把浮现在客户脑海中的模糊想法、愿望挖掘出来，并“翻译”成满足他们需求的现实产品。这个“翻译”过程的第一步就是理解用户的需求。

### 制定一个计划

以下按照最普遍的顺序列出了各个步骤。大家根据不同项目的情况可进行灵活调整，目标只有一个就是更好地理解用户需求。

1. 列出问题清单
2. 拜访客户
3. 搞清谁是谁
4. 挖掘客户大脑
5. 尝试客户的工作
6. 学习现有操作
7. 头脑风暴
8. 展望未来
9. 理解客户的质疑
10. 弄清客户的真正需求
11. 优先级
12. 确认你的理解
13. 撰写需求文档



### 1、列出问题清单

我们需要思考，向客户问些什么问题可以帮助我们了解项目的目标和范畴（scope）。以下几个方面的问题可以作为起始点。

**功能**

以下问题主要涉及系统应完成的功能与目标。

- 系统应该做些什么？
- 为什么你想建这个系统？
- 系统看上去应该是怎样的？
- 需要些什么报表？
- 用户需要自己定义新报表吗？
- 系统的操作者会是谁？



**数据需求**

这些问题是为了弄清项目的数据需求。了解需要些什么数据能帮助我们定义数据库表。

- 系统界面上需要展现哪些数据？

- 这些数据应该由谁来提供？

- 这些数据是如何关联的？

- 这些工作现在是如何处理的？数据来自哪里？

  ​

**数据完整性**

这些问题能帮助我们在构建数据库时定义完整性约束。

- 哪些数据是必须填写的？(eg: 一条客户记录必须有电话信息吗？)
- 数据的有效域是什么？(eg: 电话号码是否有格式规定？地址数据应有多长？)
- 系统是否需要根据邮编来检验城市的有效性？
- 系统中是否必须在定义了客户之后才能下订单？
- 系统要求多高的可用性等级？(系统需要7×24的可用性吗？数据的备份频率要多高？)



**安全性**

这些问题能帮助我们了解客户对权限控制与审计方面的需求。

- 是否每个用户都需要一个不同的密码？
- 是否需要控制不同的用户所能访问的数据？(eg: 销售代表有权限看到客户的信用卡账号，但订单录入专员却不能)
- 存储在数据库中的数据是否需要加密？
- 谁做了什么操作是否需要记录以便于审计？(eg: 记录销售代表提高客户级别的操作，在需要时可以追溯操作的原因)
- 系统中的客户分成几个级别？每个级别的客户有多少？
- 是否已有文档记录了用户的工作与权责？



**环境**

这些问题能帮助我们了解当前项目将代替其他什么系统或流程，以及项目将与其他哪些系统进行交互。

- 当前项目是要代替或升级现有的某系统吗？
- 是否有描述现有系统的文档？
- 现有系统的哪些功能是需要的？哪些是不需要的？
- 现有系统处理些什么数据？这些数据是如何存储的？数据之间是如何关联的？
- 是否有关于现有系统数据的文档？
- 当前项目必须与其他哪些系统交互？
- 项目与其他系统之间如何交互？
- 新项目是否需要向现有系统提供数据？如何提供？
- 新项目是否需要接收现有系统的数据？如何接收？
- 是否有关于其他系统的文档？
- 客户的整个业务流程是怎样的？(了解在整个业务流程中当前项目的作用)



### 2、拜访客户

了解我们要设计和搭建的系统的最好方式是询问客户。拿着我们在上一步中准备的问题清单安排与客户进行会面。

即使我们的项目只是去解决整个业务中的一小部分问题，我们也要试图去了解客户的整体业务流程，这可能会给我们带来意想不到的收获。

### 3、搞清谁是谁

意识到不同的客户可能对项目有不同的愿景。我们需要分辨出谁是领导，谁是积极支持者，谁是旁观者，谁是唱反调者。

以下列出了一些常见的客户角色：

- 项目发起人——一般是管理层的某位领导，他是项目的最高推动者。他会为项目协调资源，解决项目遇到的一些障碍，但他不会参与到项目每天的事务中。
- 项目执行负责人——他对于客户的需求和整个业务最为了解。他是了解用户需求阶段最重要的人，他必须有足够的时间来帮助我们定义项目目标以及回答我们的问题。当别人对某业务环节迟疑不决时，我们需要向他请教。
- 客户代表——客户代表是回答我们问题的人，他们也可能成为系统的最终用户。他们可能是某一部分业务的专家，我们需要与多个客户代表进行访谈来了解业务全貌。
- 利益相关者——这是项目将影响到的人，其中某些人可能同时也是客户代表。这些人可能对项目也有兴趣，但未必对系统都有发言权。我们在进行系统设计时也需要考虑对这些人的影响(特别是附带损害)。
- 唱反调者——这是我们需要关注的一些人。如果唱反调者只是让其他人理性或现实地来看待项目，而并不是彻底反对这个项目的话，他将是我们非常好的资源，他将帮助我们说服其他对项目抱有不切实幻想的客户。而如果唱反调者对整个项目抱有抵触时，我们就必须非常小心，有时需要项目执行负责人出面来协调这些人。

### 4、挖掘客户大脑

一旦搞清楚谁是谁之后，我们就要与项目执行负责人讨论客户需要什么。客户希望的解决方案是怎样的，需要包含什么数据，怎样呈现，以及不同数据之间如何关联。

与尽可能多的利益相关者进行交流，我们需要考虑每个人的意见，但心中要牢记项目执行负责人最为理解客户的需求并具有最终决定权。

### 5、尝试客户的工作

saying>>doing>>thinking

观察客户每日的工作能帮助我们更好的理解业务。如果我们能做一会儿客户的工作来了解其中包括的内容那就最好了。

即使我们不能实际尝试客户的工作，一般我们还是可以坐在他们身边近距离观察。告诉客户我们将稍稍降低他们的工作效率并问一些愚蠢且恼人的问题，之后我们就可以开问了。在这个过程中要进行记录，学习尽可能多的东西。有些时候外行者的一些看法可能转化为客户怎么也不会想到的好主意。

### 6、学习现有操作

在尝试客户的工作之后，我们还可以看一下是否有其他途径能了解现有流程。通常公司有描述客户角色和职责的操作手册或文档。

寻找客户现在使用的数据存储方式，可能是关系型数据库系统或是电子表格或是纸质的单据等等。了解这些数据是怎样使用的，之间是如何关联的。一般物理数据库之间是通过包含冗余信息来相互关联的，如：客户ID。

### 7、头脑风暴

此刻我们已经对客户的业务和需求较为了解了。为了确认没有什么遗漏，我们需要安排头脑风暴。召集项目执行负责人和尽可能多的客户代表与利益相关者，向他们描述前期了解到的需求情况，之后让他们畅所欲言谈谈其中有什么问题或还缺什么。

在这个过程中我们不急于答应或排除任何客户的要求，我们先把客户说到的东西记录下来，并确定这些方面我们已经考虑到了。在正式开发前，我们会与项目执行负责人一起根据项目的规模与交付期限确定需求的优先级。

### 8、展望未来

在头脑风暴过程中思考一下将来的需求。问问客户他们的业务在将来是否会变化或他们希望系统将来能包含什么功能。

我们可以把他们的一些想法放入当前的项目中，即使不能也可以使我们知道将来可能会有些什么扩展，在设计数据库时我们能预先留有余地。

### 9、理解客户的质疑

客户比我们更了解业务，他们的建议或质疑中可能蕴含着我们还未了解到的业务变化点或某些特殊业务情况。

### 10、弄清客户的真正需求

有时客户并不了解自己的真正需求。他们能看到问题的表象，但未必清楚其根源。我们需要帮助客户寻找到问题的根源并针对问题的源头提出解决方案。

### 11、优先级

经过先前的步骤，我们已列出一张长长的期望功能列表。其中的某些功能可能不切实际或超出了当前项目的范畴。为了使项目规模可控，我们要与客户一起定义功能的优先级。

一般我们可以把功能分为三个等级。

- 第一优先级是在本期开发中必须包含的功能，没有完成这些功能意味着项目的失败。
- 第二优先级是可以放到下一期开发的功能，当第一优先级的功能完成后，我们可以把第二优先级的部分功能提到当期开发。
- 第三优先级是那些相对不重要或超出项目范畴的功能，我们可以忽略这些功能。



有些情况下优先级是可能转化的。当第一优先级的某功能非常难实现时，我们可以与客户进行沟通，确认该功能是否如此重要，是否能移到第二优先级中以避免影响项目进度。当第二优先级中的某些功能很容易实现，我们可以把该功能调整到第一优先级列表中。但做这些调整之前必须与客户沟通，得到客户的认可。

### 12、验证你的理解

梳理我们对业务和需求的理解，并一一与客户进行确认。当客户说“但是”、“除了”、“有时”等词时，我们要特别当心，确认客户只是强调了我们已经知道的东西，而没有出现新的情况。在这个阶段客户可能会想到他们之前没有考虑到的例外情况。

### 13、撰写需求文档

需求文档要讲清楚我们将构建怎样的系统，该系统会完成什么工作，包含哪些功能点，并描述客户如何使用该系统来解决他们的问题。需求文档明确了项目将完成的功能，这也避免了系统交付时出现争执的情况。

需求文档中应定义可交付成果，即里程碑。里程碑是可直观展现并能验证的中间成果。客户通过里程碑能衡量项目的进度。在需求文档中还需定义最终交付成果，这也是确定项目是否完成的标准。

用例图是一种非常好的需求分析工具，可以作为需求文档的一部分。用例图的最主要功能就是用来表达系统的功能性需求或行为。用例图从业务角度上体现谁来使用系统、用户希望系统提供什么样的服务，以及用户需要为系统提供的服务，也便于软件开发人员最终实现这些功能。

在官方文档中用例图包含六个元素，分别是：

- 参与者(Actor)
- 用例(Use Case)
- 关联关系(Association)
- 包含关系(Include)
- 扩展关系(Extend)
- 泛化关系(Generalization)



![201105282148277407](C:\Users\dzhang\Pictures\201105282148277407.png)



## 六、提取业务规则

***

### 什么是业务规则

业务规则描述了业务过程中重要的且值得记录的对象、关系和活动。其中包括业务操作中的流程、规范与策略。业务规则保证了业务能满足其目标和义务。

本系列我们关注数据库相关的业务规则，一些例子如下：

- 只有当客户产生第一个订单时才创建该客户的记录。
- 若一名学生没有选任何一门课程，把他的状态字段设为Inactive。
- 若销售员在一个月中卖出10套沙发，奖励500元。
- 一个联系人必须至少有1个电话号码和1个email邮箱。
- 若一个订单的除税总额超过1000元则能有5%的折扣。
- 若一个订单的除税总额超过500元则免运费。
- 员工购买本公司商品能有5%的折扣。
- 若仓库中某货品的存量低于上月卖出的总量时，则需要进货。



### 识别关键业务规则

记录所有的业务规则并对这些规则进行分类能帮助我们更好的在系统中实现业务逻辑。

如何实现业务规则不仅与当前的业务逻辑有关，而且与该业务逻辑将来如何变化有关。当一个规则在将来很可能变化时，我们需要使用更复杂但更灵活的方式构建该规则。

需要特别注意的规则（关键业务规则）：

- 枚举值。例如：有效的发货城市，订单状态（Pending, Approved, Shipped）等。
- 计算参数。例如：对500元以上的订单免运费。这一数值可能在将来会调整为300元或600元。
- 有效参数。例如：项目组可由2至5人组成。某些项目是否可能由1个人完成或有更多人参与。
- 交叉记录和交叉表检查。例如：订单中可订购的货品数量不能超过该货品的当前库存数。
- 可概括性约束。如果可预见到将来需应用一些类似的约束，我们可以考虑把这些约束抽象出来进行管理。例如：某保险公司最近主推保险产品A。对每月能卖出20份A产品的销售人员给予1000元奖金。对于不同的保险产品在不同的时间段可能有不同的推广奖励规则。我们可以把产品名称、编号、销售量、奖金数额、促销时间段提取出来放到一张独立的表中作为计算奖金的参数。
- 非常复杂的检查。有些检查规则非常复杂，把这些规则放到程序代码中实现更为容易和清晰。例如：学生选择理学院的谓词演算课程的前提是已通过理学院的命题演算课程或已通过社科院的逻辑I和II课程或者需要导师的允许。该规则在某些数据库产品中可以通过表级的check约束实现，但放到程序中更易于维护和理解。



一些直接可以在数据库中实现的业务规则：

- 固定枚举值。例如：性别（男、女），用手习惯（左撇子、右撇子）。
- 数据类型要求。每个字段具有确定的数据类型是关系型数据库的重要特性之一。滥用通用的数据类型（如string）对性能和数据防错都会带来损害。
- 必填值。例如：会员必须有手机联系方式。
- 合理性检查。合理性检查设定的范围基本不会变化。例如：商品的价格大于等于0。



### 提取关键业务规则

识别并分类业务规则之后，我们需要在数据库中或数据库外来实现关键业务规则。

我们可以参考如下方法：

- 若规则为检验一组有效值时，把该规则转化为外键约束。先前举例中的有效发货城市就是一个很好的例子。创建ShippingCities表，填入允许的发货城市。然后把Orders表的ShippingCity列设为外键，引用ShippingCities表的主键。
- 若规则为参数可能变化的计算式时，把这些参数提取到一张表中。例如：一个月内卖出总价超过100万元汽车的销售员能获得500元奖金。把参数100万元和500元提取到一张表中，如果需要甚至可以把一个月的时间段也作为参数提取出来。


- 若逻辑或计算规则很复杂时，则提取到代码中进行实现。这里说的代码可以是应用程序端代码，还可以是数据库端存储过程。把规则放到代码中实现的意义在于业务规则与数据库表结构分离了，规则的变化不会影响到数据库表结构。通过结构化编程或面向对象编程来实现复杂的规则更易于维护。



## 七、概念数据建模

***

概念数据建模步骤：

- 辨识实体与属性
- 识别泛化层次结构
- 定义关系

### 1、辨识实体与属性

实体应包含描述性信息

多值属性应作为实体来处理

属性应附着在其直接描述的实体上

### 2、识别泛化层次

实体之间存在层次关系时，则使用超类实体。

超类实体：把标识符和公共的描述符（属性）放在超类实体中，把相同的标识符和特有的描述符放在子类实体中。

### 3、定义关系

处理代表实体之间联系的数据元素即关系。

关系在需求描述中一般是一些动词如：works-in、works-for、purchases、drives，这些动词联系了不同的实体。

**非冗余关系**

描述同一概念的两个或多个关系被认为是冗余的。

当把ER模型转化为关系数据库中的表时，冗余的关系可能造成非范式化的表。

![201106261129032016](C:\Users\dzhang\Pictures\201106261129032016.png)

**三元关系**

只有当使用多个二元关系也无法充分描述多个实体间的语义时，才会定义三元关系。

例：如果一个Technician能同时做多个Project，一个Project可以有多个Technician同时参与，一个Technician在一个Project中使用独立的一本Notebook。

![201106261129077495](C:\Users\dzhang\Pictures\201106261129077495.png)

### 案例：ER建模转化示例

第一个视图是人力资源管理视图。每一个员工属于一个部门。事业部是公司的基本单元，每个事业部包含多个部门。每一个部门和事业部都有一个经理，我们需要跟踪每一个经理。

![201106261129138613](C:\Users\dzhang\Pictures\201106261129138613.png)

第二个视图定义了每个员工的头衔，如工程师、技术员、秘书、经理等。工程师一般属于某个专业协会，并可能被分配一台工作站。秘书和经理会被分配台式电脑。公司会储备一些台式电脑和工作站，以分配给新员工或当员工的电脑送修时进行出借。员工之间可能有夫妻关系，这也需要在系统中进行跟踪，以防止夫妻员工之间有直接领导关系。

![201106261129175978](C:\Users\dzhang\Pictures\201106261129175978.png)

第三个视图，包含员工（工程师、技术员）分配项目的信息。员工可以同时参与多个项目，每一个项目可以在不同的地方（城市）设有总部。但一个员工在指定的地点只能做当地的一个项目。员工在不同的项目中可以选用不同的技能。

![20110626112919353](C:\Users\dzhang\Pictures\20110626112919353.png)

**全局ER图**

对三个视图的简单集成可得到全局ER图，它是构造范式化表的基础。全局ER图中的每一个关系都是基于企业中实际数据的一个可验证断言。对这些断言进行分析导出了从ER图到关系数据库表的转化。

![201106261129229604](C:\Users\dzhang\Pictures\201106261129229604.jpg)

从全局ER图中可以看到二元、三元和二元回归关系；可选和强制存在性关系；泛化的分解约束。

## 八、视图集成

***

俯瞰整个数据库生命周期。已完成了“确定需求”和“数据模型”（图中以灰色标出），下一步是：“视图集成”

![201107091604535112](C:\Users\dzhang\Pictures\201107091604535112.png)

**视图集成步骤**

- 集成策略选择
- 比较实体关系图
- 统一实体关系元素
- 合并、重构实体关系图

### 1、集成策略选择

每次集成2个局部ER图。

每次集成n个局部ER图（n大于2且小于等于总ER图数）。

### 2、比较实体关系图

命名上的冲突包括“同物异名”和“异物同名”。“同物异名”是指同一个概念使用了不同的名称，可以通过检视数据字典（命名及其描述对应表）来发现。

### 3、统一实体关系元素

基本目标是解决各局部ER图中的冲突，使这些元素一致化，

### 4、合并、重构实体关系图

合成和重构局部ER图，最终得到完整、最简约和可理解的全局ER图。

**了解目标**

两张具有重叠数据的局部ER图，是对两组不同用户访谈后画出的。

![201107091605059855](C:\Users\dzhang\Pictures\201107091605059855.png)

以报表为关注点的ER图，其中包含发布报表的部门、报表中的主题和报表提交的对象。

![201107091605086002](C:\Users\dzhang\Pictures\201107091605086002.png)

以发布作为关注中心，把发布内容中的关键词建模为另一个实体。

### 案例：集成步骤示例

首先，在两张局部ER图中寻找是否存在“同物异名”与“异物同名”现象。

实体Topic-area与实体Keyword为“同物异名”，虽然两个实体的属性不完全相同，但两者属性是兼容的，可以进行统一化。

![201107091605106884](C:\Users\dzhang\Pictures\201107091605106884.png)

其次，再来看两张ER图之间的结构性冲突。

实体Department与属性dept-name为类型冲突。解决该冲突的方法是保留强类型（实体Department），把属性dept-name移至实体Department中。解决该冲突，

![201107091605134046](C:\Users\dzhang\Pictures\201107091605134046.png)

比较变化后的各局部ER图，寻找之间的“共同之处”进行合并。 在真正合并之前必须确认这些“共同之处”的语义概念完全等同，这也保证了合并后语义的完整性。

两个共同实体：Department和Topic-area，且语义一致。初步合并后的全局ER图

![201107091605163126](C:\Users\dzhang\Pictures\201107091605163126.png)

实体Publication和Report与实体Department和Topic-area之间的关系存在冗余。通过与用户的再次确认，了解到Publication是Report的泛化（报表只是发布材料中的一种），故不能简单的去除实体Publication及关系have和include来消除冗余，而可以引入泛化关系并去除冗余关系publish和contain。

增加泛化关系后的ER图（Publication为超类型，Report为子类型）。

![20110709160519321](C:\Users\dzhang\Pictures\20110709160519321.png)

实体Report与实体Department和Topic-area之间的冗余关系publish和contain被去除了。Report中的属性title也被去除了，因为该属性已经出现在其超类型实体Publication中了。

![201107091605223677](C:\Users\dzhang\Pictures\201107091605223677.png)

ER图集成是一个持续优化和评估的过程。需要注意的是“最简约”未必会最高效。



## 九、ER TO SQL 转化

***

### 从ER模型到SQL表

从ER图转化得到关系数据库中的SQL表，一般可分为3类。

1. 转化得到的SQL表与原始实体包含相同信息内容。该类转化一般适用于：

   ​

   二元“多对多”关系中，任何一端的实体

   二元“一对多”关系中，“一”一端的实体

   二元“一对一”关系中，某一端的实体

   二元“多对多”回归关系中，任何一端的实体（注：关系两端都指向同一个实体）

   三元或n元关系中，任何一端的实体

   层次泛化关系中，超类实体

   ​

2. 转化得到的SQL表除了包含原始实体的信息内容之外，还包含原始实体父实体的外键。该类转化一般适用于：

   ​

   二元“一对多”关系中，“多”一端的实体

   二元“一对一”关系中，某一端的实体

   二元“一对一”或“一对多”回归关系中，任何一端的实体

   该转化是处理关系的常用方法之一，即在子表中增加指向父表中主键的外键信息。

   ​

3. 由“关系”转化得到的SQL表，该表包含“关系”所涉及的所有实体的外键，以及该“关系”自身的属性信息。该类转化一般适用于：

   ​

   二元“多对多”关系

   二元“多对多”回归关系

   三元或n元关系

该转化是另一种常用的关系处理方法。对于“多对多”关系需要定义为一张包含两个相关实体主键的独立表，该表还能包含关系的属性信息。

### 对于NULL值的处理规则

1. 当实体之间的关系是可选的，SQL表中的外键列允许为NULL。
2. 当实体之间的关系是强制的，SQL表中的外键列不允许为NULL。
3. 由“多对多”关系转化得到的SQL表，其中的任意外键列都不允许为NULL。

### 转化步骤

以下总结了从ER图到SQL表的基本转化步骤

- 把每一个实体转化为一张表，其中包含键和非键属性。
- 把每一个“多对多”二元或二元回归关系转化为一张表，其中包含实体的键和关系的属性。
- 把三元及更高元（n元）关系转化为一张表。

1、实体转化

按照实体间最为自然的父子关系，把父实体的键放入子实体中；另一种策略是基于效率，把外键加入到具有较少行的表中。

转化得到的SQL表可能会包含not null, unique, foreign key等约束。每一张表必须有一个主键（primary key），主键隐含着not null和unique约束。

2、“多对多”二元关系转化

每一个“多对多”二元关系能转化为一张表，包含两个实体的键和关系的属性。

这一转化得到的SQL表可能包含not null约束。在这里没有使用unique约束的原因是关系表的主键是由各实体的外键复合组成的，unique约束已隐含。

3、三元关系转化

每一个三元（或n元）关系转化为一张表，包含相关实体的n个主键以及该关系的属性。

这一转化得到的表必须包含not null约束。关系表的主键由各实体的外键复合组成。n元关系表具有n个外键。除主键约束外，其他候选键(candidate key)也应加上unique约束。

## 十、范式化

***

### 范式基础

关系数据库中的表有时会面对性能、一致性和可维护性方面的问题。

把整个数据库中的数据都定义在一张表中将导致大量冗余的数据，低效的查询和更新性能，对某些数据的删除将造成有用数据的丢失等。

| product_name   | order_no | cust_name | cust_addr | credit | date       | sales_name |
| :------------- | -------- | --------- | --------- | ------ | ---------- | ---------- |
| vacuum cleaner | 1435     | Dave      | Austin    | 6      | 2010-03-01 | Carl       |
| computer       |          |           |           |        |            |            |
| refrigerator   |          |           |           |        |            |            |
| DVD player     |          |           |           |        |            |            |
| radio          |          |           |           |        |            |            |
| CD player      |          |           |           |        |            |            |
| vacuum cleaner |          |           |           |        |            |            |
| refrigerator   |          |           |           |        |            |            |
| television     |          |           |           |        |            |            |

在这张表中，某些产品和客户信息是冗余的，浪费了存储空间。某些查询如“上个月哪些客户订购了吸尘器”需要搜索整张表。当要修改客户Dave的地址需要更新该表的多条记录。最后删除客户Qiang的订单（2730）将造成该客户姓名、地址、信用级别信息的丢失，因为该客户只有唯一这个订单。

如果我们通过一些方法把该大表拆分成多个小表，从而消除上述这些问题使数据库更为高效和可靠，范式化就是为了达到这一目标。范式化是指通过分析表中各属性之间的互相依赖，并把大表映射为多个小表的过程。

### 第一范式（1NF）

定义：当且仅当一张表的所有列只包含原子值时，即表中每行中的每一个列只有一个值，则该表符合第一范式。

域、属性、列之间的差异。域是某属性所有可能值的集合，但同一个域可能被用于多个属性上。

举例来说，人名的域包含所有可能的姓名集合，在表中可用于cust_name或sales_name属性。每一列代表一个属性，有些情况下代表不同属性的多个列具有相同的域，这并不会违反第一范式，因为表中的值仍是原子的。

仅符合第一范式的表常会遇到数据重复、更新性能以及更新一致性等问题。为了更好的理解这些问题，我们必须定义键的概念。

超键是一个或多个属性的集合，其能帮助我们唯一确定一条记录。若组成超键属性列的子集仍为一个超键，但该子集少了任何一个属性都将使其不再是一个超键，则该属性列子集称为候选键。主键是从一张表的候选键集合中任意挑选出的，作为该表的一个索引。



### 第二范式（2NF）

一个或多个属性值能唯一确定一个或多个其他属性值称为函数依赖。

> **report**:  report_no –> editor, dept_no
>
> ​                dept_no –> dept_name, dept_addr
>
> ​                author_id –> author_name, author_addr

定义：一张表满足第二范式（2NF）的条件是当且仅当该表满足第一范式且每个非键属性完全依赖于主键。当一个属性出现在函数依赖式的右端，且函数依赖式的左端为表的主键或可由主键传递派生出的属性组，则该属性完全依赖于主键。

仅满足第一范式的report表的缺陷。

report_no, editor和dept_no对该Report的每一位author都需要重复，故当Report的editor需要变更时，多条记录必须同步修改。这就是所谓的更新异常（update anomaly），冗余的更新会降低性能。当没有把所有符合条件的记录同步更新时，还会造成数据的不一致。若要在表中加入一位新的author，只有在该author参与了某Report的撰写才能插入该author的记录，这就是所谓的插入异常（insert anomaly）。最后，若某一张Report无效了，所有与该Report相关联的记录必须一起删除。这可能造成author信息的丢失（与该Report相关联的author_id, author_name, author_addr也被删除了）。这一副作用被称为删除异常（delete anomaly），使数据丧失了完整性。

满足第二范式表的函数依赖为：

> - report1: report_no –> editor, dept_no
>
> ​                 dept_no –> dept_name, dept_addr
>
> - report2: author_id –> author_name, author_addr
> - report3: report_no, author_id为候选键，无函数依赖

满足第二范式的表可以直接从ER图转化得到。



### 第三范式（3NF）

定义：一张表满足第三范式（3NF）当且仅当其每个非平凡函数依赖X –> A，其中X和A可为简单或复合属性，必须满足以下两个条件之一。1. X为超键 或 2. A为某候选键的成员。若A为某候选键的成员，则A被称为主属性。注：平凡函数依赖的形式为YZ –> Z。

第三范式表及函数依赖：

> - report11: repot_no –> editor, dept_no
> - report12: dept_no –> dept_name, dept_addr
> - report2:   author_id –> author_name, author_addr
> - report3:   report_no, author_id为候选键（无函数依赖）



### Boyce-Codd范式（BCNF）

定义：一张表R满足Boyce-Codd范式（BCNF），若其每一条非平凡函数依赖X –> A中X为超键。

BCNF范式是比第三范式更高级别的范式因为其去除了第三范式中的第二种条件（允许函数依赖右侧为主属性），即表的每一条函数依赖的左侧必须为超键。每一张满足BCNF范式的表同时满足第三范式、第二范式和第一范式。



### 案例：数据库范式化

![201109041741309008](C:\Users\dzhang\Pictures\201109041741309008.png)

函数依赖可通过分析ER图及业务经验推得。

> - emp_id, start_date –> job_title, end_date
> - emp_id –> emp_name, phone_no, office_no, proj_no, proj_name, dept_no
> - phone_no –> office_no
> - proj_no –> proj_name, proj_start_date, proj_end_date
> - dept_no –> dept_name, mgr_id
> - mgr_id –> dept_no

解决方案涵盖了所有函数依赖。满足第三范式和BCNF范式，同时该方案创建了最少数量的表。

> - emp_hist:      emp_id, start_date –> job_title, end_date
> - employee:    emp_id –> emp_name, phone_no, proj_no, dept_no
> - phone:            phone_no –> office_no
> - project:           proj_no –> proj_name, proj_start_date, proj_end_date
> - department: dept_no –> dept_name, mgr_id
>
> ​                           mgr_id –> dept_no



**范式化从ER图得到的候选表**

在数据库生命周期中，对表的范式化是通过分析表的函数依赖完成的。这些函数依赖包括：从需求分析中得到的函数依赖；从ER图中得到的函数依赖；从直觉中得到的函数依赖。

主函数依赖代表了实体键之间的依赖。次函数依赖代表实体内数据元素间的依赖。一般来说，主函数依赖可从ER图中得到，次函数依赖可从需求分析中得到。表1展示了每种基本ER构件所能得到的主函数依赖。

| 关系的度（Degree） | 关系的连通数（Connectivity） | 主函数依赖 |
| ------------ | -------------------- | ----- |
| 二元或二元回归      |                      |       |
| 三元           |                      |       |
| 泛化           |                      |       |

每个候选表一般会有多个主函数依赖和次函数依赖，这决定了当前表的范式化程度。对每个表采用各种技术使其达到需求规格中要求的范式化程度，在范式化过程中要保证数据完整性，即范式化后得到的表应包含原先候选表的所有函数依赖。精心设计的概念数据模型通常能得到基本已范式化的表，后期的范式化处理不会很困难，所以概念数据建模非常重要。



## 十一、通用设计模式

***

这一小节我们将分析一些较为常见的业务场景，并给出对于这些场景的表结构设计方法。这些方法可以放入我们自己的数据库设计工具箱，当在面对现实需求时可灵活加以运用。

### 多值属性

多值属性很常见，如淘宝网中每个用户都可以设置多个送货地址，又如在CRM系统中客户可以有多个电话号，一个号码用于工作时间，另一个用于下班时间等。

以存储客户的联系电话为例。联系电话是客户的属性，所以首先可能想到的一种设计方案如下：

![201109192110465282](C:\Users\dzhang\Pictures\201109192110465282.png)

这一设计满足了当前的需求，但不久我们发现客户的联系电话比我们想象的多，他们还有移动电话，而且有些客户有不止一个办公电话号码，我们需要记录这些电话号码，并标识不同的办公室地点。

对于这种需求，我们可以在Customer表中增加列，但这样做会有两方面的问题：首先，每次增加新列都需要修改数据表结构，需要DBA从后台写脚本完成，且前台显示联系电话的功能模块也需要相应进行修改。其次，每个客户具有的联系电话类型及数量各不相同，大量的联系电话单元格都是空的，浪费了许多存储空间。

我们换一种设计方案。每个客户有多个不同类型的联系电话，可以把联系电话作为弱实体从原先Customer实体中分离出来，

![201109192110472675](C:\Users\dzhang\Pictures\201109192110472675.png)

语义1：一个客户不能有重复的联系类型。即一个客户的每个PhoneType不能重复，但多个不同的PhoneType可能对应相同的PhoneNum（如：PhoneType为“Office”和“Home”对应同一个号码）。符合该语义的主键为：CustID，PhoneType；
语义2：一个客户不能有重复的联系电话。即一个客户的每个PhoneNum不能重复，但多个不同的PhoneNum可能对应相同的PhoneType（如：PhoneType为“Office”有多个不同号码）。符合该语义的主键为：CustID，PhoneNum；
语义3：一个客户的一个联系类型能有多个不同的联系电话，一个联系电话可能对应不同的联系类型。符合该语义的主键为：CustID，PhoneType，PhoneNum；

### 历史追溯

说到历史就会涉及时间。例如：当前物价持续上涨，同一产品的售价每个月都有可能调整，若要追溯产品价格变化的情况，仅仅记录该产品当前的一个售价是不足够的。同样对于银行中的利率变化，购入原材料的单价变化等，都需要进行历史追溯。

要跟踪一个实体随时间的变化可以在该实体中增加属性列，指明实体中每个实例的有效日期。图3展示了可追溯产品价格的订单结构（已经过简化）。

![201109192110492967](C:\Users\dzhang\Pictures\201109192110492967.png)

对于上述表结构，回溯历史某个订单的信息的步骤如下：

1. 根据订单号（OrderID）在Orders表中找到对应的记录，并记录下OrderDate

2. 在OrderItems表中根据OrderID找到对应的所有订单明细记录。对每一条明细，记录下Quantity和ProductID，之后：

   a. 通过ProductID，在Products表中找到对应产品的产品描述（Description）

   b. 在ProductPrices表中找到对应ProductID，且EffectiveStartDate <= OrderDate < EffectiveEndDate的记录。该记录中的Price为指定产品在历史下单时的价格。

这样我们就得到了该订单的历史“快照”信息。

需要注意的几点：

1. 如果我们只需要追溯订单中产品的历史价格，可省去上述步骤中的a。
2. 上述订单表结构在每次查看订单时都需要查询ProductPrices表。我们可以通过在OrderItems表中增加ItemPrice列，来避免对ProductPrices表的频繁查询。当创建订单明细记录时，把从ProductPrices中查询到的价格记录到ItemPrice列中，之后每次查看订单时就不需要再查询ProductPrices表了。
3. ProductPrices表的主键为ProductID，EffectiveStartDate。同时该表还隐含着约束：同一种产品的价格有效时间段不能重叠。
4. ProductPrices表结构中EffectiveEndDate列可省去，把该产品的下一个EffectiveStartDate作为上一个有效时间段的自然结束时间点。但这样做会增加查询的复杂度。

![20110919211051883](C:\Users\dzhang\Pictures\20110919211051883.png)

类似的场景包括：跟踪员工薪资的变化情况，跟踪汇率的变化情况等等。还有一种场景可使用该技术，当我们通过系统前台试图删除某信息时，系统的后台数据库并不真正去做删除操作，而是通过EffectiveEndDate标识记录的无效时间。通过EffectiveStartDate和EffectiveEndDate可回溯任何历史时间点存在的记录“快照”。

### 树型结构

树型结构最典型的例子是员工组织机构图

![201109192110522146](C:\Users\dzhang\Pictures\201109192110522146.png)

实体Employees中的EmpID，FirstName，LastName，HireDate，Salary等属性描述了员工的基本信息，树型层次关系通过ManagerID属性进行描述，该属性存储了该员工的经理ID，即指向其父节点。
在节点实体中存储指向父节点的属性已足够描述树型结构的语义，但为了提高查询的效率，设计中可增加树型结构层次（Lvl）和物化路径（Path）作为辅助信息。

![201109192110535951](C:\Users\dzhang\Pictures\201109192110535951.png)

| EmpID | FirstName | LastName  HireDate … | ManagerID | Lvl  | Path         |
| ----- | --------- | -------------------- | --------- | ---- | ------------ |
| 1     | David     | ……                   | NULL      | 0    | .1.          |
| 2     | Eitan     | ……                   | 1         | 1    | .1.2.        |
| 4     | Seraph    | ……                   | 2         | 2    | .1.2.4.      |
| 5     | Jiru      | ……                   | 2         | 2    | .1.2.5.      |
| 10    | Sean      | ……                   | 5         | 3    | .1.2.5.10.   |
| 8     | Lilach    | ……                   | 5         | 3    | .1.2.5.8.    |
| 6     | Steve     | ……                   | 2         | 2    | .1.2.6.      |
| 3     | Ina       | ……                   | 1         | 1    | .1.3.        |
| 7     | Aaron     | ……                   | 3         | 2    | .1.3.7.      |
| 11    | Gabriel   | ……                   | 7         | 3    | .1.3.7.11.   |
| 9     | Rita      | ……                   | 7         | 3    | .1.3.7.9.    |
| 12    | Emilia    | ……                   | 9         | 4    | .1.3.7.9.12. |
| 13    | Michael   | ……                   | 9         | 4    | .1.3.7.9.13. |



### 有向无环图结构

有向无环图（DAG）的典型应用场景是物料清单（BOM）。BOM记录了产品的组装零件或配置方式，下图为某咖啡店的BOM图，描述了配置每种饮料的原料及剂量。

![201109192111022452](C:\Users\dzhang\Pictures\201109192111022452.png)

我们如何把这一BOM信息存储到数据库中呢？

BOM场景以有向无环图为模型。有向无环图与树型层次结构的差异之处在于，有向无环图中的一个节点能有多个父节点。故ER模型中，有向无环图需建模成两个实体，一个实体用于描述节点，另一个实体用于描述节点之间的边。咖啡店BOM场景的ER模型如图7所示，实体Parts表示咖啡店的原料及饮品，实体Assemble表示原料配置的方向（即“有向边”），其中还包括边的权值，此例中边的权值为qty，表示配料的剂量，unit为配料的剂量单位（如：g，ml等）。

![201109192111058841](C:\Users\dzhang\Pictures\201109192111058841.png)

需要注意以下几点：

1. 上述代码在SQL Server 2008下测试通过。对于其他数据库产品，代码细节可能需稍作调整，但主体设计结构不变。
2. Assemble表的主键为：PartID，AssemblyID。
3. Assemble表的PartID列和AssemblyID列外键引用Parts表。
4. Assemble表的check约束保证其中任何记录的PartID与AssemblyID的值不会相同。



### 无向循环图结构

无向循环图的一个典型例子是城市道路系统。下图展示了美国主要城市之间的道路

![201109192111151688](C:\Users\dzhang\Pictures\201109192111151688.png)

每个节点表示一个城市，城市之间的连线代表城市之间的道路，连线上的数值表示距离。道路系统以无向循环图为模型，无向循环图中的节点能与任意数量的其他节点相连，且相连接的节点之间没有父子或先后关系（即“边”没有方向）。

![201109192111161032](C:\Users\dzhang\Pictures\201109192111161032.png)

需要注意以下几点：

1. 上述代码在SQL Server 2008下测试通过。对于其他数据库产品，代码细节可能需稍作调整，但主体设计结构不变。
2. 为了更易于理解，图9道路系统ER模型中的关系connect，在转化为SQL表时更名为Roads。Roads表描述了一个无向循环赋权图。表中每一行表示一条边（道路）。Distance属性表示权值（城市间的距离）。
3. Roads表的CityID列和DestID列外键引用CityID表。
4. Roads表的主键为CityID，DestID。
5. Roads表中包含check约束（CityID < DestID），以避免存入两个相同的边（eg：“芝加哥到纽约”和“纽约到芝加哥”）。无向循环图中节点之间是平等的，故该约束很重要，避免冗余数据。
6. 若要扩展到“有向循环图”场景（如：道路系统中的单行道），我们只要去除check约束（CityID < DestID），此时不同方向的数据不再是冗余。


