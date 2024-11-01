
https://www.yuque.com/hollis666/xx5hr2/abxh7z

Rocket MQ 的事务性消息是通过 TransactionLinstener 接口实现的

1. 在发送事务消息时，首先向 Rocket MQ Broker 发送一条 “half消息” （半消息），半消息被存储在 Broker 端的事务消息日志中，但这个消息还不能被消费者消费
2. 半消息发送成功后，应用程序通过执行本地事务来确定是否要提交该事务消息
3. 如果本地事务执行成功，就会通知 Rocket MQ Broker 提交该事务消息，使得该消息可以被消费者消费；否则，就会通知 Rocket MQ Broker 回滚该事务消息，该消息被删除，从而保证消息不会被消费者消费
![[Pasted image 20240422132541.png]]
即有4个步骤：
1. 发送半消息。应用程序向 Rocket MQ Broker发送一条半消息，该消息在 Broker 端的事务消息日志中被标记为 “`prepared`” 状态
2. 执行本地事务。Rocket MQ 会通知应用程序执行本地事务。如果本地事务执行成功，应用程序通知 Rocket MQ Broker 提交该事务消息
3. 提交事务消息。Rocket MQ 收到提交消息以后，会将该消息的状态从 “prepared” 改为 “`commited`” ，并使该消息可以被消费者消费
4. 回滚事务消息。如果本地事务执行失败，应用程序通知 Rocket MQ Broker 回滚该事务消息，Rocket MQ 将该消息的状态从 “prepared” 改为 “`rollback`”，并将该事务消息从事务消息日志中删除，从而保证该消息不会被消费者消费。


# 如果一直没有收到 commit 或者 rollback 怎么办？

在 Rocket MQ 的事务消息中，如果半消息发送成功之后，Rocket MQ Broker 在规定时间内没有收到 commit 或者 rollback 消息。
Rocket MQ 会向应用程序发送一条检查请求，应用程序可以通过回调方法返回是否要提交或者回滚该事务消息。如果应用程序在规定时间内未能返回响应，Rocket MQ会将该消息标记为 “UNKNOW”状态。
在标记为 “UNKNOW” 状态的事务消息中，如果应用程序有了明确的结果，还可以向 MQ 发送 commit 或者 rollback
但是 MQ 不会一直等下去，如果过期时间已到，Rocket MQ 会自动回滚该事务消息，将其从事务消息日志中删除