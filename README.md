//# MultiThreading-java
//I have designed producer consumer synchronized working thread model in java 
package multithreading;
class Product{
	int data;
	boolean status=false;
	synchronized void get(int data) throws InterruptedException {
		System.out.println("Producer thread ");
		if(status==true) {
			System.out.println("Producer goes in waiting state");
			wait();
		}
		this.data=data;
		System.out.println("Data added....."+data);
		status=true;
		notify();
	}
	synchronized void put() throws InterruptedException {
		System.out.println("Consumer thread ");
		if(status==false) {
			System.out.println("Consumer going into waiting state");
			wait();
		}
		//this.data=data;
		System.out.println("Data added....."+data);
		status=false;
		notify();
	}
}
class Producer extends Thread{
	Product p;

	/**
	 * @param p
	 */
	Producer(Product p) {
		super();
		this.p = p;
	}
	public void run() {
		for(int i=0;i<5;i++) {
			try {
				p.get(i);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
class Consumer extends Thread{
	Product p;

	/**
	 * @param p
	 */
	Consumer(Product p) {
		super();
		this.p = p;
	}
	public void run() {
		for(int i=0;i<5;i++) {
			try {
				p.put();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}
public class Demo4 {

	public static void main(String[] args) {
		Product p1=new Product();
		Producer p=new Producer(p1);
		p.start();
		Consumer c=new Consumer(p1);
		c.start();

	}

}

