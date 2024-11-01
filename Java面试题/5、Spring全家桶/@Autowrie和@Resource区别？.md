
# 相同点
基本是等价的，都可以将 Bean 注入到对应的属性中

# 不同点
## byName 和 byType匹配顺序不同
- Autowired 在获取Bean的时候，先是 byType 的方式，再是通过 byName 的方式
- Resource 在获取Bean的时候，先是 byName 的方式，再是通过 byType 的方式

## 作用域不同
- Autowired 可以作用在构造器、字段、setter 方法上
- Resource 可以作用在字段、setter 方法上

## 支持方不同
- Autowired 是 Spring 提供的自动注入的注解，只有 Spring 容器会支持，如果做容器迁移，是需要修改代码的
- Resource 是 jdk 官方提供的自动注入注解（JSR-250）。他等于说是一个标准或者约定，所有的IOC容器都会支持这个注解。容器迁移不需要修改