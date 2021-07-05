# String--截取与替换

```java
    static class Solution {
        public String replaceSpaces(String s1, int length) {
//            String one = s1.substring(0, length);  取所需要的长度
//            String two = one.replace(" ", "%20");  将制定内容替换
            return s1.substring(0, length).replaceAll(" ", "%20");
        }
    }
```



# String、StringBuffer、StringBuilder

```java
String是final修饰的，不可变，每次操作都会产生新的String对象
StringBuffer和StringBuilder都是在原对象上操作
StringBuffer是线程安全的，StringBuilder线程不安全的
StringBuffer方法都是synchronized修饰的
性能：StringBuilder > StringBuffer > String
场景：经常需要改变字符串内容时使用后面两个
优先使用StringBuilder，多线程使用共享变量时使用StringBuffer
```

