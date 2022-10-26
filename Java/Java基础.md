## Java基础

### SPI机制

#### 概念描述

SPI（Service Provider Interface），是Java提供的一套用来被第三方实现或者扩展的API，它可以用来启用框架扩展和替换组件。

整体机制图如下：![java-advanced-spi-8](../asset/img/java-advanced-spi-8.jpg)

#### 使用方法

当服务的提供者提供了一种接口的实现之后，需要在`classpath`下的`META-INF/services/`目录里创建一个以服务接口命名的文件，这个文件里的内容就是这个接口的具体的实现类。当其他的程序需要这个服务的时候，就可以通过查找这个jar包（一般都是以jar包做依赖）的`META-INF/services/`中的配置文件，配置文件中有接口的具体实现类名，可以根据这个类名进行加载实例化，就可以使用该服务了。

JDK中查找服务的实现的工具类是：`java.util.ServiceLoader`。

#### 示例

1. 定义接口与实现类

   - 接口

     ```java
     public interface Search {
         public List<String> doSearch(String keyword);   
     }
     ```

   - ES搜索实现类

     ```java
     public class EsSearch implements Search{
         @Override
         public List<String> doSearch(String keyword) {
              doEsSearch(keyword);
             return null;
         }
     }
     ```

   - 数据库搜索实现

     ```java
     public class DBSearch implements Search{
         @Override
         public List<String> doSearch(String keyword) {
             doDBSearch(keyword);
             return null;
         }
     }
     ```

2. 在`resources`下新建`META-INF/services/`目录，新建接口全限定名的文件：`site.lonelyman.demo.service.Search`，文件内输入需要使用的实现类的全限定名

   ```
   site.lonelyman.demo.service.impl.DBSearch
   ```

3. 测试

   ```java
   public class SPITest {
       public static void main(String[] args) {
           ServiceLoader<Search> s = ServiceLoader.load(Search.class);
           Iterator<Search> iterator = s.iterator();
           while (iterator.hasNext()) {
              Search search = iterator.next();
              search.doSearch("xxx");
           }
       }
   }
   ```

   如果在`site.lonelyman.demo.service.Search`内写上两个实现类，就会将两个实现类中的方法都执行一次。

> 参考来源：[Java常用机制 - SPI机制详解](https://pdai.tech/md/java/advanced/java-advanced-spi.html)
