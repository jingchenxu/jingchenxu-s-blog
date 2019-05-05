# 职责链模式

* 使用的场景

我们先说一种使用的场景，在电商活动中常会使用到的场景，那就是销售活动时制定的各种各样的规则，比如说我们最常见的消费额度不同，获得的优惠的额度也不同，我就举最近的一个充值优惠，冲300，减30；冲500减100；冲1000减300，我们需要根据不同的充值金额计算优惠金额。

* 通过职责链来实现

其实上面的代码通过简单的if/else就可以实现了，而且逻辑相对来说还是比较的清晰，但是相对来说代码的耦合性就比较的高，所有的业务逻辑都被耦合在if/else当中。

通过职责链来实现：

```javascript
        // 会员卡充值1000元
        var card1000 = function(money) {
            if(money == 1000) {
                console.log('会员卡充值1000元，优惠300元');
            }
            else {
                return 'nextSuccessor';
            }
        }

        var Lian = function( fn ) {
            this.fn = fn;
            this.successor = null;
        };

        // 指定在链中的下一个节点
        Lian.prototype.setNextSuccessor = function( successor ){
            return this.successor = successor;
        }

        // 传递请求给某个节点
        Lian.prototype.passRequest = function() {
            // 改变了函数的作用域和参数
            var rtv = this.fn.apply(this, arguments );
            if(rtv === 'nextSuccessor'){
                // 如果判断到当前的方法不符合条件，则调用下一个方法
                // 通过apply方法将当前节点获取到的数据传递给下个节点
                console.dir(this.successor);
                // TODO 这里不是很理解
                return this.successor && this.successor.passRequest.apply(this.successor, arguments);
            }
            // 执行到想要的结果就结束了
            return rtv;
        }

        var lian1000 = new Lian(card1000);
        var lian500 = new Lian(card500);
        var lian300 = new Lian(card300);

        lian1000.setNextSuccessor(lian500);
        lian500.setNextSuccessor(lian300);

        lian1000.passRequest(300);
```

在上面的代码中，我们活动的各个规则抽象为一个链条上的各个点，每个点回判断这件事是不是他来做，如果是他来做，这个节点则会完成要做的是，并且不再告知下一个节点，如果不是他来做，那就告诉下个节点。

从上面分析一下，我们在什么情况下使用职责链模式，首先各个职责点其实是存在顺序的，链条中只有一个点是符合情况的。

