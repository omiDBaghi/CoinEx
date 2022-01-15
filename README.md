# digital currency exchange

This project is an exchange project, providing users with online virtual currency transactions, market viewing, automatic buying and sellin

database path /Database/yingbt.sql

## Directory Structure
    api WEB deployment directory
    ├─index.php 	main entry file
    ├─README.md 	README file
    ├─Application 	directory
    │ ├─Admin 		background module Responsible for background functions and operations
    │ ├─common 		Common function directory
    │ ├─Home 		Front-end module Responsible for user operations and functions
    │ ├─Mobile	 	terminal module Responsible for the display and processing of mobile phone access
    ├─Public 		resource file directory
    ├─Upload 		upload resource directory
    └─ThinkPHP 		framework directory
    └─run.php 		Perform data synchronization operations (virtual currency withdrawal, deposit and query, real transfer and transaction)

# other remarks

New fields need to be commented

# database table structure

tw_user_coin user wallet table

tw_coin system currency base table

tw_trade_json trade chart data (separated by time)

# Front-end API interface

### POST to get the API key according to /Ajax/getkey

request parameter
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 remarks | yes | string | remarks
 ip | no | string | limit ip


 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests
 SecretKey | yes | string | private key
 user_id | yes | int | user id
 remarks | yes | string | remarks
 ip | no | string | bind ip


 Return example:

{
"remarks": "test",
"ip": "114.25.12.2",
"user_id": "6277",
"AccessKey": "myiyiloxpfr51l7zxxa7jdrz5xy5m2zv",
"SecretKey": "rcc4wzbnxlxxolm57lx90746mp5sf4ti"
}


 -------------------------------------------------- ------

# User API interface

Be sure to set X-Requested-With: XMLHttpRequest in the Header


The user's API permissions are obtained from the website's Account->API Management. Click Create to get it.


### IMPORTANT: These two keys are closely related to account security and should not be disclosed to others at any time.


SignatureMethod The hash-based protocol for the user to calculate the signature, here HmacSHA256 is used


Signature example:

For example: In the API interface of canceling the order, sign the following parameters, assuming that the Secret Key is: gnlzv781mgzsvkywj6tkhn1ogfeycbuu
Be
AccessKey=wgpthr6mz7uvaf8y5tcwiququh54jbxd&id=3405&time=1538100485

Use HmacSHA256 algorithm and SecretKey as the secret key. Sign the string and perform md5 digital signature on the signed content to obtain the md5 value: 618ec861b1c3ff9a2fb2a9aec1ab7522

The final combined request parameters are as follows:

AccessKey=wgpthr6mz7uvaf8y5tcwiququh54jbxd&id=3405&time=1538100485&sign=618ec861b1c3ff9a2fb2a9aec1ab7522




### GET /api/accounts Get basic user information

request parameter
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests


 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 code | yes | int | 1 request succeeded -1 request failed
 msg | yes | string | the returned prompt message
 code | yes | string | verbose data returned
 id | yes | int | user id
 username | yes | string | username
 status | yes | int | 1 normal 0 frozen


 Return example:
 
{
"code": 1,
"msg": "Success",
"data": [{
"id": "6277",
"username": "abc@qq.com",
"status": "1"
}]
}



### GET /api/get_time to get the current timestamp

request parameters
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests

 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 code | yes | int | 1 request succeeded -1 request failed
 msg | yes | string | the returned prompt message
 data | yes | int | system timestamp (Dongba District)


 Return example:
 
{
"code": 1,
"msg": "Success",
"data": 1537344636
}


### GET /api/currencys Get supported currencies

request parameter
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests


 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 code | yes | int | 1 request succeeded -1 request failed
 msg | yes | string | the returned prompt message
 data | yes | string | supported currencies


 Return example:

{
"code": 1,
"msg": "Success",
"data": ["btc", "ltc", "bcc", "eth"]
}


### GET /api/balance Get user account transaction balance and frozen balance

request parameters
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests


 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 code | yes | int | 1 request succeeded -1 request failed
 msg | yes | string | the returned prompt message
 data | yes | string | return balance
 btc | no | double | bitcoin balance
 btcd | no | double | Bitcoin frozen amount
 eth | no | double | ether balance
 ethd | no | double | amount of ether frozen
 ltc | no | double | Litecoin balance
 ltcd | no | double | Litecoin frozen amount


 Return example:

{
"code": 1,
"msg": "Success",
"data": {
"btc": "999495.70961240",
"ltc": "975.00000000",
"eth": "0.00000000",
"btcd": "53.30785460",
"ltcd": "0.00000000",
"ethd": "0.00000000"
}
}

### POST /api/place for transaction

Be sure to set X-Requested-With: XMLHttpRequest in the Header

request parameters
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests
 price | yes | double | transaction price
 num | yes | double | number of transactions
 market | yes | string | transaction currency (ltc_btc or bcc_btc or ltc_eth)
 type | yes | int | transaction type 1 buy 2 sell
 time | yes | int | timestamp (East Eighth District time)
 sign | yes | string | signature

 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 into | yes | string | transaction information prompt
 status| yes | int | transaction status 1 success 0 failure
 url | no | string | other hints



 Return example:

{
"code": 1,
"msg": "Successful transaction!",
"data": ""
}


### POST /api/cancelOrder Cancel the order


request parameters
 
 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 AccessKey | yes | string | required key for API requests
 id | yes | int | order id
 time | yes | int | timestamp (Dongba District)
 sign | yes | string | signature

 Response result:

 Parameter Name | Required | Data Type | Description |
 ---- | ----- | ------ | ----
 into | yes | string | transaction letter
