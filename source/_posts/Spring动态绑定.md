---
title: Spring 动态绑定
categories: 笔记
tags:
  - Spring
  - 动态绑定
description: <center>通过 Spring 动态绑定实现多实例注入。</center>
abbrlink: 1655783544
date: 2022-01-07 19:55:59
---

# 需求场景

在系统运行时，根据不同的场景需要多个类实例。比如一个 Redis 类，需要传入不同的 Category（可以理解为 Key 前缀），从而实现多个 Redis 子类。

# 步骤一、自定义注解

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface CacheService {
    String category() default "";
}
```

可以设置为 `String value() default "";` ，使用时可以隐藏字段名。

# 步骤二、扫描属性上的注解

```java
Field[] fields = object.getClass().getDeclaredFields();
CacheService annotation = field.getAnnotation(CacheService.class);
```

# 步骤三、向容器注入 Bean

```java
// 获取 context
ConfigurableApplicationContext context = (ConfigurableApplicationContext) applicationContext;
// 获取 BeanFactory
DefaultListableBeanFactory beanFactory = (DefaultListableBeanFactory) context.getBeanFactory();
// 创建 bean 信息
BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.genericBeanDefinition(RedisService.class);

// 设置字段
beanDefinitionBuilder.addPropertyValue("category", category);
beanFactory.registerBeanDefinition(field.getName(), beanDefinitionBuilder.getBeanDefinition());

// 获取动态注册的 bean
RedisService bean = (RedisService) applicationContext.getBean(field.getName());
field.setAccessible(true);
field.set(object, bean);
```

# 步骤四、使用

```java
@CacheService(category = Constants.ORDER_DRIVER)
private RedisService orderDriverCache;
```

# 示意图

![](../images/DynamicBinding.png)

# 全部示例代码

```java
// RedisService.java
public class RedisService {
    @Resource(name = "redisNioClient")
    private SquirrelClient redisNioClient;

    @Resource(name = "redisClient")
    private RedisStoreClient redisClient;

    @Setter
    private String category;
}

// CacheService.java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface CacheService {
    String category() default "";
}

// CacheServiceAnnotation.java
@Component
@Slf4j
public class CacheServiceAnnotation implements ApplicationListener<ContextRefreshedEvent> {

    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        final ApplicationContext applicationContext = event.getApplicationContext();

        Arrays.stream(applicationContext.getBeanDefinitionNames())
                .map(applicationContext::getBean)
                .forEach(object -> {
                    final Field[] fields = object.getClass().getDeclaredFields();
                    Arrays.stream(fields)
                            .filter(field -> field.isAnnotationPresent(CacheService.class))
                            .forEach(field -> {
                                final RedisService bean = getRedisService(applicationContext, field);
                                try {
                                    field.setAccessible(true);
                                    field.set(object, bean);
                                    log.info("set field success, object: {}, bean: {}", object.getClass().getSimpleName(), field.getName());
                                } catch (IllegalAccessException e) {
                                    log.error("set field fail, object: {}, bean: {}", object.getClass().getSimpleName(), field.getName(), e);
                                }
                            });
                });
    }

    private RedisService getRedisService(ApplicationContext applicationContext, Field field) {
        if (applicationContext.containsBeanDefinition(field.getName())) {
            return (RedisService) applicationContext.getBean(field.getName());
        }

        final CacheService annotation = field.getAnnotation(CacheService.class);
        final String category = annotation.category();

        ConfigurableApplicationContext context = (ConfigurableApplicationContext) applicationContext;
        DefaultListableBeanFactory beanFactory = (DefaultListableBeanFactory) context.getBeanFactory();
        BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.genericBeanDefinition(RedisService.class);

        // 设置字段
        beanDefinitionBuilder.addPropertyValue("category", category);
        beanFactory.registerBeanDefinition(field.getName(), beanDefinitionBuilder.getBeanDefinition());

        // 获取动态注册的 bean
        RedisService bean = (RedisService) applicationContext.getBean(field.getName());
        log.info("register Bean success, bean: {}", field.getName());
        return bean;
    }
}
```



参考资料：

[Java获取类、方法、属性上的注解_根号三-CSDN博客_java 获取属性上的注解](https://blog.csdn.net/u011983531/article/details/70941123)

[使用自定义注解动态绑定多实现类实例](https://www.cnblogs.com/east7/p/14390816.html)

[SpringBoot动态注入及操作Bean](https://segmentfault.com/a/1190000024522038)
