# 单例模式

---
## 一、介绍

**1.1 定义**

&emsp;确保一个类仅有一个实例，并提供一个访问它的入口。


**1.2 作用**

&emsp;在设计的系统中，有些类其实只需要一个对象，如果只需要一个对象却new出多个对象，可能会导致一些问题的产生，使用单例模式，确保只生成一个对象，则避免问题的产生。还可以降低系统的开销，减轻CPU的压力。

---
## 二、实现


**2.1 饿汉式**

* 类字段上创建实例（线程安全）
```Java
public class Single{
    private static Single instance = new Single();
    private Single(){}

    public static Single getInstance(){
        return instance;
    }
}
```

* 枚举实现（推荐）（线程安全）
```Java
public enum Single{
    INSTANCE;
  
    public void doSomeThing(){
        System.out.println("枚举方法实现单例");
    }
}
```


**2.2 懒汉式**

* 非线程安全
```Java
public class Single{
    private static Single instance;
  
    private Single(){};

    public static Single getInstance(){
        if(instance == null){
        instance = new Single();
        }
        return instance;
    }
}
```

* synchronized关键字实现线程安全
```Java
public class Single{

    private static Single instance;
  
    private Single(){};

    public static synchronized Single getInstance(){
        if(instance == null){
            instance = new Single();
        }
        return instance;
    }
}
```

* 双重检查加锁
```Java
public class Single{
  
    private volatile static Single instance;
  
    private Single(){};
 
    public static Single getInstace(){
    
        if(instance == null){
            synchronized(Single.class){
                if(instance == null){
                    instance = new Single();
                }
            }
        }
        return instance;
    }
}
```


* 静态内部类实现（线程安全）
```Java
public class Single{

    private static class SingleHolder{
        private static final Single instance = new Single();
    }
  
    private Single(){};
  
    public static final Single getInstance(){
        return SingleHolder.instance;
    }
}
```