# PUSH OFFER
## 1. GENERAL DESCRIPTION
Push  has following options
1.1 *DISPLAY TEXT*
  Displays simple offer content with YES/NO options. It sends sms to SERVER in case of YES/NO option is selected.It disapears from ME screen after 2 minutes and it re-display after each call disconnect for 2 hours.  
 1.2 *GET INKEY*
   It displays offer content with built-in keyboard. Suitable for multi-offers or voting. In case of user press OK  button it sends  sms to SERVER.
1.3 *SETUP CALL*
  It makes phone call to desired destination in behalf of customer. Asks customer permission each time.
## "PUSH OFFER" for 3rd party systems
  * SENDING REQUEST USING HTTP POST
  * REVEIVING USER RESPONSE USING RABBIT MQ
  
# API BODY
  Here OFFERID MUST BE UNIQUE
## DISPLAY TEXT WITH SINGLE CONFIRMATION TYPE=1
  ```json
  {
  "msisdn": "89110437",
  "content": "this is test",
  "offerId": "test",
  "type": 1,
  "confirmation": false
}
  ```
## DISPLAY TEXT WITH DOUBLE CONFIRMATION TYPE=1 , CONFIRMATION=TRUE
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
## GET INKEY TYPE=2
  ```json
{
  "msisdn": "89110437",
  "content": "test",
  "offerId": "test",
  "type": 2
}
  ```
## SETUP CALL TYPE=3 , TO=NUMBER_TO_CALL
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
MQ queue name is **push.|OFFERID|**

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
## API RESPONSE
IN CASE OF SUCCESS
```json 
{
"status": "SUCCESS",
"reason": null,
"requestType": 3
}
```
# USER RESPONSES
## 1.DISPLAY TEXT
### USER PRESSED YES BUTTON
```json
{"message":"00","reason":"YES","recievedDate":"20180531171801","offerId":"test_all211","status":"SUCCESS","msisdn":"89117511","requestType":1,"requestContent":"test ettests tas das asd  ","reportDate":"20180531171801"}
```
### USER PRESSED BACK BUTTON 
```josn
{"message":"11","reason":"BACK","recievedDate":"20180531171844","offerId":"test_all211","status":"SUCCESS","msisdn":"89117511","requestType":1,"requestContent":"test ettests tas das asd  ","reportDate":"20180531171844"}
```
### IF USER PRESSED CANCEL BUTTON
```json
{"message":"10","reason":"CANCEL","recievedDate":"20180523183655","offerId":"test_all211","status":"SUCCESS","msisdn":"89111103","requestType":1,"requestContent":"Yuch bitgii daraarai ","reportDate":"20180523183655"}
```
### TIME OUT OCCURED ON SERVER SIDE
```json
{"message":null,"reason":"time out","recievedDate":"20180530150811","offerId":"test_all211","status":"failed","msisdn":"89110437","requestType":1,"requestContent":null,"reportDate":null}
```
### IF SERVER RECEIVES SMS AFTER TIME OUT 
```json
{"message":"00","reason":"request_expired","recievedDate":"20180528113422","offerId":null,"status":"SUCCESS","msisdn":"88881005","requestType":0,"requestContent":null,"reportDate":"20180528113422"}
```
## 2.GET INKEY
  HERE message  field value is in HEXADECIMAL
 ```json
 {"message":"31","reason":"YES","recievedDate":"20180524092841","offerId":"test_all211","status":"SUCCESS","msisdn":"89110437","requestType":2,"requestContent":"Yuch bitgii daraarai ","reportDate":"20180524092841"}
 ```
## 3. SETUP CALL
  Do not send sms to SERVER in all  cases
