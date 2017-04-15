---
layout: post
title:  "Java中单例模式的几种实现方式"
date:   2016-03-06
categories: Android
excerpt: Android
---


## **定义**
> 确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。

## **使用场景**
> 避免产生多个对象消耗过多的资源，或者某种类型的对象有且只有一个 (例如：访问IO和数据库资源。)

## **关键点**
> 1. 私有化构造函数。
> 2. 通过一个静态方法或者枚举返回单例类对象。
> 3. 确保单例类对象有且只有一个，尤其是在多线程环境下。
> 4. 确保单例类对象在反序列化时不会重新构建对象。

### *饿汉式*

````
/**
 * 单例 饿汉式
 * Created by hp1 on 2016/3/4.
 */
public class Singleton {

    private static Singleton instance = new Singleton();

    //函数私有化
    private Singleton() {	
    }

    public static Singleton getInstance() {
        return instance;
    }

}
````
### *懒汉式*

````
/**
 * 单例 懒汉式
 * Created by hp1 on 2016/3/4.
 */
public class Singleton {

    private static Singleton instance;

    private Singleton() {	
    }	

    /*
     * 获取实例
     * @return
     */
    public static synchronized Singleton getInstance(){
        if (instance == null) {
            instance=new Singleton();
        }
        return instance;
    }

}
````
#### 懒汉式缺点

- 第一次加载需要及时进行实例化，反应稍慢
每次调用getInstance都会进行同步，造成不必要的同步开销

### *Double Check Lock(双检锁)*

````
/**
 * 单例 Double Check Lock
 * Created by hp1 on 2016/3/4.
 */
public class Singleton {

    private static Singleton instance = null;
    
    private Singleton() {
    
    }
    
    /*
     *获取实例
     */
    public static Singleton getsInstance() {
        if (instance == null) {//为了避免不必要的同步
            synchronized (Singleton.class) {
                if (instance == null) {//为了在null的情况下创建实例
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
````

#### DCL缺点

- 第一次加载时反应慢，也由于java内存模型的原因偶尔会失败(高并发或JDK6以下版本)


### *静态内部类单例模式*


````
/**
 * 静态内部类单例模式
 * Created by hp1 on 2016/3/4.
 */
public class Singleton {

    private Singleton() {
    }

    /**
     * 获取实例
     * @return
     */
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }

    /**
     * 静态内部类
     */
    private static class SingletonHolder {
        private static final Singleton instance=new Singleton();
    }

}
````

#### 优点

- 第一次加载Singleton类并不会初始化instance，只有在第一次调用getInstance
方法时才会导致instance被初始化(延迟加载)。这种方式不仅能够确保线程安全，也能够保证单例对象的唯一性(利用Classloader的特性)，同时也延迟了单例的实例化，所以这是推荐使用的单例模式实现方式。

----------

### *枚举式*

````
/**
 * 枚举单例
 * Created by hp1 on 2016/3/4.
 */
public enum SingletonEnum {

    /**
     * 实例
     */
    INSTANCE;

    public void doSomething() {
        System.out.println("do sth...");
    }
    
}
````

#### 枚举单例的优点
- 枚举在java中与普通的类是一样的，不仅能够拥有字段，还能够
 有自己的方法。最重要的是默认枚举实例的创建时线程安全的，并且在任何
 情况下它都是一个单例。


----------


## **个人理解**

- 由于Android官方在关于管理应用内存方面给出的建议
![Android官方关于管理应用内存的建议](http://hukai.me/images/android_perf_3_enum.png) 

- 言外之意就是Enum在Android中内存占用比较高，所以不建议使用。
- 这里推荐使用**静态内部类单例模式**


## **总结**
> 不管以哪种方式实现单例模式，它们的核心都是将构造函数私有化，并且通过静态方法获取一个唯一的实例，在这个获取的过程中必须保证线程安全、防止反序列化导致的重新生成实例对象等问题。选择哪种实现方式取决于项目本身，如是否复杂的并发环境，JDK版本是否过低、单例对象的资源消耗等。
> 
> ——`《Android源码设计模式解析与实战》`






