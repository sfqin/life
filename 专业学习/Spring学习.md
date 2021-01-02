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

BeanFactory与ApplicationContext 谁才是Ioc容器?

- BeanFactory 是一个底层的Ioc容器，ApplicationContext 是在其基础上增加更多的企业级特性，比如AOP面向切面，国际化，注解，事件发布等。ApplicationContext 实现将BeanFactory 已属性的方式组合进来，和直接的BeanFactory 并不是同一个对象。

```java
// demo ApplicationContext 作为更强大的超集，扩展的java注解方式定义bean与获取bean
public class SpringTestService {

    public static void main(String[] args) {

        //创建 beanFactory 容器
        AnnotationConfigApplicationContext beanFactory = new AnnotationConfigApplicationContext();
        //设置当前类作为配置类 Configuration Class
        beanFactory.register(SpringTestService.class);
        //启动应用上下文
        beanFactory.refresh();
        //依赖查找对象
        if(beanFactory instanceof ListableBeanFactory) {
            ListableBeanFactory listableBeanFactory = (ListableBeanFactory) beanFactory;
            Map<String, BaseResult> beansOfType = (Map) listableBeanFactory.getBeansOfType(BaseResult.class);
            System.out.println(beansOfType);
        }
    }

    //java注解的方式定义一个Bean
    @Bean
    public BaseResult getBase() {
        BaseResult baseResult = new BaseResult();
        baseResult.setCode(1);
        baseResult.setMessage("success");
        return baseResult;
    }
postProcessBeanFactory invokeBeanFactoryPostProcessors initMessageSource
}
```

Spring Ioc 容器的生命周期

- 启动 AbstractApplicationContext refresh 方法
  1.  prepareRefresh 前继准备工作。记录启动时间，初始化property source，注册事件
  2. obtainFreshBeanFactory 获得beanFactory 实例化
  3. prepareBeanFactory 配置beanFactory的标准上下文（初始化），比如classLoader 排除一些内置Bean的依赖
  4. postProcessBeanFactory beanFactory后置化自定义操作
  5. invokeBeanFactoryPostProcessors bean进行调整，将bean
  6. initMessageSource 初始化MessageSource 上下文
  7. 初始化时间，监听器等
- 运行
- 停止