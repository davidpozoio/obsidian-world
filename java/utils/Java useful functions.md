## Timer
To create a a timer that executes after a delay, this function is equivalent to `setInterval` in javascript.
```java
import javax.swing.Timer;

Timer timer = new Timer(DELAY_IN_MILLISECONDS, (e)->{
	System.out.println("hello");
});

timer.start();
```
Stop timer if an error occurred
```java
Timer timer = new Timer(DELAY_IN_MILLISECONDS, (e)->{
	try {
		// some code
	}catch (Error error){
		((Timer) e.getSource()).stop();
	}
});
```