# Celery笔记：

#### **Celery是个异步分布式任务队列**

##### 三个核心组件：

**生产者(Celery client)**。生产者(Celery client)发送消息。在Flask上工作时，生产者(Celery client)在Flask应用内运行。

**消费者(Celery workers)**。消费者用于处理后台任务。消费者(Celery client)可以是本地的也可以是远程的。我们可以在运行Flask的server上运行一个单一的消费者(Celery workers)，当业务量上涨之后再去添加更多消费者(Celery workers)。

**消息传递者(message broker)。**生产者(Celery client)和消费者(Celery workers)的信息的交互使用的是消息队列(message queue)。Celery支持若干方式的消息队列，其中最常用的是RabbitMQ和Redis.

或者：
**任务（task）-队列（broker）-worker-后台存储（backend）**

**任务**：包括异步任务和定时任务，异步任务通常在业务逻辑中被触发并发往任务队列，而定时任务由 Celery Beat 进程周期性地将任务发往任务队列。
使用-使用@celery.task定义任务。在需要使用异步任务的时候“任务名+.delay(需要的参数)”来使用

**队列**：Broker，即为任务调度队列，接收任务生产者发来的消息（即任务），将任务存入队列。Celery 本身不提供队列服务，官方推荐使用 RabbitMQ 和 Redis 等。
使用-直接在初始化celery对象时指定“broker  = XX”

**worker：**Worker 是执行任务的处理单元，它实时监控消息队列，获取队列中调度的任务，并执行它
使用-进入项目的虚拟环境，运行“celery -A tasks.celery worker --pool=solo --loglevel=info”命令

**结果存储**：Backend 用于存储任务的执行结果，以供查询。同消息中间件一样，存储也可使用 RabbitMQ, redis 和 MongoDB 等。
使用-直接在初始化celery对象时指定“backend  = XX”

**简单使用celery**
首先先导入celery。然后初始化celery，传入app，指定broker和backend，然后官网初始化celery的代码直接复制就好。接下来定义celery任务，使用@celery.task定义任务，在需要使用异步任务的时候“任务名+.delay(需要的参数)”来使用