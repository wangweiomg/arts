## ARTS - Tip

spark建hive表insert 时候的一些问题



使用pyspark 来创建hive表并insert时候，一直没遇到啥问题。最近在增加了几个字段，然后跑任务后，发现使用hive查数据dt过滤，新增的字段是NULL， 但是用sparksql查出来确实有值的。开发的回复是，都用hive sql来建表，sparksql建表有问题。至于是什么问题，暂时还不清楚，需要细查一下。
