- Spring五大编程模型

1. 面向对象编程
   1. 契约接口：Aware，BeanPostProcessor ...
   2. 设计模式：观察者模式，组合模式，模版模式 ...
   3. 对象继承：Abstract* 类

2. 面向切面编程
   1. 动态代理： JdkDynamicAopProxy
   2. 字节码提升：ASM，CGlib，AspectJ ...

3. 面向元编程
   1. 注解：@Service @Repository @Controller ...
   2. 配置：Environment 抽象、PropertySouces、BeanDefinition ...
   3. 泛型：GenericTypeResolver、ResolvableType ...

4. 函数驱动
   1. 函数接口： ApplicationEventPublisher 
   2. Reactive：Spring WebFlux

5. 模块驱动
   1. Maven Artifacts
   2. OSGI bundes
   3. Java 9 Automatic Module
   4. Spring @Enable*