# list列表处理

1、table 表格 ctrl+T 在淡出对话框中选择行数和列数，自动生成表格。

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

2、建立表格另一种方式，使用“|” 符号进行分割，然后回车创建表格，使用 ctrl+ Enter 新添加一行

| 表格1 | 表格2 | 表格3 |
| ----- | ----- | ----- |
|       |       |       |
|       |       |       |
|       |       |       |

# 时序图（Sequence）

Typora 时序图依托于js-swquence 来实现

demo1

```sequence
Alice->Bob: Hello boy ,how are you?
Note right of Bob:Boy thinks
Bob-->Alice: I am good thanks!

```

demo2

```sequence
Title:标题:复杂使用
对象A->对象B:对象B你好吗？（请求）
Note right of 对象B:对象B 的描述
Note left of 对象A:对对象A 的描述
对象B-->对象A:我好呢好（响应）
对象B->小三:你好吗？
小三-->>对象A:对象B找我了。
对象A->对象B:你真的好吗？
Note over 小三,对象B:我们是朋友
participant C
Note right of C:没人陪我玩
对象A->C:我来找你了(请求)
C-->对象A:我有男朋友
participant 男朋友
note right of 男朋友:我是C的男朋友
C->男朋友:对象A骚扰我
男朋友-->>对象A:小心我锤你

```



