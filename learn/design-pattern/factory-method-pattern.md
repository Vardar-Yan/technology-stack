# <center>单例模式</center>

---
## 一、介绍

**1.1 定义**

&emsp;**工厂方法模式**，定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。


**1.2 适用场景**

&emsp;工厂方法模式实现时，客户端需要决定实例化哪一个工厂来实现运算类，选择判断的问题还是存在的，也就是说，工厂方法把简单工厂的内部逻辑判断移到了客户端代码来进行。你想要加功能，本来是改工厂类的，而现在是修改客户端。

---
## 二、实现

**问题描述：** 利用工厂方法实现加减乘除运算。

* 先构建一个运算类，其拥有加减乘除子类
```Java
@Data
public abstract Operation{
    protected double num1;
    protected double num2;
    public abstract double getResult();
}

public class OperationAdd extends Operation{
    //实现两数相加,返回结果
    public double getResult(){};
}
public class OperationSubtract  extends Operation{
    //实现两数相减，返回结果
    public double getResult(){};
}
public class OperationMultiply  extends Operation{
    //实现两数相乘，返回结果
    public double getResult(){};
}
public class OperationDivide  extends Operation{
    //实现两数相除，返回结果
    public double getResult(){};
}
```

* 再构建一个工厂接口,然后加减乘除各建一个具体工厂去实现这个接口。
```Java
public interface Factory{
    Operation createOperation();
}
public class AddFactory implements Factory{
    public Operation createOperation(){
        return new OperationAdd();
    }
}
public class SubtractFactory implements Factory{
    public Operation createOperation(){
        return new OperationSubtract();
    }
}
public class MultiplyFactory implements Factory{
    public Operation createOperation(){
        return new OperationMultiply();
    }
}
public class DivideFactory implements Factory{
    public Operation createOperation(){
        return new OperationDivide();
    }
}
``` 

* 测试类调用
```Java
public class Test{
    public static void main(String[] args){
        Factory operationFactory = new AddFactory();
        Operation operation = operationFactory.createOperation();
        operation.setNum1(1);
        operation.setNum2(2);
        double result = operation.getResult();
    }
}
```
