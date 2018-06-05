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
