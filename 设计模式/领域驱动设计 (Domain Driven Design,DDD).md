# 领域驱动设计 (Domain Driven Design,DDD)

**推荐阅读**

- [书籍《实现领域驱动设计》, Vaughn Vernon著](../图书资源/实现领域驱动设计_Vaughn%20Vernon著.pdf)
- [书籍《实现领域驱动设计》总结](https://www.cnblogs.com/davenkin/p/road-to-ddd.html)
- [事件驱动架构(EDA)编码实践, Thoughtworks洞见](https://insights.thoughtworks.cn/backend-development-eda/)
- [在微服务中使用领域事件, Thoughtworks洞见](https://insights.thoughtworks.cn/use-domain-events-in-microservices/)
- [简单可用的CQRS编码实践, Thoughtworks洞见](https://insights.thoughtworks.cn/backend-development-cqrs/)
- [领域驱动设计(DDD)编码实践, Thoughtworks洞见](https://insights.thoughtworks.cn/backend-development-ddd/)

## 重要概念

| 概念 | 简要解释 | 
| --- | --- |
| 领域和子域（Domain/Subdomain）| 在日常开发中，我们通常会将一个大型的软件系统拆分成若干个子系统。这种划分有可能是基于架构方面的考虑，也有可能是基于基础设施的。但是在DDD中，我们对系统的划分是基于领域的，也即是基于业务的。|
| 限界上下文（Bounded Context）| 在一个领域/子域中，我们会创建一个概念上的领域边界，在这个边界中，任何领域对象都只表示特定于该边界内部的确切含义。这样边界便称为限界上下文。限界上下文和领域具有一对一的关系。|
| 实体vs值对象（Entity vs Value Object） | 在一个软件系统中，实体表示那些具有生命周期并且会在其生命周期中发生改变的东西；而值对象则表示起描述性作用的并且可以相互替换的概念。多数领域概念都可以建模成值对象，而非实体|
| 聚合（Aggregate）| 聚合中所包含的对象之间具有密不可分的联系，他们是内聚在一起的。比如一辆汽车（Car）包含了引擎（Engine）、车轮（Wheel）和油箱（Tank）等组件，缺一不可。一个聚合中可以包含多个实体和值对象，因此聚合也被称为根实体。聚合是持久化的基本单位 |
| 领域服务（Domain Service） | 领域服务和上文中提到的应用服务是不同的，领域服务是领域模型的一部分，而应用服务不是。应用服务是领域服务的客户，它将领域模型变成对外界可用的软件系统。 |
| 资源库（Repository） | 资源库用于保存和获取聚合对象。资源库分为两种，一种是基于集合的（如Java中的list数据结构），一种是基于持久化的（）。 |
| 领域事件（Domain Event） | 最终一致性取代了事务一致性，通过领域事件的方式达到各个组件之间的数据一致性。领域事件的命名遵循英语中的“名词+动词过去分词”格式，即表示的是先前发生过的一件事情。比如，购买者提交商品订单之后发布OrderSubmitted事件，用户更改邮箱地址之后发布EmailAddressChanged事件。|
| 领域对象（Domain Object, DO） | 从现实世界中抽象出来的有形或无形的业务实体 |
| 数据传输对象（Data Transfer Object, DTO） | 用于跨进程或远程传输时，不应该包含业务逻辑 |
| 数据访问对象（Data Access Object, DAO） | 1、用来封装对数据库的访问（CRUD）；2、通过接收Business层的数据，将POJO持久化为PO |
| 持久对象（Persistent Object, PO） | 1、Data对象，对应数据库中的entity，可以简单地认为一个PO对应数据库中的一条记录；2、PO中不应该包含任何对数据库的操作 |


## 基于业务的分包

在关于代码开发的时候，跟平时我们常用的MVC（Model-View-Controller）设计思想，有这么一种代码模块的划分思路：

```text
├── user
│   ├── ......
│   ├── ......
├── order
│   ├── OrderApplicationService.java
│   ├── OrderController.java
│   ├── OrderPaymentProxy.java
│   ├── OrderPaymentService.java
│   ├── OrderRepository.java
│   ├── command
│   │   ├── ChangeAddressDetailCommand.java
│   │   ├── CreateOrderCommand.java
│   │   ├── OrderItemCommand.java
│   │   ├── PayOrderCommand.java
│   │   └── UpdateProductCountCommand.java
│   ├── exception
│   │   ├── OrderCannotBeModifiedException.java
│   │   ├── OrderNotFoundException.java
│   │   ├── PaidPriceNotSameWithOrderPriceException.java
│   │   └── ProductNotInOrderException.java
│   ├── model
│   │   ├── Order.java
│   │   ├── OrderFactory.java
│   │   ├── OrderId.java
│   │   ├── OrderIdGenerator.java
│   │   ├── OrderItem.java
│   │   └── OrderStatus.java
│   └── representation
│       ├── OrderItemRepresentation.java
│       ├── OrderRepresentation.java
│       └── OrderRepresentationService.java
```

所谓基于业务分包即通过软件所实现的业务功能进行模块化划分，而不是从技术的角度划分(比如首先划分出service和infrastruture等包)。

在DDD中，聚合根是主要业务逻辑的承载体，也是“内聚性”原则的典型代表，因此通常的做法便是基于聚合根进行顶层包的划分。