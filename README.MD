# Design Pattern
* Helps to identify recurring problem and provides ready to use solution.
* consider -> solution problem context
* solution for a problem in a given environment or CONTEXT
# Design Pattern Catolog
* [Gang Of Four(CORE)](#core(gof)-design-patterns)
	* [Creational Patterns](#creational-patterns)
* [J2EE Desing Patterns](#j2ee-design-patterns)
	
# Creational Patterns
* [Singleton](#singleton)
## Singleton
* Make Constructor private
* Create static variable to hold the instance - make it volatile
* Static Method to get singleton object

```
public class LoggerUtil {
	public static volatile LoggerUtil instance;
	private LoggerUtil() {
	}
	public static LoggerUtil getInstance() {
		if (instance == null) {
			instance = new LoggerUtil();
		}
		return instance;
	}
	public void info(String msg) {
		System.out.println(msg);
	}
}
```

**Handle Mulithreading** <br>
```
public static LoggerUtil getThreadSafeInstance() {
		if (instance == null) {
			synchronized (LoggerUtil.class) {
				if (instance == null) {
					instance = new LoggerUtil();
				}
			}
		}
		return instance;
	}
```
**Handle Cloning** <br>
```
@Override
public Object clone() throws CloneNotSupportedException {
	throw new CloneNotSupportedException();
}	
```
**Handle Serialization** <br>
_When we desrialize the object from file new object will be created, we can avoid this by overriding **readResolve** of **Serializable** interface
```
public Object readResolve() {
	return getThreadSafeInstance();
}	
```
**Proper Singleton with all cases handled** <br>
```
public class LoggerUtil implements Cloneable,Serializable{
 
	private static final long serialVersionUID = 1L;
	private static volatile LoggerUtil instance;

	private LoggerUtil() {
		System.out.println("IN CONSTRUCTOR");
	}

	public static LoggerUtil getInstance() {
		if (instance == null) {
			instance = new LoggerUtil();
		}
		return instance;
	}

	public static LoggerUtil getThreadSafeInstance() {
		if (instance == null) {
			synchronized (LoggerUtil.class) {
				if (instance == null) {
					instance = new LoggerUtil();
				}
			}
		}
		return instance;
	}
 
	public Object readResolve() {
		return getThreadSafeInstance();
	}
	
	@Override
	public Object clone() throws CloneNotSupportedException {
		throw new CloneNotSupportedException();
	}	
	 
	public void info(String msg) {
		System.out.println(msg);
	}

}

```
