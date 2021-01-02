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

Spring的Ioc如何理解？

- Ioc原意是控制反转，Spring的Ioc只是众多的Ioc思想的一种实现，通过构造器注入，set注入实现。AOP抽象，强大的第三方整合，易测试性。

依赖查找与依赖注入有什么区别？

- 依赖查找是通过主动的方式（主动调用）查找依赖，通常需要依赖容器或者标准API实现。依赖注入是通过手动或者自动依赖绑定的方式，无需依赖特定的容器和API。

Spring Ioc依赖查找的方式

- 根据bean名称查找。分为实时查找与延时查找。
- 根据bean类型查找。单个bean与beanList
- 根据bean名称+类型查找。
- 根据Java注解查找。

```java
//通过xml配置bean的方式启动spring应用上下文
ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("classpath:/application.properties");

//父类接收
BeanFactory beanFactory = new ClassPathXmlApplicationContext("classpath:/application.properties");

//根据名称获取bean
BaseResult bean1 = (BaseResult) beanFactory.getBean("baseResult");

//根据类型获取单个bean 有多个会报错 需指定primary
BaseResult bean = beanFactory.getBean(BaseResult.class);

//根据类型获取beanList
if(beanFactory instanceof ListableBeanFactory) {
    ListableBeanFactory listableBeanFactory = (ListableBeanFactory) beanFactory;
    Map<String, BaseResult> beansOfType = listableBeanFactory.getBeansOfType(BaseResult.class);
}

//根据注解查找 (可以自定义注解)
if(beanFactory instanceof ListableBeanFactory) {
    ListableBeanFactory listableBeanFactory = (ListableBeanFactory) beanFactory;
    Map<String, BaseResult> beansOfType = (Map) listableBeanFactory.getBeansWithAnnotation(Autowired.class);
}
```