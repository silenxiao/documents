# Java线程中的yield和join  
看到一篇介绍[yield和join](http://www.importnew.com/14958.html)的博客文章，我以为正如博客所说的那样，但事实上coding运行一下发现并不如此
```java
	package test.core.threads;
	public class YieldExample{
		public static void main(String[] args)
	    {
	      Thread producer = new Producer();
	      Thread consumer = new Consumer();
	 
	      producer.setPriority(Thread.MIN_PRIORITY); //Min Priority
	      consumer.setPriority(Thread.MAX_PRIORITY); //Max Priority
	 
	      producer.start();
	      consumer.start();
	    }
	}
 
	class Producer extends Thread
	{
   		public void run()
   		{
      		for (int i = 0; i < 5; i++)
      		{
         		System.out.println("I am Producer : Produced Item " + i);
         		Thread.yield();
      		}
   		}
	}
 
	class Consumer extends Thread
	{
   		public void run()
   		{
      		for (int i = 0; i < 5; i++)
      		{
         		System.out.println("I am Consumer : Consumed Item " + i);
         		Thread.yield();
      		}
   		}
	}
```
其输出的结果，并不是如博客所说的，在我的机器上不管有没有`yield()`都输出： 

	I am Consumer : Consumed Item 0
 	I am Consumer : Consumed Item 1
	I am Consumer : Consumed Item 2
	I am Consumer : Consumed Item 3
	I am Consumer : Consumed Item 4
	I am Producer : Produced Item 0
	I am Producer : Produced Item 1
	I am Producer : Produced Item 2
	I am Producer : Produced Item 3
	I am Producer : Produced Item 4
说明`yield`不能保证线程的交替执行（对于网上提供的实例还是自己编码跑跑才靠谱啊）

要达到交替执行，评论给了一个实现方式
	
	1.设置相同优先级
	2.每个`yield`执行前sleep 1毫秒