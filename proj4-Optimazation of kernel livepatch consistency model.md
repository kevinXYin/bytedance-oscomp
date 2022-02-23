# proj4-Optimazation of kernel livepatch consistency model
### 项目名称
Linux内核热补丁一致性模型优化

### 项目描述与背景
1. Linux内核livepatch技术用于免重启对内核态代码进行打补丁，也就是业界说的内核热补丁。

2. consistency model，这里翻译做一致性模型，其要达到的目的就是保证打补丁时整个系统的状态是一致的，具体来说，假设内核函数foo，打补丁前的原始函数为 foo\_old，打完补丁后的函数为 foo\_new，那么对于任务来说，它不能一会执行的是foo\_old，一会执行的是foo\_new，否则就会出现不可预期的行为。

3. kernel livepatch的代码实现仅能达到 per-task consistency，也就是说对于单个任务来说，其在补丁生效前只能执行foo\_old的逻辑，补丁生效后只能执行 foo\_new 的逻辑，注意这里仅是per-task而不是整个系统的consistency，意味着同一个时刻，可能有的任务执行foo\_old的逻辑，有的任务执行替换后的foo\_new 的逻辑，这样在特定情况下就会有风险。

4. [kpatch](https://github.com/dynup/kpatch)  社区的实现中，其进行打补丁时所用的一致性模型是利用 stop_machine把机器停住，然后检查每个任务栈， 看要补丁的函数有无在运行，若没有，则可安全打补丁； 若有，则退出，然后再继续恢复机器运行，其中保证整个系统的consistency，也就是从所有任务的视角来看，要补丁的函数要么没被替换，要么已被替换，不存在有的任务跑foo\_old逻辑，有的任务跑foo\_new逻辑的情况


kpatch的stop_machine callback相比kernel livepatch更为安全，但是在多/众 核场景下的latency会更高，可能造成业务受损，需要一个既能保证稳定性又能兼顾性能的方案。



### 功能描述

#### 基础功能：
改进kernel的livepatch所用的consistency model，使其不再是per-task consistency，而是whole system consistency，同时保留其对于kpatch stop_machine方案的性能优势，要求最差性能情况下退化到stop machine的性能。

#### 扩展功能：


### 所属赛道

2022全国大学生操作系统比赛的“OS内核实现”赛道



### 参赛要求

- 以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生（2022年春季学期或之后本科毕业的大一~大四的学生）
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
- 请遵循“2022全国大学生操作系统比赛”的章程和技术方案要求



### 项目导师





### 难度

高


### 参考资料

[livepatch](https://www.kernel.org/doc/html/latest/livepatch/livepatch.html)

[kpatch](https://github.com/dynup/kpatch)  

[livepatch: consistency model](https://lwn.net/Articles/632582/)


### License

* [GPL-2.0](https://opensource.org/licenses/GPL-2.0)



## 预期目标

完成基础的开发并输出说明文档一篇。

**注意：下面的内容是建议内容，不要求必须全部完成。选择本项目的同学也可与导师联系，提出自己的新想法，如导师认可，可加入预期目标**

参考任务描述部分。