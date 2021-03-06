# 存储层组件

存储层会被除了交互层以外的所有位置用到,因此组件的需求也是最多样的.往往各层之间使用的技术栈会有较大差别,但我们也希望我们使用的技术栈越小越好,这样就不用整天的在调研工具而可以更加专注于业务.存储层的组件要满足如下几点:

+ 一定要有很好的通用性,最好可以在多个位置使用,以降低成员学习成本
+ 一定要有很好的接口稳定性,不能三天两头的改接口,使用的接口协议越稳定越好
+ 应该尽量有广泛的对各种编程语言的支持.不能出现为了一个组件去换编程语言这种事.在本文的语境下存储层的组件应该至少支持go(业务层,推理层)和python(分析层,建模层).如果有java支持那更好(spark)
+ 一定要易于维护.通常越是傻瓜配置的组件越好用.
