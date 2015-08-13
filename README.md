# design patterns
##Overview
This is for Java Design patterns, and alos for reading notes of the book **Head First Design Patterns**.


####Singleton
**description**:
ensures a class has only one instance, and provides a global point of access to it.

**Double checking version**:

```
1 public class Singleton{
2	private volatile static Singleton instance;
3	private Singleton{}
4	
5	public static Singleton getInstance{
6		if(instance == null){
7			synchronized(Singleton.class){
8				if(instance == null){
9					instance = new Singleton();
10				}
11			}
12		}
13	}
14 }
```
**解释**

1.同步代码中，仍然有一个判断，原因是如果没有这个判断，在多线程环境中，初始化实例的方法虽然不会同步发生，但是是**线性的，顺序的**，仍然会有多个实例产生。

2.在同步代码块外加判空的目的：
因为单例模式中，我们只需要初始化实例一次，剩下的操作都是读操作，但是每次我们都要进入同步代码块，导致性能很差，所以在之前加一个判空

3.那为何要用**Volatile**关键字呢，这是因为new 这个动作不是原子的，可以分为三步：

	1 为对象分配内存
	2 初始化实例和变量
	3 将引用指向分配的内存地址

经过JIT优化的程序，2 3 的顺序是不保证的，有可能就是132， 当线程A走到3时，实例已经不为空，但是变量还没有初始化，所以在线程B进行第一步判空的时候，直接返回了，但是却没有初始化，导致报错。而**Volatile**关键字的含义是：多线程不保存变量副本，直接从内存中取；不允许优化

**优雅版**

```
public static class SingletonHolder{
	private static Singleton instance;
	public Singleton getInstance(){
		return new instance();
	}
}
```
主要目的是在需要的时候在初始化实例，延迟初始化


