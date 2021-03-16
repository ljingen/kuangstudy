# 官网手册Spring Core Technologies

## 1. Ioc容器

本章介绍Spring的反转控制(IoC)容器

### 1.1 Spring的IoC容器和Beans概述







### 1.9 基于注解的容器配置







### 1.10 通过类路径扫描和管理控件



#### 1.10.2 使用元注解和组件注解

spring 提供的一些注解可以作为你自己注解的元注解。元注解是可以用来注解另一个注解的注解。例如，@Service 注解在更早期是使用@Component进行注解的注解。就像下面这样

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component 
public @interface Service {

    // ...
}
```

The `Component`注解让 `@Service`也会被当成一个 `@Component`进行处理.





#### 1.10.6  命名自动检测组件

若使用过滤器自定义扫描 `context:component-scan base-package="xxx"` 

当添加自动扫描组件的策略后，bean的名字就会被BeanNameGenerator自动接管，根据BeanNameGenerator的策略来进行生成。默认情况下，任何包含 name值的Spring“典型”注解 (@Component、@Repository、 @Service和@Controller) 会把那个名字 提供给相关的bean定义。如果这个注解不包含name值或是其他检测到的组件 (比如被自定义过滤器发现的)，默认bean名称生成器会返回小写开头的非限定(non-qualified)类名。

也就是说被加了注解扫描到spring容器中的bean会默认将小写开头的类名 作为bean的name,供其他地方引用.

例如:以下两个组件在spring中引用的名称分别是'customUserDetailsService '和 'detailService'

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {}
 
 
@Service("detailService")
public class CustomUserDetailsService implements UserDetailsService {}

//这个生成的bean名字：myMovieLister 
@Service("myMovieLister")
public class SimpleMovieLister {}
//这个生成的bean名字：movieFinderImpl
@Repository
public class MovieFinderImpl implements MovieFinder {}
```

如果不希望去使用默认的bean命名策略，可以提供一个自定义的bean命名策略。 编写一个自定义的bean命名策略，首先实现BeanNameGenerator接口，记住确保包含无参构造器，接着在自动扫描配置中提供完全限制类名，如下例所示：

> 如果由于具有相同非限定类名的多个自动检测组件产生命名冲突，例如在不同的包中出现了相同的类名，你可能需要配置一个BeanNameGenerator,使用类的完全限定类名作为bean名字来规避命名冲突。在Spring Framework 5.2.3里面，也提供了一个类似功能的BeanNameGenerator,  就是在org.springframework.context.annotation包里面的FullyQualifiedAnnotationBeanNameGenerator。

```java
@Configuration
@ComponentScan(basePackages = "org.example", nameGenerator = MyNameGenerator.class)
public class AppConfig {
    // ...
}
```

```java
<beans>
    <context:component-scan base-package="org.example"
        name-generator="org.example.MyNameGenerator" />
</beans>
```



按照规则，每当其他组件可能对组件进行显式引用时，可以考虑使用上述凡是为注解指定名称，另一方面，当容器进行组装装配时，自动生成的bean名字就足够了。