## 面向对象三大特性

- 封装：将对象的实现细节隐藏起来，然后通过一些共用方法来暴露使用该对象的功能。
- 继承：面向对象实现软件复用的重要手段，当子类继承父类后，子类作为特殊的父类，将直接获得父类的属性和方法。
- 多态：子类对象可以直接赋给父类变量，但是在运行时依然表现出子类的行为特征，这意味着同一个类型的对象在执行同一方法时，可以表现出不同的的行为特征

## Lambda表达式

可以用来创建只有一个抽象方法的接口，即创建匿名内部类。Lambda表达式只能用函数式接口来接收，函数式接口就是只包含一个抽象方法的接口。方法引用和构造器引用

![lambda](./image/lambda.png)

#### String 类型的hashCode是怎么计算的？

hashCode 是一个int类型的数字，计算方式如下：首先将String转成 char[] 然后用每个字符的ascii码对应的数字做运算 s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

#### new BigDecimal（double a）不要用，使用 BigDecimal.valueOf(3.5f) 来自官方建议

### Java集合使用stack与queue实现

- stack与queue均可使用ArrayDeque与LinkedList来实现
- stack操作，入栈push，出栈pop，获取栈顶元素peek
- queue操作，入队offer，出队poll，获取队首元素peek