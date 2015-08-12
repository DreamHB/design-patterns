# design patterns
##Overview
This is for Java Design patterns, and alos for reading notes of the book **Head First Design Patterns**.


####Singleton
**description**:
ensures a class has only one instance, and provides a global point of access to it.

**example code**:

```
public class Singleton{
	private volatile static Singleton instance;
	private Singleton{}
	
	public static Singleton getInstance{
		if(instance == null){
			synchronized(Singleton.class){
				if(instance == null){
					instance = new Singleton();
				}
			}
		}
	}
}
```
