## 产品设计规范与关乎“秩序和混乱”的人生算法

以前设计产品把随性当创意，不屑于已经被定义好的东西，不屑于规范性的东西。后来才认识到，越自我的东西，距离产品越远，距离艺术越远，距离人性越远。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2444157014,1602156836&fm=173&s=B491E937131179DA831955C20300A032&w=640&h=360&img.JPEG)

不知不觉做产品一年多了，接触的大多数是后端产品。今天和大家分享一下关于后台产品设计的一些经验和心得，与君共勉。

文章结构：

目录结构规范；原型设计规范；注释规范；关于“秩序（规范）和混乱”的一些思考。

一、目录结构规范

0岁的产品同学很多对Axure的定位就是一个画原型的工具，经常拿起Axure就开始画页面，一打开.rp文件目录第一页就是产品首页。

一个好的原型设计不仅仅是画出满足业务需求的原型页面，而是让浏览者清晰的明白这个项目的背景和业务逻辑以及整个产品结构，而这些基本都是通过目录来引导的，所以拿起Axure 不是一开始就画页面，而是完善目录结构。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=1736274035,3262605147&fm=173&s=C0C2D51A119751CE085DA1DA0000C0B2&w=414&h=605&img.JPEG)

需求规格说明书 & 产品结构导图

通常介绍整个项目背景以及产品形态等，通常以《需求规格说明书》的形式展示：

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3328681538,1736779827&fm=173&s=EEE0E05E51DEC5CC0A5D905A000080F1&w=640&h=728&img.JPEG)

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=3135216507,3212480939&fm=173&s=A6D1E07A414E454D467D305B0000C0F0&w=640&h=729&img.JPEG)

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2085767870,4193585093&fm=173&s=EEE0F05E155F41CC18FDB15B000080F1&w=640&h=721&img.JPEG)

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=210455813,2231645746&fm=173&s=48063D72473A5620504474D60000C0B2&w=640&h=481&img.JPEG)

产品结构导图相当于移动端产品的功能结构图，一般用思维导图和架构图来展示说明，以上两个页面主要告诉阅读者项目背景，产品功能形态。

项目开发流程&基本流程图

项目开发流程图主要介绍整个项目开发过程中各部门角色之间的协作流程，有的公司偏向于敏捷开发，有的公司走传统的需求，评审，设计&开发，测试，验收，上线的路线，相关流程都应该体现在流程图里，方便阅读者知晓。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=570939271,3888940179&fm=173&s=66F1407E0F8B41625AF5505A000050F2&w=640&h=338&img.JPEG)

产品设计过程中每一个涉及到业务逻辑和功能逻辑的流程都需要在基本流程图里提现，当遇到复杂的业务逻辑的时候，流程图往往比原型页面更加直观。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=332051047,3869561236&fm=173&s=6EF1CA5C7B8A65680A4D405A0000C0F2&w=640&h=347&img.JPEG)

原型封面 & 版本控制

原型封面主要介绍作简要的产品描述和归属标识，作为原型设计完整性的一部分，不是必要的。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=873985847,771669432&fm=173&s=85F3427E055A566D1A55B04A0000C0F2&w=640&h=409&img.JPEG)

版本控制主要作为产品更新迭代记录以及设计周期的一个时间标识。

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=2669775021,3917686072&fm=173&s=24C0307A036A45204AE4A14B0000F0F1&w=640&h=334&img.JPEG)

全局框架设计规范 & 全局控件设计规范 & 原型注释规范

不仅是专业的UI设计需要遵守设计规范，产品设计也需要定义一定的设计规范，方便自己设计产品随时复用，也方便其他人接手时能尽可能的在保持设计样式整体一致的前提下，快速上手。

二、产品设计规范

保持整个系统所有页面结构的一致性和所有控件元素的统一性是我们定义设计规范最初的目的。

产品设计规范具体体现在：全局框架设计规范，全局控件设计规范，原型注释规范这三个方面。

全局框架设计规范

后台系统框架主要分为三个部分：① 系统功能区 ② 菜单区 ③ 数据/功能区

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=967948507,668259905&fm=173&s=0BB278854AAE8568148C248F02005081&w=640&h=455&img.JPEG)

当然三个区域可以进行很多灵活的设计，无论选择哪种都需要保持所选框架下所有页面的整体一致性。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=862736701,376351235&fm=173&s=82A6FC041DAA87681E9CB50D0300C0C0&w=640&h=466&img.JPEG)

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=419188497,2200057098&fm=173&s=E1B03CD4E78A274948AF8D9603001088&w=640&h=401&img.JPEG)

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=2226650286,1809340221&fm=173&s=D1B43CD4AF8A374948A7959603008088&w=640&h=403&img.JPEG)

全局控件设计规范

上文提到刚开始设计原型的同学经常会出现不同的页面同样的信息不同大小的字体，同样的功能按钮用不同的颜色，结果导致整个原型看起来像是很多个人设计出来的，没有一致性，原因就在于开始的时候没有定义好一套控件设计规范。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=676018069,3513024257&fm=173&s=C2934F22C858E00B48F83542020010FE&w=640&h=324&img.JPEG)

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2286192644,3198232220&fm=173&s=980A5C32572A7109026D98C20200A0F1&w=640&h=471&img.JPEG)

