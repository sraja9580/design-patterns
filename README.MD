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

`
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

`
*Singleton in Mulithreading Environmane*

