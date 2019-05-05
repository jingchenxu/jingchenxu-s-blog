# LOCUST 压测实战

* 测试背景

最近正在开发一个分享红包的功能，之前做过一个类似的抽奖活动，这类功能的开发存在一个并发的问题，这类功能通常会设计到2张表，一张表为抽奖记录表，还有一张是抽奖汇总表，在插入或更新抽奖记录表时同时还需要更新抽奖记录汇总表，新增抽奖记录通常是有条件限制的，如果在Java中做条件限制那是不可能的，也没有效果的，所以相关的限制只能添加在SQL语句当中。

* 测试业务

关于点击链接分享红包，红包的具体数据都是预先生成好的，每个人点击以此分享链接，则update一条数据，至于update第几条数据与点击链接的人时第几个人有关系，已领取的数量存放在链接主表当中，一般这样的情况会利用到乐观锁来进行数据的校验，主要用于校验当前是否满足插入或更新的条件，只有在满足更新条件的情况下才能更新数据。

* 测试代码

具体的测试代码如下：

```python
from locust import HttpLocust, TaskSet, task

class UserBehavior(TaskSet):
  def on_start (self):
    print('xxx活动压力测试开始')

  @task(2)
  def user1(self):
    self.client.get("/xxx/share/getshare?openid=1&orderid=1")

  @task(2)
  def user2(self):
    self.client.get("/xxx/share/getshare?openid=2&orderid=1")

  @task(1)
  def user3(self):
    self.client.get("/xxx/share/getshare?openid=3&orderid=1")

  @task(2)
  def user4(self):
    self.client.get("/xxx/share/getshare?openid=4&orderid=1")

  @task(2)
  def user5(self):
    self.client.get("/xxx/share/getshare?openid=5&orderid=1")

class WebsiteUser(HttpLocust):
  task_set = UserBehavior
  host='https://xxxx.xxxx.com'
  max_wait = 5000
  min_wait = 3000
```

