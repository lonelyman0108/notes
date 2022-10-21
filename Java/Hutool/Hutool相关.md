## Hutool相关

### `URLEncodeUtil`

#### URL编码

使用`URLEncodeUtil`工具类中`encode`方法将URL编码，某些特殊字符未被转义，例如：`+`

```java
String path = "/3838600 etiq ID+code barre revB.SLDPRT";
encodedPath = URLEncodeUtil.encode(path);
System.out.println(encodedPath);
----------
/3838600%20etiq%20ID+code%20barre%20revB.SLDPRT
```

其中`+`并未被转义为`%2B`

正确结果应该为：

```java
/3838600%20etiq%20ID%2Bcode%20barre%20revB.SLDPRT
```

> Hutool版本：5.7.21