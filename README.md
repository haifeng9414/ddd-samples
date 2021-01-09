运行项目需要执行startContainers.sh文件创建rabbitmq和mysql容器。

每个模块都很简单，看例子的时候从单测开始看，每个模块涉及到的架构：
- iddd_agilepm模块将领域事件发送到了消息队列，同时也是《实现领域驱动设计》第13章：通过消息集成限界上下文的实现
- iddd_collaboration模块使用事件源和CQRS
- iddd_collaboration模块ORM和领域事件

除了每个模块用到的架构，值得学习的地方还有公共父类的实现，这些父类提供了哪些公共功能、类名和包名的定义、类所在包、领域服务能够直接引用哪些类，聚合根的方法可以接受领域服务作为方法参数、单测的编写等

These are the sample Bounded Contexts from the book
"Implementing Domain-Driven Design" by Vaughn Vernon:

http://vaughnvernon.co/?page_id=168

The models and surrounding architectural mechanisms
may be in various states of flux as the are refined
over time. Some tests may be incomplete. The code is
not meant to be a reflection of a production quality
work, but rather as a set of reference projects for
the book.

Points of Interest
==================

The iddd_agilepm project uses a key-value store as
its underlying persistence mechanism, and in particular
is LevelDB. Actually the LevelDB in use is a pure Java
implementation: https://github.com/dain/leveldb

Currently iddd_agilepm doesn't employ a container of
any kind (such as Spring).

The iddd_collaboration project uses Event Sourcing and
CQRS. It purposely avoids the use of an object-relational
mapper, showing that a simple JDBC-based query engine
and DTO matter can be used instead. This technique does
have its limitations, but it is meant to be small and fast
and require no configuration or annotations. It is not
meant to be perfect.

It may be helpful to make one additional mental note on
the iddd_collaboration CQRS implementation. To keep the
example simple it persists the Event Sourced write model
and the CQRS read model in one thread. Since two different
stores are used--LevelDB for the Event Journal and MySQL
for the read model--there may be very slight opportunities
for inconsistency, but not much. The idea was to keep the
two models as close to consistent as possible without
using the same data storage (and transaction) for both.
Two different storage mechanisms were used purposely to
demonstrate that they can be separate.

The iddd_identityaccess project uses object-relational
mapping (Hibernate), but so as not to leave it "boring" it
provides a RESTful client interface and even publishes
Domain-Event notifications via REST (logs) and RabbitMQ.

Finally the iddd_common project provides a number of reusable
components. This is not an attempt to be a framework, but
just leverages reuse to the degree that code copying doesn't
liter each project. This is not a recommendation, but it
did work well and save a considerable amount of work while
producing the samples.

Usage
=====

Requires
--------

- Java 7 (8+ does not work)
- MySQL Client + Server
- RabbitMQ

Setup (with Docker)
-------------------

To make it easy to run the tests and it requirements,
the `startContainers.sh` script is provided. Which
will start a:
- MySQL Server container
- RabbitMQ Server container
- RabbitMQ Management container

If the `mysql` command is available, which is the mysql client,
also the required SQL scripts will be imported into the MySQL
Server.

If you use the `startContainers.sh` script, you don't need
MySQL Server and RabbitMQ installed locally. Instead,
Docker needs to be installed as the script will start
MySQL and RabbitMQ in Docker containers.

Build
------

You can build the project by running:

```
./gradlew build
```

This automatically downloads Gradle and builds the project, including running the tests.

The Gradle build using Maven repositories was provided by
Michael Andrews (Github michaelajr and Twitter @MichaelAJr).
Thanks much!


I hope you benefit from the samples.

Vaughn Vernon
Author: Implementing Domain-Driven Design
Twitter: @VaughnVernon
http://vaughnvernon.co/
