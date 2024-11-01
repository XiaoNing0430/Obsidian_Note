
https://www.yuque.com/hollis666/xx5hr2/fadkbgd4fyv8816p

1. 从主启动类找到 run() 方法，在执行 run() 方法之前 new 一个 SpringApplicantion 对象
2. 进入 run() 方法，创建应用监听器 SpringApplicationRunListeners 开始监听
3. 加载 SpringBoot 配置环境（ConfigurableEnvironment），然后把配置环境（Environment）加入监听对象中
4. 加载应用上下文（ConfigurableAppplicationContext），当做 run 方法的返回对象
5. 创建 Spring 容器，refreshContext(context)，实现 starter 自动化配置和 Bean 的实例化等工作

![[Pasted image 20240427220228.png]]
