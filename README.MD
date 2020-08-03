# Design Pattern
* Helps to identify recurring problem and provides ready to use solution.
* consider -> solution problem context
* solution for a problem in a given environment or CONTEXT
# Design Pattern Catolog
* [Gang Of Four(CORE)](#core(gof)-design-patterns)
	* [Creational Patterns](#creational-patterns)
	* [Behavioural  Patterns](#behavioural-patterns)
* [J2EE Desing Patterns](#j2ee-design-patterns)
	
# Creational Patterns
* [Singleton](#singleton-pattern)
* [Factory](#factory-pattern)

## Singleton
Singleton pattern restricts the instantiation of a class and ensures that only one instance of the class exists in the java virtual machine.
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
When we desrialize the object from file new object will be created, we can avoid this by overriding **readResolve** of **Serializable** interface
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
## Factory Pattern
 In Factory pattern, we create object without exposing the creation logic to client and the client use the same common interface to create new type of object.
 A factory pattern is one of the core design principles to create an object, allowing clients to create objects of a library(explained below) in a way such that it doesn’t have tight coupling with the class hierarchy of the library <br> ex: **java.util.Calendar**<br>
 
**Interface and Implementation** <br>
 ```
 interface FMCDJob {
	void execute();
}

class FDSDealerJob implements FMCDJob{
	@Override
	public void execute() {
		System.out.println("Executing NA FDSDealer job");
	}	
}

class FDSEmpJob implements FMCDJob{
	@Override
	public void execute() {
		System.out.println("Executing NA FDSEmployee job");
	}	
}
 ```
**Factory** 
 ```
 class FMCDJobFactory {
	public static FMCDJob getJob(String jobName) {
		FMCDJob fmcdJob = null;
		if(jobName.equals("dealer")) {
			fmcdJob = new FDSDealerJob();
		}else if(jobName.equals("emp")) {
			fmcdJob = new FDSEmpJob();
		}
		return fmcdJob;
	}
}

public class FactoryPattern {
	public static void main(String args[]) {
		FMCDJobFactory.getJob("dealer").execute();
		FMCDJobFactory.getJob("emp").execute();
	}
}
 ```
# Behavioural  Patterns
* [Template Method pattern](#template-method-pattern)
 
## Template Method pattern
The Template Method pattern is known as a behavioural pattern,as it's used to manage algorithms, relationships and responsibilities between objects.Thedefinition of Template Method provided in the original Gang of Four book on DesignPatterns states: <br>
_Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithms structure._
```
abstract class FMCDFTPJob {
	public final void executeJob() {
		downloandFtpFile();
		processFile();
		cleanUpFile();
	}
	abstract void processFile();
	private final void downloandFtpFile() {
		System.out.println("Downloading file from SFTP server");
	}
	private void cleanUpFile() {
		System.out.println("Cleaning file from local after processing");
	}
}

class LeadsJob extends FMCDFTPJob{
	@Override
	void processFile() {
		System.out.println("Read Leads file and update in database");		
	}	
}

class DisneJob extends FMCDFTPJob{
	@Override
	void processFile() {
		System.out.println("Read Disne file and update in database");		
	}	
}

public class BehaviouralPattern {
	public static void main(String args[]) {
		FMCDFTPJob ftpjob1 = new LeadsJob();
		FMCDFTPJob ftpjob2 = new DisneJob();
		ftpjob1.executeJob();
		System.out.println("------------------------");
		ftpjob2.executeJob();
	}
}
```
