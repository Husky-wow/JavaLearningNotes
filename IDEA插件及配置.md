# 配置

## 1. 自动 import 无歧义类:

​	Settings --> 搜索 auto import --> 勾选 Add unambiguous imports on the fly

## 2. mybatis mapper 模板

​	settings --> Editor --> File and Code Templates --> 新建 mybatis-mapper.xml

​	使用如下模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    
    <mapper namespace="${mapper_interface_location}">
    
    </mapper>
```

## 3. 自定义一些好用的Live Template

1. Thread.sleep

   ```java
   // Abbreviation: sl
   
   try {
       Thread.sleep($A$);
   } catch (Exception e) {
       e.printStackTrace();
   }
   
   // Abbreviation: slnt
   
   Thread.sleep($A$);
   ```

2. JUnit

   ```java
   // Abbreviation: testj
   
   @Test
   public void test$A$() {
       $B$
   }
   ```

3. 键盘输入

   ```java
   // Abbreviation: si
   
   System.out.println("Enter...");
   System.in.read();
   ```

4. RuntimeException

   ```java
   // Abbreviation: tnr
   
   throw new RuntimeException($A$)
   ```

   

5. IDEA 生成序列号快捷键

   Settings --> Editor --> Inspections --> Java --> Serialization issues --> 勾选 Serializable class without seriaVersionUID

   此后创建的类如果实现了java.io.Serializable 接口的话，通过 alt + Enter 自动添加 seriaVersionUID

6. IDEA 编码

   Settings --> Editor --> File Encoding，将  Global Encoding 、Project Encoding、Default encoding for properties files 均设置为UTF-8

7. 当使用 IDEA + Gradle 时，如果控制台输出中文就会出现乱码，解决方法：Help --> Edit Custom VM Options --> 在所有配置的最后面加上一行配置：-Dfile.encoding=UTF-8 -->重启IDEA-->解决