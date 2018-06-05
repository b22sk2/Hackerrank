# PUSH OFFER
## GENERAL DESCRIPTION
Push  has following options
1. *DISPLAY TEXT*
  Displays simple offer content with YES/NO options. There is an optional functionality in case of YES option is selected.
2. *GET INKEY*
   It displays offer content with built-in keyboard. Suitable for multi-offers or voting.
3. *SETUP CALL*
  It makes phone call to desired destination in behalf of customer. Asks customer permission each time.
## "PUSH OFFER" for 3rd party systems
  1. SENDING REQUEST USING HTTP POST
  2. REVEIVING USER RESPONSE USING RABBIT MQ
  
# API BODY
  Here OFFERID MUST BE UNIQ
## DISPLAY TEXT WITH SINGLE CONFIRMATION
  ```json
  {
  "msisdn": "89110437",
  "content": "this is test",
  "offerId": "test",
  "type": 1,
  "confirmation": false
}
  ```
## DISPLAY TEXT WITH DOUBLE CONFIRMATION
  field confirmation must be true
  ```json
  {
  "msisdn": "89110437",
  "content": "this is test",
  "offerId": "test",
  "type": 1,
  "confirmation": true
}
  ```
## GET INKEY
  ```json
{
  "msisdn": "89110437",
  "content": "test",
  "offerId": "test",
  "type": 2
}
  ```
## SETUP CALL
  ```json
{
  "msisdn": "89110437",
  "content": "test",
  "offerId": "test",
  "to":"9761414",
  "type": 3
}
  ```
## CONSUMING RABBIT MQ
RABBIT MQ Client maven 
```xml
        <dependency>
            <groupId>com.rabbitmq</groupId>
            <artifactId>amqp-client</artifactId>
            <version>3.6.6</version>
            <type>jar</type>
        </dependency>
```
MQ quue name is >>push.|OFFERID|>>

```java
import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Consumer;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;
import java.io.IOException;
 
public class Test {
    public static void main(String[] argv) throws Exception {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost(host);
        factory.setUsername(username);
        factory.setPassword(password);
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();
 
        Consumer consumer = new DefaultConsumer(channel) {
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body)
                    throws IOException {
                String message = new String(body, "UTF-8");
                System.out.println(message);
            }
        };
        channel.basicConsume(queueName, true, consumer);
    }
 
}
```
