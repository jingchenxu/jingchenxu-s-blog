## java多线程

> 微信及支付宝在支付成功后会向服务端进行回调，为确保支付回调成功，微信或是支付宝会发起多次回调，如果将回调后执行的业务代码写为同步的代码，会导致的问题是，由于业务代码执行的时间较长，可能会导致多次回调多次执行。

- 采用线程类开启多线程

通过继承的方式创建一个线程类：

````java
	private class Threadwxreturn extends Thread{
		private String paytype;
		private String orderid;
		private String appid;
		private String transactionid;
		private ReturnValue rtv;
		public void setPaytype(String paytype){
			this.paytype = paytype;
		}
		public void setOrderid(String orderid){
			this.orderid = orderid;
		}
		public void setAppid(String appid){
			this.appid = appid;
		}
		public void setTransactionid(String transactionid){
			this.transactionid = transactionid;
		}
		public void setRtv(ReturnValue rtv){
			this.rtv = rtv;
		}
		public void run() {
			logger.info("开启线程为"+orderid+"进行拆单");
			orderservice.RetuenChai(paytype, orderid, appid, transactionid, rtv);
			if(rtv.isSuccess()){
				logger.info(orderid+"拆单成功");
			}
			else {
				logger.error(orderid+"拆单失败");
			}
		}
	}
````

调用线程执行业务代码：

````java
                       Threadwxreturn threadwxreturn = new Threadwxreturn();
			threadwxreturn.setAppid(item.getAppid());
			threadwxreturn.setOrderid(orderid);
			threadwxreturn.setTransactionid(item.getTransaction_id());
			threadwxreturn.setPaytype(paytype);
			threadwxreturn.setRtv(rtv);
			Thread thread1 = new Thread(threadwxreturn);
			thread1.start();
````