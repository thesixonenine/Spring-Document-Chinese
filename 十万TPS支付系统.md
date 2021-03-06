## 十万TPS支付系统

#### 架构

1. 账务系统

   1. 计算与存储分离
   2. 分库
   3. 分布式事务(能省则省)

   缺陷: 分库的数量是一定的: id%100=0, id%100=1, id%100=2 ...... id%100=99. 固定了只有100个MySQL, 难以扩展.

2. 削峰, 将一部分请求存下来慢慢处理. 缺点就是有延迟.

3. 中间态, 因为支付并不需要马上将买家的钱划到卖家账号上, 所以可以采用中间账号(担保账号)的方式.

#### 瓶颈

1.  单机TPS在1000左右

   2000000 = 单机容量 × 分库数量

   2000000 =          100 × 20000

   2000000 =        1000 × 2000

   2000000 =      10000 × 200

   2000000 =    100000 × 20

   - 一百万业务TPS等于两百万系统TPS(分布式事务的原因, 扣钱与加钱是两个事务)
   - 单机可支持的事务数有上限
   - 机器越多硬件出错可能越大

2. 难以处理超级热点账户(月底自动缴费水电气网话费等)

3. 靠牺牲业务来支撑峰值流量(当业务量上去后, 系统大部分时间处于峰值, 没办法削峰填谷)

## 百万TPS支付系统

为什么要用数据库?

**ACID**

A  原子性

C  一致性

I   隔离性

D 持久性

AIC 都是数据操作, D是数据存储