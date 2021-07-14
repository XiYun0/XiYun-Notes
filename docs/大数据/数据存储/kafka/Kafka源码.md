# Kafka源码

## 源码环境准备

> 这里使用的是2.1.0
>
> 注意，jdk需要10以上版本，8不行。

```
git clone https://gitee.com/jeffy-wang/kafka-2.1.0-src.git
```



## 从Demo入手

找到exmaple模块

Producer

```java
public void run() {
        int messageNo = 1;
        while (true) {
            String messageStr = "Message_" + messageNo;
            long startTime = System.currentTimeMillis();
            if (isAsync) { // Send asynchronously
                    // 异步发送, 返回Future<RecordMetadata>, 需要传入一个回调函数来确认是否发送成功
                    // 生产商一般使用异步发送
                    producer.send(new ProducerRecord<>(topic,
                        messageNo,
                        messageStr), new DemoCallBack(startTime, messageNo, messageStr));
            } else { // Send synchronously
                try {
                    // 同步发送, 返回RecordMetadata, 必须返回计算完的结果才会执行后面的语句(阻塞)
                    producer.send(new ProducerRecord<>(topic,
                        messageNo,
                        messageStr)).get();
                    System.out.println("Sent message: (" + messageNo + ", " + messageStr + ")");
                } catch (InterruptedException | ExecutionException e) {
                    e.printStackTrace();
                }
            }
            ++messageNo;
        }
    }
```

