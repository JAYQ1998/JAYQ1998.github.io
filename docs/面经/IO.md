## IO

#### 1. 介绍一下Java的序列化与反序列化

**参考答案**

序列化机制可以将对象转换成字节序列，这些字节序列可以保存在磁盘上，也可以在网络中传输，并允许程序将这些字节序列再次恢复成原来的对象。其中，对象的序列化（Serialize），是指将一个Java对象写入IO流中，对象的反序列化（Deserialize），则是指从IO流中恢复该Java对象。

**若对象要支持序列化机制，则它的类需要实现Serializable接口**，该接口是一个标记接口，它没有提供任何方法，只是标明该类是可以序列化的，Java的很多类已经实现了Serializable接口，如包装类、String、Date等。

**若要实现序列化，则需要使用对象流ObjectInputStream和ObjectOutputStream**。其中，在序列化时需要调用ObjectOutputStream对象的writeObject()方法，以输出对象序列。在反序列化时需要调用ObjectInputStream对象的readObject()方法，将对象序列恢复为对象。

如果对象在序列化中有些字段不想进行序列化，则使用transient关键字进行修饰。