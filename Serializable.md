1. Serializable接口

   ​	类实现Serializable接口后就被序列化；

   ​	类实现Serializable接口后，需要在类中添加serialVersionUID变量并给其赋值；（如不显示赋值，存在默认值）

2. serialVersionUID的作用
   1. Java序列化机制通过判断serialVersionUID的验证版本一致性；
   2. 在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常，即是InvalidCastException。
   3. 有个比较有意思的点，如果不显示给该变量赋值，程序会给默认值。而该默认值在你对类进行修改后会发生改变；因此，如果你将类序列化后对该类进行更改，反序列化时会因为serialVersionUID不一致而报错；
   4. 如果显示对serialVersionUID赋值，只要你不重新生成serialVersionUID，其值就不会改变，依然可以反序列化。