平时的产品设计过程中可以参考一些优秀的UI框架，例如：Bootstrap，Flat UI ，jQuery UI 等。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3973119974,1692503246&fm=173&s=7881FD1089FB6001485C08D2020050B6&w=640&h=307&img.JPEG)

在设计原型前定义好这些控件以及样式的规范后，所有的页面都引用一套样式，不仅使整个原型保持一致性，而且能大大的提高设计效率。

这里要提到Axure 的两个很实用的功能：一个是母版，一个是控件样式

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=487842768,3102191123&fm=173&s=65586533511F61CE10D8D1CA0000C0B1&w=640&h=629&img.JPEG)

把后面可能要多次复用的控件做成母版以及把需要多次复用的控件样式设置成主题样式会大大的提高设计效率。

全局注释规范

当然这里指的注释规范是基于Axure 的，并非借助于PRD文档。对于PC端的原型设计，我更倾向于用Axure 原生的注释功能来描述。分别为：业务逻辑，需求说明，功能逻辑，视觉（交互）逻辑，技术逻辑，备注等。

![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=2567207451,2855648386&fm=173&s=0CE1587E4303416E1AD5D5CA0000C0F1&w=640&h=673&img.JPEG)

在保证整个产品设计统一性和一致性的前提下，每个人都可以根据自己的喜好定义自己的原型设计规范。

四、关于“秩序（规范）和混乱”的一些思考

秩序和混乱一直是指导我学习和工作的第一性原理，下面我用毕加索画牛的过程来解释这个道理，也解释了为什么我会觉得“规范”是一件很重要的事情，以至于要用一篇文章来强调。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=2910206302,2277008609&fm=173&s=94B1E335463071925CB830DE0100C0A0&w=496&h=379&img.JPEG)

这是毕加索1934年12月5日画的一张牛，看起来很正常，惟妙惟俏。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2192149217,829202105&fm=173&s=94B36B35164235415C19188E0100C0A0&w=548&h=362&img.JPEG)

过了几天，他又画了一张图，看起来也不错，比较正常。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3591751472,509396479&fm=173&s=9039EB3794D27FC24C3C34CE0100D0A1&w=579&h=366&img.JPEG)

这张也不错，有点小顽皮。

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=3815968303,3871828340&fm=173&s=B431EB370CD27FC21C3D98DE0100C0A0&w=565&h=373&img.JPEG)

这张更顽皮了

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=4272409034,3598452961&fm=173&s=90B1E937035047CE0ED5ACCB010080B0&w=640&h=413&img.JPEG)

接下来的几张越来越顽皮了

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=951500129,952950053&fm=173&s=F5B02977590E474D1E7DACDB0100C0B3&w=640&h=594&img.JPEG)

你可能看到过毕加索画的很抽象的几何牛，也可能只看过他笔下惟妙惟肖的仿真牛，但是连贯起来看毕加索画牛的过程，从最开始的仿真牛，不断的被结构化，几何化，最后画出了很抽象的几何牛。

普通人完全可以画出一个一模一样的几何牛，但是为什么毕加索画出的牛就这么牛，这是一个值得思考的问题。毕加索是建立在仿真牛的基础上，通过解构，抽象出来的几何牛，这是建立在某种“秩序”上引入的艺术性“混乱”，而不是直接去通过简笔画画出一个几何牛，这就是大师和普通人的本质差别。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=364340285,702899096&fm=173&s=A5548B62D55C2D7CD455FC1F010010C1&w=640&h=319&img.JPEG)

“我们的混乱，是逃避秩序前无力掌控的毁灭性混乱”这让我想到几个月前的自己，对工作的热爱和对产品的追求以及处女座的性格。总是希望画原型的时候每一个页面让它好看，炫酷，以至于过度发挥。这样做导致的结果是可能一个页面很好看，整体会很混乱，看起来像是很多人画出来的。

后来我明白了建立在秩序上的混乱这样的道理之后，每次画原型都会定义好设计规范和UI框架，第一次感觉得到秩序和规范带来的美，以及遵守规范带来的简洁，流畅和高效，我更相信在框架和规范下再去引入混乱和创新才是有价值的。

以前太自我，不屑于已经被定义好的东西，不屑于规范性的东西。后来认识到，越自我的东西，距离产品越远，距离艺术越远，距离人性越远。如果你想在一个领域成为大师，至少应该先遵守这个领域内的某种规范，驾驭了规范之后，再去创造，那样才是可被掌控的有价值的创造。

作者：Allen，一个热爱科学，迷恋技术的文艺青年，金融产品小助理一枚，爱读书，爱旅行，爱健身。个人公众号：思维改变生活

本文由 @Allen 原创发布于人人都是产品经理。未经许可，禁止转载。

题图来自pixabay，基于CC0协议