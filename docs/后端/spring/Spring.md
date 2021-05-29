# Spring

## IoC

> 社会主义最高阶段的中国，就是一个超级容器。
>
> 现阶段都是自己买房、找老婆、帮小孩儿找学校，现在控制反转！进入了社会主义最高级阶段，中国是个大容器，你长到23岁了，会按需分配，自动发房子，自动发老婆。

```r
IoC：( Inversion of Control，控制（权）反转)，它是一种设计思想，就是将原本在程序中手动创建对象的控制权，交由Spring框架来管理。 IoC在其他语言中也有应用，并非Spring特有。IoC容器是Spring用来实现IoC的载体，IoC容器实际上就是个Map（key，value），Map中存放的是各种对象。
```

```java
1. 主动式：要什么资源都自己创建。
    BookServlet{
    	BookService bs = new BookService();
    	AirPlane ap = new AirPlane0; //复杂对象的创建是比较庞大的工程
    }
2. 被动式：资源的获取不是我们自己创建，而是交给一个容器来创建和设置。
    BookServlet{
    	BookService bs;
    	public void test010{
    		bs.checkout();//
    	}
    }
```





简单工厂





通用工厂

```java
package com.baizhiedu.basic;

import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class BeanFactory {
    private static Properties env = new Properties();
    static{
        try {
            //第一步 获得IO输入流
            InputStream inputStream = BeanFactory.class.getResourceAsStream("/applicationContext.properties");
            //第二步 文件内容 封装 Properties集合中 key = userService value = com.baizhixx.UserServiceImpl
            env.load(inputStream);

            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
     /*
      key 小配置文件中的key [userDAO,userService]
      */
     public static Object getBean(String key){
         Object ret = null;
         try {
             Class clazz = Class.forName(env.getProperty(key));
             ret = clazz.newInstance();
         } catch (Exception e) {
            e.printStackTrace();
         }
         return ret;
     }

}
```

### DI（Dependency Injection）依赖注入

就是IoC



## AOP

