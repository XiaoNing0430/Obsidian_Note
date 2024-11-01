
随着应用程序启动，会扫描主启动类上的 `@SpringBootApplication` 注解，其中有一个 `@EnableAutoConfiguration` 注解，在 `@EnableAutoConfiguration` 中有一个 `@Impor` 注解。

会执行 `@Impor` 注解中的类里面的 `selectImports` 方法，`selectImports` 返回的是配置类的全路径名

基于 SPI 机制，去classpath 下的 META-INF 目录下找所有的 `spring.factories` 文件，然后将所有的 `spring.factories` 文件进行解析

自动将需要的Bean对象注入到 IOC 容器中，自动体现在没有对任何类添加 `@Configuration` 和 `@Bean` 注解