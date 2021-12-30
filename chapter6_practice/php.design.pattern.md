PHP Design Pattern



设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。
使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。


SOLID (object-oriented design)
SOLID (single responsibility, open-closed, Liskov substitution, interface segregation and dependency inversion)

面向对象的五大基本原则: 

(1) 单一职责原则（SRP, Single Responsiblity Principle）
(2) 开放封闭原则（OCP, Open Closed Principle） 
(3) 里氏替换原则（LSP, Liskov Substitution Principle） 
(4) 接口隔离原则（ISP, Interface Segregation Principle）
(5) 依赖倒置原则（DIP, Dependency Inversion Principle） 


面向对象有几个原则：

单一职责原则 （Single Responsiblity Principle SRP）
开闭原则（Open Closed Principle，OCP）
里氏代换原则（Liskov Substitution Principle，LSP）
依赖倒转原则（Dependency Inversion Principle，DIP）
接口隔离原则（Interface Segregation Principle，ISP）
合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）
最小知识原则（Principle of Least Knowledge，PLK，也叫迪米特法则）

开闭原则具有理想主义的色彩，它是面向对象设计的终极目标。
其他几条，则可以看做是开闭原则的实现方法。
设计模式就是实现了这些原则，从而达到了代码复用、增加可维护性的目的。


单一职责原则 （Single Responsiblity Principle SRP）

单一职责原则（SRP：Single responsibility principle）又称单一功能原则，面向对象五个基本原则（SOLID）之一。
它规定一个类应该只有一个发生变化的原因。该原则由罗伯特·C·马丁（Robert C. Martin）于《敏捷软件开发：原则、模式和实践》一书中给出的。马丁表示此原则是基于汤姆·狄马克(Tom DeMarco)和Meilir Page-Jones的著作中的内聚性原则发展出的。


开闭原则（Open Closed Principle，OCP）

此原则是由Bertrand Meyer提出的。
原文是：“Software entities should be open for extension,but closed for modification”。
就是说模块应对扩展开放，而对修改关闭。
模块应尽量在不修改原（是“原”，指原来的代码）代码的情况下进行扩展。

里氏代换原则（Liskov Substitution Principle，LSP）

里氏代换原则是由Barbara Liskov提出的。如果调用的是父类的话，那么换成子类也完全可以运行。



依赖倒转原则（Dependency Inversion Principle，DIP）

指在软件里面，把父类都替换成它的子类，程序的行为没有变化。
简单的说，子类型能够替换掉它们的父类型。
依赖性倒转其实可以说是面向对象设计的标志，用哪种语言编程并不是很重要。



接口隔离原则（Interface Segregation Principle，ISP）

定制服务的例子，每一个接口应该是一种角色，不多不少，不干不该干的事，该干的事都要干。


合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）

合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）经常又叫做合成复用原则。
合成/聚合复用原则就是在一个新的对象里面使用一些已有的对象，使之成为新对象的一部分；新的对象通过向这些对象的委派达到复用已有功能的目的。
它的设计原则是：要尽量使用合成/聚合，尽量不要使用继承。


最小知识原则（Principle of Least Knowledge，PLK，也叫迪米特法则）
最少知识原则也叫迪米特法则。
不要和陌生人说话，即一个对象应对其他对象有尽可能少的了解。

迪米特法则（Law of Demeter）又叫作最少知识原则（Least Knowledge Principle 简写LKP），就是说一个对象应当对其他对象有尽可能少的了解,不和陌生人说话.

1987年秋天由美国Northeastern University的Ian Holland提出，被UML的创始者之一Booch等普及。
后来，因为在经典著作《 The Pragmatic Programmer》而广为人知。

迪米特法则的初衷在于降低类之间的耦合。
由于每个类尽量减少对其他类的依赖，因此，很容易使得系统的功能模块功能独立，相互之间不存在（或很少有）依赖关系。
