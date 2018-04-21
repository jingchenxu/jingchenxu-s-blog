## 前端开发章程

> 有感于最近在管理一个项目中遇到的团队协作问题，觉得要好好的梳理下，如何保持团队在协作过程中的代码一致，处理好日常merge过程中出现的意外，个人感觉有必要规整下了。

### 代码管理模式

个人经历过的代码管理模式有手动合并，svn管理，git管理，当然最好的还是git管理了。

对于git管理我个人认为是最合适的，当时在管理项目的时候，需要项目成员对git都要有一定的了解，个人认为最简单的git工作流如下：

早上：

````bash
git fetch
git merge origin/master
````

晚上：

````bash
git add .
git commit -m --save
git push
````

以上的这个流程是对团队成员的开发流程，作为团队的管理人员，需要在项目中承担git管理的智能。

### 任务的分配模式

对于项目制的项目而言，个人认为，按照模块的划分是比较好的，前期不一定要有过多的规范，但是需要清晰的划分个人开发的模块，一切以进度为主，在开发的第一阶段，沟通的频次相对来说要高一点，可以做到每天沟通一次，但是沟通的时间不要太长，个人认为，一次沟通中产生大量的信息是没有必要的。

采用时间段频次高的沟通方式，是为了及时的了解到在项目开发中遇到的问题，做好日后优化的准备。

采用按模块划分任务的方式能够很好的明确个人的职责，对于锻炼成员的独立开发能力也会有所提高。

