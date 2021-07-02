# 1)用于注册bean对象注解

## 1.1@Component

### **作用：**

```java
调用无参构造创建一个bean对象，并把对象存入spring的IOC容器，交由spring容器进行管理。相当于在xml中配置一个bean。
```

### **属性：**

```java
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写
```



## 1.2 @Controller

### 作用：

```java
作用上与@Component。一般用于表现层的注解。
```



### 属性：

```java
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```

