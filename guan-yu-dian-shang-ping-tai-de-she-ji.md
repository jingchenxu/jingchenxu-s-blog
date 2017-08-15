## 关于电商平台的设计

> 大概一年的时间吧！目前经历了三个电商项目，目前经历的电商项目相对来说还是比较简单的，主要体现在商品类型的单一性，所以该电商并不是通用的电商模式，但是在开发的过程中遇到的很多问题还是值得以后思考的。

- 数据库表设计

注意事项1：项目在进行数据库库表设计时多半会使用powerdesigner,但是在设计表和字段时候，最好是将字段的备注和表的备注添加到数据库当中，可以通过在使用powerdesigner设计时完善备注字段，如果你使用的sql语句来创建表，则可以在表或是字段中添加comment信息，这样在使用的时候回相对方便一点，无需使用pdm才能查看表或字段的意义。

处于后台管理模块的表的设计，我们先聚焦电商模块表的设计，

- 模块设计

【订单确认】关于订单确认这一块的设计，我们需要考虑到一下几点：地址选择、商品信息、发票信息、优惠信息。订单确认页的信息应该时候后台接口根据实际情况生成的，如果将大量的业务逻辑交由前台进行处理，则需要频繁的修改前台页面的业务逻辑，而且业务逻辑暴露在前台也是件危险的事情。

商品信息：建议商品信息在订单确认页是不可修改的，但是修改的也是可以的。

- 多平台设计

是否需要响应式？

如果你的电商网站需要在pc浏览器和手机浏览器

