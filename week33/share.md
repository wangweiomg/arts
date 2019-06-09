## ARTS - Share 补2019.2.20

## 关于APP后端的登陆状态

#### 问题

最近做APP的后端，考虑如何做登陆的状态，要实现的目标是区分用户，并且不用频繁重新登登录，不暴露系统用户的实际ID。

#### 思路

有这样一个思路，考虑用户表增加一个token列，当token存在，就说明已经登陆，不存在说明未登录。token的控制可以分为客户主动登出，或者超时退出(后端定时清理)。这样就不用暴露userId到外部，使用token传输放在header里，每次只要验证有没有这个参数，参数是否存在库里(加缓存)，就能判断登陆状态。

#### 生成规则

不使用UUID，因为它无序，我们需要能够按照时间有序生成，这里参考了下twitter开源的ID生成策略项目snowflake，支持分布式唯一ID。

#### 结构

0 - 0000000000 0000000000 0000000000 0000000000 0 - 00000 - 00000 - 000000000000

第一位为未使用，接下来的41位为毫秒级时间(41位的长度可以使用69年)，然后是5位datacenterId和5位workerId(10位的长度最多支持部署1024个节点） ，最后12位是毫秒内的计数（12位的计数顺序号支持每个节点每毫秒产生4096个ID序号）

一共加起来刚好64位，为一个Long型。(转换成字符串后长度最多19)

snowflake生成的ID整体上按照时间自增排序，并且整个分布式系统内不会产生ID碰撞（由datacenter和workerId作区分），并且效率较高。经测试snowflake每秒能够产生26万个ID。

#### 源码

```java


public class SnowFlakeIdWorker {

    /**
     * 开始时间截(2015-01-01)
     */
    private final long twepoch = 1420041600000L;


    /**
     * 机器ID所占位数
     */
    private final long workerIdBits = 5L;

    /**
     * 数据标识id所占位数
     */
    private final long datacenterIdBits = 5L;


    /**
     * 支持的最大机器ID，结果是31，
     */
    private final long maxWorkerId = -1L ^ (-1L << workerIdBits);

    /**
     * 支持的最大数据标识id，结果是31
     */
    private final long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

    /**
     * 序列在id中占的位数
     */
    private final long sequenceBits = 12L;

    /**
     * 机器ID向左移12位
     */
    private final long workerIdShift = sequenceBits;

    /**
     * 数据标识id向左移17位(12+5)
     */
    private final long datacenterIdShift = sequenceBits + workerIdBits;

    /**
     * 时间截向左移22位(5+5+12)
     */
    private final long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

    /**
     * 生成序列的掩码，这里为4095 (0b111111111111=0xfff=4095)
     */
    private final long sequenceMask = -1L ^ (-1L << sequenceBits);

    /**
     * 工作机器ID(0~31)
     */
    private long workerId;

    /**
     * 数据中心ID(0~31)
     */
    private long datacenterId;

    /**
     * 毫秒内序列(0~4095)
     */
    private long sequence = 0L;

    /**
     * 上次生成ID的时间截
     */
    private long lastTimestamp = -1L;

    //==============================Constructors=====================================

    public SnowFlakeIdWorker(long workerId, long datacenterId) {
        if (workerId > maxWorkerId || workerId < 0) {
            throw new IllegalArgumentException(String.format("worker Id can't be grater than %d or less than 0", maxWorkerId));
        }
        if (datacenterId > maxDatacenterId || datacenterId < 0) {
            throw new IllegalArgumentException(String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
        }
        this.workerId = workerId;
        this.datacenterId = datacenterId;
    }


    //==============================Methods==============================

    /**
     * 获得下个ID
     */
    public synchronized long nextId() {
        long timestamp = timeGen();

        // 如果当前时间小于上次ID生成的时间戳，说明系统时钟回退过，这时候抛出异常
        if (timestamp < lastTimestamp) {
            throw new RuntimeException(
                    String.format("Clock moved backwards.  Refusing to generate id for %d milliseconds", lastTimestamp - timestamp));
        }

        // 如果同一时间生成的，则进行毫秒内序列
        if (lastTimestamp == timestamp) {
            sequence = (sequence + 1) & sequenceMask;

            if (sequence == 0) {
                // 阻塞到下一毫秒，获得新的时间戳
                timestamp = tilNextMillis(lastTimestamp);
            }
        }

        // 时间戳改变，毫秒内序列重置
        else {
            sequence = 0L;
        }

        // 上次生成ID的时间戳
        lastTimestamp = timestamp;

        //移位并通过或运算拼到一起组成64位的ID
        return ((timestamp - twepoch) << timestampLeftShift)
                | (datacenterId << datacenterIdShift)
                | (workerId << workerIdShift)
                | sequence;

    }

    /**
     * 阻塞到下一个毫秒，直到获得新的时间戳
     * @param lastTimestamp
     * @return
     */
    private long tilNextMillis(long lastTimestamp) {
        long timestamp = timeGen();
        while (timestamp <= lastTimestamp) {
            timestamp = timeGen();
        }
        return timestamp;
    }

    /**
     * 获取当前时间，毫秒数
     *
     * @return
     */
    private long timeGen() {
        return System.currentTimeMillis();
    }


    /**
     * 测试
     */
    public static void main(String[] args) {

        SnowFlakeIdWorker worker = new SnowFlakeIdWorker(0, 0);
        for (int i = 0; i < 1000; i++) {
            long id = worker.nextId();
            System.out.println(Long.toBinaryString(id));
            System.out.println(id);

        }
    }
}

```



---

参考

[Twitter的分布式自增ID算法snowflake(Java版)](https://www.cnblogs.com/relucent/p/4955340.html)