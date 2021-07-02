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

