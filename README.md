# part-6_Practical-5

package part6;

import java.util.*;

public class Practical_5 {

	public static void main(String[] args) throws InterruptedException {

		final PC pc = new PC();

		Thread t1 = new Thread(new Runnable() {
			public void run() {
				try {
					pc.produce();
				} catch (InterruptedException e) {
					System.out.println("Something Interrupted...");
				}
			}
		});
		Thread t2 = new Thread(new Runnable() {
			public void run() {
				try {
					pc.consume();
				} catch (InterruptedException e) {
					System.out.println("Something Interrupted...");
				}

			}
		});

		t2.start();
		t2.join();
	}

	public static class PC {
		LinkedList<Integer> list = new LinkedList<>();
		int capacity = 2;

		public void produce() throws InterruptedException {
			int value = 0;
			while (true) {
				synchronized (this) {

					while (list.size() == capacity)
						wait();

					System.out.println("Producer Produced ... -" + value);

				}
			}
		}

		public void consume() throws InterruptedException {
			while (true) {
				synchronized (this) {

					while (list.size() == 0)
						wait();

					int val = list.removeFirst();

					System.out.println("Consumer Consumed ... -" + val);

				}
			}
		}
	}

}
