### How to configure the Spring boot REST API with Swagger? ###
Even though REST services are document itself by using hetoas. We can use Swagger to visualizing API.
- We need to add springfox swagger and swagger ui dependencies to out project
```XML
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.6.1</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.6.1</version>
    <scope>compile</scope>
</dependency>
```
- Configure the swagger2 to identify the application api's
```JAVA
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket productApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()                 
                .apis(RequestHandlerSelectors.basePackage("com.fuppino.controllers"))
                .paths(regex("/product.*"))
                .build();
    }
}
```

### What are different MicroServices patterns? ###
[ref](https://www.youtube.com/watch?v=iJVW7v8O9BU)
- Aggregator Pattern
	- Individual services with separate cache and DB
	- All independent services are called by a Aggregator component to combine the services result
	- Aggregator component can be Load balance, the public exposer is Aggregator component
	- Individual services are not exposed to outside
![Aggregator Pattern](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Aggregator_Pattern.png)

- Proxy Pattern
	- Individual services with separate cache and DB
	- Instead of Aggregator component, It might have Proxy layer
	- the major difference between Aggregator component and Proxy Pattern, Aggregator combines the result. Where as Proxy Component do data convertion like binary or format convertion like JSON format etc.
	- Proxy component can be Load balance, the public exposer is Proxy component
![Proxy Pattern](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Proxy_Pattern.png)

- Chained Pattern
	- It's like calling one service to other service
	- Client sees only the one public service, but internally it may call multiple services to prepare the data
	- dis.adv: if you have chained multiple services then it increases the client waiting time ot if one node got failed to generate the data, the remaining node behaviour changes.
![Chained Pattern](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Chained_Pattern.png)

- Branch Pattern
	- Depending on business logic the component can change the chain
	- It's like composition pattern
	- Easy to adapt new changes in service.

![Branch Pattern](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Branch_Pattern.png)

- Shared Resources
	- With in Branch pattern, it can share the rest resources
	- It's not sharing database tables, it's only sharing the database.

![Shared Resources](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Shared_Resources.png)

- Async Messaging
	- Som services are communicating using queues or async calls between services
![Async Messaging](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Async_Messaging.png)

- Mediator topology
	- a star style topology. The mediator sends a message, get the reply and send other message based on the reply to the next Micro-Service.
![Mediator topology]()

- Broker topology
	- a chain based topology. The messages are passed thru the Micro-Services. Each one will reply the outcome to the next one.
![Broker topology]()



### Design Principles for Monoliths ###
- DDD - Domain Driven Design
- SoC using MVC - Separation of Concern
- High cohesion, low Coupling
- DRY
- CoC - Convention over Configuration
- Yagni - You aren't gonna need it [ref](https://martinfowler.com/bliki/Yagni.html)


- What is parallel system ?
	- A parallel system consists of multiple processors that communicate with each other using shared memory.

- What is distributed system ?
	- The computer systems that contains multiple processors connected by a communication network.

- What is process?
	- When process has its own code and data, it is called a heavyviezght process, or simply a process.

- What is thread?
	- When processes share the addrcss space, namely, code and data, then they are called lightweight processes or threads. All threads share the address space but have their own local stack.

- What is difference between Thread safety and Concurrence?
	- Thread safety, is to protect shared data and code
	- Concurrence, is to make it available

- What is critical region?
	- A section of the code that needs to be executed atomically is also called a critical region or a critical section.

- Write a Java class that allows parallel search in an array of integer. It provides the following method:
public static void parallelSearch(int x, int[] A, int numThreads)

```JAVA
public class Searcher implements Runnable {
    private int intToFind;
    private int startIndex;
    private int endIndex;
    private int[] arrayToSearchIn;

    public Searcher(int x, int s, int e, int[] a) {
        intToFind = x;
        startIndex = s;
        endIndex = e;
        arrayToSearchIn = a;
    }

    public void run() {
        for (int i = startIndex; i <= endIndex; i++) {
            if (arrayToSearchIn[i] == intToFind) 
            	System.out.println("Found x at index: " + i);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        int[] a = {0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20};
        int numberOfThreads = 5;
        int x = 20;
        findElement(numberOfThreads, x, a);
    }

    private static void parallelSearch(int x, int[] a,int numberOfThreads) {
        int sizeOfa = a.length;
        int range = sizeOfa/numberOfThreads;
        for (int i = 0; i <= numberOfThreads-1; i++) {
            Thread searcher;
            if (i == numberOfThreads-1) {
                searcher = new Thread(new Searcher(x, i*range, sizeOfa-1, a));
            } else {
                searcher = new Thread(new Searcher(x, i*range, i*range+range-1, a));
            }
            searcher.start();
        }
    }
}
```

- Write java program using multi thread to generate Fibonacci number value for an interger

```JAVA
public class Fibonacci extends Thread { 
	int n;
	int result;
	public Fibonacci (int ii) {
		this.n=n; 
	}
	public void run () {
		if ((n == O) || (n == 1)) result = 1;
		else { 
			Fibonacci f1 = new Fibonacci(n-1); 
			Fibonacci f2 = new Fibonacci(n-2); 
			f1.start();
			f2. start ();
			try {
				f1.join(); 
				f2.join ();
			} catch ( InterruptrdException e ) { }
			result = f1.getResult() + f2.getResult();
		}
}
public int getResult (){
	return result ;
}
public static void main(String [] args) {
	Fibonacci f1 = new Fibonacci(Integer.parsrInt(args[O]));
	f1.start();
	try {
		f1.join();
	} catch (InterruptedException e ) { } ;
	System.out.println("Answer is :" + f1.getResult 0)
}
```

## Dead lock with Example ##
```JAVA
class BCell { //can result a deadlocks int value;
	public synchronized int getvalue () {
		return value;
	}
	public synchronized void setvalue ( int i ) {
		value = i ;
	}
	public synchronized void swap(BCel1 x ) {
		int temp = getvalue (); 
		setvalue (x.getvalue 0); 
		x.setvalue (temp);
	}
}
```
	
- Assume that we have two objects, p and q, as instances of class BCell. What happens if a thread t1 invokes p.swap(q) and another thread, say, t2, invokes q.swap(p) concurrently? Thread t1 acquires the lock for the monitor object p and t2 acquires the lock for the monitor object q. Now, thread t1 invokes q.getValue() as part of the swap method. This invocation has to wait because object q is locked by t2. Similarly, t2 has to wait for the lock for p, and we have a deadlock!

### How to aviod Dead lock in the above situation ###
```JAVA
class Cell { 
	int valce;
	public synchronized int getvalue(){
		return value
	}
	public synchronized void setvalue(int i) {
		value = i;
	}
	protected synchronized void doSwap(Cell x) {
		int temp = getvalue (); 
		srtValue(x.getValue());
		x.setvalue(temp);
	}
	public void swap(Cell x) { 
		if (this==x)
			return ;
		else if(System.identityHashCode(this) 
		< System.identityHashCode(x)) 	
			doSwap(x);
		else
			x.doSwap(this);
	}
}
```
- Threading Comparision
	- [Bird View](https://dzone.com/articles/a-birds-eye-view-on-java-concurrency-frameworks-1)
	- [Git Repo](https://github.com/vijay-vk/java-concurrency)
	
- Synchronization easy understanding examples?
	- [Synch - part1](https://dzone.com/articles/how-synchronization-works-in-java-part-1)
	- [Synch - part2](https://dzone.com/articles/how-synchronization-works-in-java-part-2)
	
- Can we Serialize and de-Serialize the complete JVM?
	- No, JSR323 is a specification which has raised for strong mobility of JVM HotSpot.
	- [JSR323](https://jcp.org/en/jsr/detail?id=323)
	- There are some systems which provides container or cluster, where we can store the complete cluster and restore that. e.g: [terracotta](http://www.terracotta.org/)


- Oauth2 step-by-step code and ref
	- [Gihub code](https://github.com/dvinay/oauth2-step-by-step)
	- [ref](http://www.swisspush.org/security/2016/10/17/oauth2-in-depth-introduction-for-enterprises)












