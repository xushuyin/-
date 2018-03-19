package cn.test.callback;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

/**
 * ExecutorService.submit会按照线程先后顺序获取结果，
 * 如： 第一个执行10s，第二个执行5s，无法在第一个任务未执行完成之前获取结果
 * 想实现这种效果，可以使用CompletionService
 */
public class CallBackAndFuture {
	public static void main(String[] args) {
		ExecutorService executorService = Executors.newCachedThreadPool();
		List<Future<String>> resultList = new ArrayList<Future<String>>();

		for (int i = 0; i < 10; i++) {
			// 使用ExecutorService执行Callable类型的任务，并将结果保存在future变量中
			Future<String> future = executorService.submit(new TaskWithResult(i));
			
			resultList.add(future);
		}
		// 调用后，不可以再submit新的task，已经submit的将继续执行。
		executorService.shutdown();
		
		// 试图停止当前正执行的task，并返回尚未执行的task的list
		// executorService.shutdownNow();

		for (Future<String> fs : resultList) {
			try {
				// 线程未执行完成时，fs.get()获取返回值时会阻塞
				System.out.println(fs.get()); // 打印各个线程（任务）执行的结果
			} catch (InterruptedException e) {
				e.printStackTrace();
			} catch (ExecutionException e) {
				executorService.shutdownNow();
				e.printStackTrace();
				return;
			}
		}
	}
}

class TaskWithResult implements Callable<String> {
	private int id;

	public TaskWithResult(int id) {
		this.id = id;
	}

	// 类似Runnable的run()，用于线程回调
	@Override
	public String call() throws Exception {
		System.out.println("call()方法被自动调用,干活！！！             " + Thread.currentThread().getName());
		//if (new Random().nextBoolean())
			//throw new TaskException("Meet error in task." + Thread.currentThread().getName());
		
		// 一个模拟耗时的操作
		Thread.sleep(2000);
		
		return "call()方法被自动调用，任务的结果是：" + id + "    " + Thread.currentThread().getName();
	}
}

class TaskException extends Exception {
	
	private static final long serialVersionUID = 1L;

	public TaskException(String message) {
		super(message);
	}
}
