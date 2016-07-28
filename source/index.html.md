---
title: Zinny API Documentation

language_tabs:
- shell
- ruby
- python
- javascript


toc_footers:
- <a href='#'>Zinny platform API documentation</a>

includes:
- errors

search: true
---

# Introduction

	Open API는 플랫폼의 백앤드 서비스를 호출할 수 있는 HTTP Interface를 제공합니다.
SDK를 사용하지 않는 게임 서버, 웹 기반 게임 클라이언트 등에서 Open API를 이용하여 플랫폼이 제공하는 기능을 사용할 수 있습니다.
Open API는 HTTP Secure와 앱별로 발급되는 인증키를 사용하여 높은 수준의 보안성을 제공합니다. 또한 내부적으로 fully asynchronous 구조를 사용하여 많은 트래픽을 효과적으로 처리합니다.

	Open API는 게임 서버나 클라이언트가 사용하는 `Service API`와 내부 서버 전용의 `Internal API`로 나누어 제공됩니다.
일부 Service API(header로 zat을 요구하는 API) 들은 내부적으로 인증 단계를 거친후 서비스를 제공합니다. zat은 항상 요청 시점에 SDK를 통해 획득한 최신값을 사용하여야 합니다.
모든 API는 POST Method로 호출되어야 합니다. (일부 API들은 GET Method도 허용합니다만 본 문서에서는 소개하지 않습니다.)


# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
-H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Open API

## accumulateScore (deprecated)

```ruby
Example Request
POST /service/accumulateScore HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json
[
{
    "leaderboardId":"gold",
    "delta":12
  }
]
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
}
```
### Description

리더보드 서버에 점수를 등록한다. 이미 기록된 점수가 있을 경우, 기존 점수에 합산하여 기록한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
delta | long | Y | 점수 증분

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## applyUnregisterFromApp (deprecated)

```ruby
Example Request

POST /service/applyUnregisterFromApp HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
playerId: 21792586.kakao.1
```
```json  
{
}
```

```ruby
Example Response

HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
}
```

### Description
게임에서 탈퇴를 신청한다.
이 API는 탈퇴 유예기간 처리를 위해 사용되며 호출시 실제 탈퇴가 이루어지는 것은 아니며 로그인만 차단한다. 탈퇴 신청된 사용자는 해당 앱에 다시 접속할 수 없다.
신청 일주일간 유예기간 이후 탈퇴라는 정책이 있다면 이 API를 호출하고 일주일 경과후 unregisterFromApp (deprecated) 를 호출하면 된다.
(참조: 2.4 게임 탈퇴 (old))


### METHOD

`POST`

### Request Headers

Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
playerId | String | Y | 플레이어 ID

### Request Body Parameters

none (Header만 사용)

## checkReservation 

```ruby
Example Request
POST /service/checkReservation HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "userKey":"8834533"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "appId":"",
    "playerId": "5858",
    "rewards": [ 1, 2 ]
}
```
### Description

사전 예약 대상자인지 확인하고 보상을 수령한다.
<aside class=“notice”>사전 예약 기능을 사용하기 위해서는 기술PM을 통해 사전 예약 대상자 목록을 등록하는 과정이 필요합니다.자세한 내용은 담당 기술PM에게 문의해주십시오.</aside>

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
userKey | String | Y | 사전예약에서 사용한 유저 키 (카카오 게임의 경우, Service User ID)

### Response Content (보상 받은 건이 있을때만 content가 존재하고 그렇지 않으면 200, OK만 반환)
Name | Type | Description
--------- | ------- | ------
appId | String | 게임 앱 ID
playerId | String | Player ID
rewards | List<Integer> | 보상 받은 사전이벤트 seq

## claimAttachedItems 

```ruby
Example Request
POST /service/claimAttachedItems HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "itemIds": [
            "8e76e0c2-f9a6-4e7f-8af0-ef311b7528d8"
   ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
   "items": [
                {
                    "appId": "125125",
                    "itemCode": "test",
                    "quantity": 10,
                    "itemId": "8e76e0c2-f9a6-4e7f-8af0-ef311b7528d8",
                    "validityTime": 2592000000
                }
            ]
}
```
### Description

아이템 ID 기반으로 아이템 수령 프로세스를 시작한다.
위 API 호출시 지급할 아이템 목록을 반환하고 지급중 상태로 변경되며 처리가 완료된 이후 `confirmAttachedItems` 를 호출해야 지급완료 상태가 되어 트랜잭션이 종료 된다.
만약 `confirmAttachedItems`를 호출하지 않으면 처리가 완료되지 않은 것으로 간주되어 동일한 요청이 한번 더 들어오더라도 목록을 반환한다.
위 API로 동일한 `itemId`에 대해서 3회 호출 시 에러 처리되며 처리 목록에서 제외된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
itemIds | List<String> | Y | 수령할 아이템 아이디 목록

### Response Content
Name | Type | Description
--------- | ------- | ------
appId | String | 게임 앱 ID
itemCode | String | 아이템 코드
quantity | Long | 아이템 수량
itemId | String | 아이템 UUID
validityTime | long | 아이템 유효기간 (ms)

## claimItems 

```ruby
Example Request
POST /service/claimItems HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "messageIds": [
      "3729694e-913b-4b29-94ef-36fd66f44ce8",
      "4234554e-783b-b4g4-3f43-nfc34987fn38"
   ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
   "results": [
                {
                    "messageId" : "3729694e-913b-4b29-94ef-36fd66f44ce8",
                    "status":200,
                    "items": [
                        {
                            "itemCode": "testItem111",
                            "quantity": 100,
                            "itemId": "3729694e-913b-4b29-94ef-36fd66f44ce8",
                            "validityTime": -1
                        }
                    ]
                },
                {
                    "messageId" : "4234554e-783b-b4g4-3f43-nfc34987fn38",
                    "status":406,
                    "items": []
                }
   ]
}
```
### Description

메시지에 포함된 아이템들을 수령 프로세스를 시작한다.
위 API 호출시 지급할 아이템 목록을 반환하고 지급중 상태로 변경되며 처리가 완료된 이후 confirmItems 를 호출해야 지급완료 상태가 되어 트랜잭션이 종료 된다.
만약 confirmItems를 호출하지 않으면 처리가 완료되지 않은 것으로 간주되어 동일한 요청이 한번 더 들어오더라도 목록을 반환한다.
위 API로 동일한 itemId에 대해서 3회 호출 시 에러 처리되며 처리 목록에서 제외된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageIds | List<String> | Y | 아이템을 수령할 메시지 ID 목록

### Response Content
Name | Type | Description
--------- | ------- | ------
messageId | String | messageId
status | Integer | status code
items | List<Item> | 아이템 정보 목록

### Item
Name | Type  | Description
--------- | -------- | ------------
appId | String | App ID
itemcode | String | 아이템 코드
quantity | Long | 아이템 수량 
itemId | String | 아이템 고유 ID
validityTime | Long | 유효기간

## confirmAttachedItems 

```ruby
Example Request
POST /service/confirmAttachedItems HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "itemIds": [
            "8e76e0c2-f9a6-4e7f-8af0-ef311b7528d8"
   ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description

아이템의 배송 트랜잭션을 종료 처리한다.
이 API가 호출 되어야 정상적으로 아이템 배송처리가 완료된 것으로 간주한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
itemIds | List<String> | Y | 수령할 아이템 아이디 목록

## confirmItems 

```ruby
Example Request
POST /service/confirmItems HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "messageIds": [
      "3729694e-913b-4b29-94ef-36fd66f44ce8",
      "4234554e-783b-b4g4-3f43-nfc34987fn38"        
   ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
   "results": [
      {
         "messageId" : "3729694e-913b-4b29-94ef-36fd66f44ce8",
         "status":200
      },
      {
         "messageId" : "4234554e-783b-b4g4-3f43-nfc34987fn38",
         "status":406
      }
   ]
}
```
### Description

메시지에 포함된 아이템들의 배송 트랜잭션을 종료 처리한다.
이 API가 호출 되어야 정상적으로 아이템 배송처리가 완료된 것으로 간주한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageIds | List<String> | Y | 수령할 아이템의 메시지 ID 목록

## createZinnyDeviceToken 

```ruby
Example Request
POST /service/createZinnyDeviceToken HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
        "appVer": "1.0.0",
        "sdkVer": "1.0.0",
        "os": "ios",
        "market": "appStore",
        "deviceId": "1234567890"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
 
{
        "accessToken": "7hAZ0miwJ6zIuovFw2JTLC4YmuLDW3mrNBXYHADdaxXT3HYxUm6WF0VFqfiYDHHnIXS01SpKLCR4G/6TBBuGoaiVQdT/zvfOmCQnZxXwmsuiV1VNCYDhUO7tAecePKyOmRLOBKT6nTqu8xS5vBrqg16pDbKECgcVWeoGh59RXYc=",
        "zdExpiryTime": 1436510283357
}
```
### Description
디바이스 인증 토큰을 생성한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key



### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appVer | String | Y | 게임 app version
sdkVer | String | Y | SDK version
os | String | Y | OS 종류 (ios/android)
market | String | Y | 마켓 코드 — appStore, googlePlay, tStore, ollehMarket, uPlus
deviceId | String | Y | 기기 고유 ID

### Response Content
Name | Type | Description 
--------- | ------- | ------ 
accessToken | String | 새로 발급한 인증 토큰
zdExpiryTime | Long | 인증 토큰 만료시간

## eventAppDefined 

```ruby
Example Request
POST /service/eventAppDefined HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "deviceId":"123",
    "type":"invite",
    "value":"10"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description
프로모션 서버에 앱에서 정의한 이벤트 발생을 보고한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
deviceId | String | Y | 디바이스 ID
type | String | Y | 이벤트 타입
value | String | Y | 이벤트 값


### Response Content
none (Status Code만 사용)

## eventLevelUp 

```ruby
Example Request
POST /service/eventLevelUp HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "deviceId":"123",
    "level":3
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description
유저의 레벨이 갱신될 경우, 프로모션 서버에 레벨업 이벤트 발생을 보고한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
deviceId | String | Y | 디바이스 ID
level | int | Y | 레벨 

### Response Content
none (Status Code만 사용)

## eventPurchase 

```ruby
Example Request
POST /service/eventPurchase HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "deviceId":"123",
    "itemCode":"gem100",
    "price":3000
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description
프로모션 서버에 구매 이벤트 발생을 보고한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
deviceId | String | Y | 디바이스 ID
itemCode | String | Y | 구매한 아이템 코드
price | int | Y | 지불 가격

### Response Content
none (Status Code만 사용)

## eventStageClear 

```ruby
Example Request
POST /service/eventStageClear HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "deviceId": "",
    "stage":3
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description
유저가 해당 스테이지를 첫번째 클리어 한 경우, 프로모션 서버에 스테이지 클리어 이벤트 발생을 보고한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
deviceId | String | Y | 디바이스 ID
stage | int | Y | 클리어한 스테이지 번호

### Response Content
none (Status Code만 사용)

## finishMessages 

```ruby
Example Request
POST /service/finishMessages HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "messageIds": [
      "3729694e-913b-4b29-94ef-36fd66f44ce8",
      "4234554e-783b-b4g4-3f43-nfc34987fn38"
   ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "results": [
        {
            "messageId" : "3729694e-913b-4b29-94ef-36fd66f44ce8",
            "status":200
        },
        {
            "messageId" : "4234554e-783b-b4g4-3f43-nfc34987fn38",
            "status":406
        }
    ]
}
```
### Description
메시지에 포함된 아이템의 배송 트랜잭션을 종료 처리(아이템 지급완료)하고 메시지를 삭제한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageIds | List<String> | Y | 완료 처리할 메시지 ID 목록

### Response Content
Name | Type | Description
--------- | ------- | ------
results | List<Result> | 아이템 정보 목록

## getPlayer 

```ruby
Example Request
POST /service/getPlayer HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 1824950129.zd3.1
```
```json  
{
        "fields": [
            "os",
            "country",
            "lang",
            "market",
            "phoneNumber",
            "lastAppId",
            "idpAlias",
            "customProperty",
            "appStatus",
            "status",
            "regTime",
            "modTime",
            "idpCode",
            "deviceLogin"
        ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8 
```
```json  
{
        "player": {
            "country": "kr",
            "os": "android",
            "idpId": "1824950129",
            "deviceLogin": true,
            "appStatus": "normal",
            "market": "googlePlay",
            "regTime": 1457931028180,
            "phoneNumber": null,
            "idpAlias": "test01:10EEA5DD57476D2EDC9035079EEF2E1EEBB3205399782E3C07883DF30CDFD1BC:028a1cf509548d53",
            "modTime": 1457931028301,
            "appId": "test01",
            "lang": "ko",
            "lastAppId": "test01",
            "customProperty": null,
            "status": "normal",
            "playerId": "1824950129.zd3.1",
            "idpCode": "zd3"
        }
}
```
### Description
플레이어 정보를 조회한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
fields | List<String> | Y | 조회 가능 항목 — os, country, lang, market, phoneNumber, lastAppId, idpAlias, customProperty, appStatus, status, regTime, modTime, idpCode, deviceLogin, idpPerson, kakaoUuid

### Response Content
Name | Type | Description
--------- | ------- | ------
player | player | 플레이어 정보

### Player
Name | Type | Description
--------- | ------- | ------
player | String | 플레이어 아이디
appId | String | 앱 아이디
idpCode | String | IDP 코드
idpAlias | String | IDP 계정 정보
regTime | Long | 등록 시각
modTime | Long | 마지막 수정 시각
lastAppId | String | 마지막 접속 앱 아이디
phoneNumber | String | 전화번호
customProperty | Map<String, String> | 커스텀 속성
status | String | Player의 상태 ( normal, restricted, removed )
appStatus | String | 앱 별 Player의 상태 ( normal, restricted, removed )
os | String |OS 정보
country | String |국가 코드
lang | String |언어 코드
market | String |마켓 정보
deviceLogin | Boolean |디바이스 로그인 여부
idpPerson | String |IDP 에서 제공해주는 IDP 사용자 구분 값
kakaoUuid | String |카카오에서 제공해주는 UUID — kakao2 IDPCode 에서 사용된다.

## getPlayersWithField 

```ruby
Example Request
POST /service/getPlayersWithField HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
        "searchField": "idpPerson",
        "searchValue": "88253716024128657.kakao",
        "fields": [
            "idpPerson",
            "kakaoUuid"
        ]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8 
```
```json  
{
            "players": [
                {
                    "appStatus": "normal",
                    "kakaoUuid": "1234567890",
                    "appId": "test01",
                    "playerId": "66409134.kakao2.1",
                    "idpPerson": "88253716024128657.kakao"
                },
                {
                    "appStatus": "normal",
                    "kakaoUuid": null,
                    "appId": "test01",
                    "playerId": "88253716024128657.kakao1.1",
                    "idpPerson": "88253716024128657.kakao"
                }
            ]
}
```
### Description
조회 조건으로 searchField와 `searchValue`를 지정하여 플레이어 정보를 조회한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
searchField | String | Y | 조회 조건 field name — deviceId, idpPerson, kakaoUuid
searchValue | String | Y | 조회 조건 field value
fields | List<String> | Y | 조회 가능 항목 — os, country, market, phoneNumber, lastAppId, idpAlias, customProperty, appStatus, status, regTime, modTime, idpCode, deviceLogin, idpPerson, kakaoUuid
counterIds | List<String> | N | 조회할 카운터 아이디 목록

### Response Content
Name | Type | Description
--------- | ------- | ------
player | player | 플레이어 정보

### Player
Name | Type | Description
--------- | ------- | ------
player | String | 플레이어 아이디
appId | String | 앱 아이디
idpCode | String | IDP 코드
idpAlias | String | IDP 계정 정보
regTime | Long | 등록 시각
modTime | Long | 마지막 수정 시각
lastAppId | String | 마지막 접속 앱 아이디
phoneNumber | String | 전화번호
customProperty | Map<String, String> | 커스텀 속성
status | String | Player의 상태 ( normal, restricted, removed )
appStatus | String | 앱 별 Player의 상태 ( normal, restricted, removed )
os | String |OS 정보
country | String |국가 코드
lang | String |언어 코드
market | String |마켓 정보
deviceLogin | Boolean |디바이스 로그인 여부
idpPerson | String |IDP 에서 제공해주는 IDP 사용자 구분 값
kakaoUuid | String |카카오에서 제공해주는 UUID — kakao2 IDPCode 에서 사용된다.

## getRankedScores (deprecated) 

```ruby
Example Request
POST /service/getRankedScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "leaderboardId":"gold",
    "fromRank":1,
    "toRank":10
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "scores": [
        {
            "score": 222,
            "rank": 1,
            "lastSeasonRank": 0,
            "lastSeasonScore": 0,
            "timestamp": 1461732215501,
            "player": {
                "playerId": "58",
                "name": "오빠",
                "customProperty": {
                    "grade": "VIP"
                },
                "appId": "test01"
            }
        }
    ],
    "seasonInfo": {
       "type": "DAILY",
       "resetHour": 12,
       "nextResetTime": 1461726000000,
       "seq": 16918
    },
    "desc": "테스트 리더보드",
    "leaderboardSize": 6
}
```
### Description
순위표를 얻는다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
fromRank | int | Y | 조회할 시작 등수 (1보다 크거나 같아야 함)
toRank	| int | Y | 조회할 마지막 등수 (fromRank보다 크거나 같고 1000보다 작거나 같아야 함)


### Response Content
Name | Type | Description
--------- | ------- | ------
scores | List<Player> | 순위 정보
seasonInfo | SeasonInfo | 현재 시즌 정보
desc | String | 리더보드 설명

### SeasonInfo
Name | Type | Description
--------- | ------- | ------
type | String | Daily, Weekly, Monthly, All
resetDay | int | type이 —> MONTHLY인 경우, reset 날짜 —> WEEKLY인 경우, reset 요일 (일 ~ 토 = 1 ~ 7)그렇지 않으면 반환하지 않음.
resetHour | int | type이 ALL이 아닐 경우, reset 시 (0 ~ 23)
nextResetTime | long | 다음 reset 시각
seq | int | 시즌 번호

### player
Name | Type | Description
--------- | ------- | ------
appId | String | 
playerId | String | 
name | String | 사용자명
customProperty | Map<String, String> |

## getReceivedMessages

```ruby
Example Request
POST /service/getReceivedMessages HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: player222
```
```json  
{
        "messageBoxId":"inBox",
        "count": 1
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
            "maxCount": 10,
            "totalCount": 3,
            "nextPageKey": 6,
            "messages": [
                {
                    "appId": "testApp1",
                    "senderId": "player111",
                    "receiverId": "player222",
                    "message": {
                        "deliverySeq": 7,
                        "messageId": "fcc794be-002b-4ec4-abce-b40407a95a93",
                        "messageBoxId": "inBox",                       
                        "title": "test message title",
                        "body": "test message body",
                        "resourceMap": {
                            "imageLink": "test://12221212"
                        },
                        "state": "read",
                        "regTime": 1427350711000,
                        "modTime": 1427350711000,
                        "readTime": 1427350711000,
                        "expiredTime": 1435126739000
                    },
                    "items": [
                        {
                            "itemId": "e3b35f9f-b556-4473-9aff-6232d9532f61",
                            "quantity": 100,
                            "itemCode": "testItem111",
                            "state": "confirmed",
                            "regTime": 1427350711000,
                            "modTime": 1427350711000,
                            "sentTime": 1427350711000,
                            "confirmedTime": 1427350711000,
                            "expiredTime": 1435126739000,
                            "validityTime": -1
                        }
                    ]
                }
            ]
}
```
### Description
메시지 배송 서버로 전달된 메시지 목록을 조회한다.
조회된 목록은 시간 역순으로 가장 최신 메시지가 첫페이지로 조회 된다.
메시지에는 복수개의 첨부아이템 목록이 존재한다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playId | String | Y | 플랫폼에서 발급한 사용자  ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageBoxId | String | Y | 메시지 박스 ID — 특정 메시지 박스에 대해서만 조회 시에 사용
cont | int | Y | 죄회할 메시지 개수 (최대 100개씩)
nextPageKey | Long | Y | 조회할 페이지의 시작 offset null 입력시 첫페이지를 요청함 Long Max (9223372036854775807) 값을 입력해도 동일한 첫페이지를 요청함
states | List<String> | N | 조회할 메시지 상태 값 목록 — unread, read, error, deleted, expired 미입력시 unread 상태의 메시지목록이 반환됨



### Response Content
Name | Type | Description
--------- | ------- | ------
nextPageKey | Long | 다음 페이지 요청시 사용될 nextPageKey — -1 이면 더이상 데이터가 없음
messages | List<MessagePacket> | 메시지 목록
maxCount | Integer | 최대 보유 가능 메시지 개수
totalCount	 | Integer | 현재 메시지 개수 (전체 페이지)

### MessagePacket
Name | Type | Description
--------- | ------- | ------
senderAppId | String | 발신자 App ID
senderId | String | 발신자 ID (playerId)
receiverAppId | String | 수신자 App ID
receiverId | String | 수신자 ID (playerId)
message | Message | 메시지 데이터
items | List<Item> | 첨부 아이템 목록 

### Message
Name | Type | Description
--------- | ------- | ------
deliverySeq|Long|메시지 데이터
messageId|String|메시지 ID
messageBoxId|String|메시지 Box ID
title|String|제목
body|String|본문
resourceMap|Map|메시지를 표현할때 필요한 데이터 값
state|String|메시지 상태 값 — unread, read, error, deleted, expired
regTime	|Long|등록 시각
modTime	|Long|최종 수정 시각
readTime | Long|확인 처리 시각 (읽음 처리)
expiredTime|Long|만료시각

### Item
Name | Type | Description
--------- | ------- | ------
messageId | String | 메시지 ID
itemId | String | 아이템 고유 ID
itemCode | String | 아이템 코드
quantity | Long | 아이템 수량
state | String	| 처리 상태값 — registered, sent, confirmed, error, deleted, expired
validityTime | Long | 아이템 유효시간
regTime | Long | 등록 시각
modTime | Long | 최종 수정 시각
sentTime | Long | requestItem 요청 시각
confirmedTime | Long | 확인 처리 시각
expiredTime | Long | 만료시각

## getRemovedPlayers 

```ruby
Example Request
POST /service/getRemovedPlayers HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
        "dayTimeSlot": 1468508400000,
        "appId": "test01",
        "offset": 0,
        "size": 10
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
        "players": [
            {
                "dayTimeSlot": 1468508400000,
                "playerId": "208226189.kakao2.3",
                "appId": "test01",
                "regTime": 1468565799875,
                "removedTime": 1468584464414
            }
        ],
        "hasNext": true
}
```
### Description

플랫폼 단에서 24시간 유예 기간을 탈퇴 처리한 플레이어 목록을 조회한다.
28일 전 데이터까지 조회할 수 있다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
dayTimeSlot | long | Y	 | 조회 대상 일자 — 한국 시간(UTC+9) 기준 0시 조회 대상일 epochTime milliseconds 설정 ex> 2016-07-18인 경우, 1468767600000
offset	| int | Y | 조회 시작 위치. 0부터 시작
size | int | Y | 조회 목록 크기, 최대 200

### Response Content
Name | Type | Description
--------- | ------- | ------ 
players | List<RemovedPlayer> | 탈퇴 처리 플레이어 목록
hasNext | Boolean | 다음 항목 존재 여부

### RemovesPlayer
Name | Type | Description
--------- | ------- | ------ 
playerId | String | 플레이어 아이디
regTime | Long| 가입 시각 / 가입 시점의 epochTime milliseconds
dayTimeSlot | Long | 탈퇴 요청일 / UTC+9 기준 탈퇴 요청일의 0시 epochTime milliseconds
appId | Long | 앱 아이디
removedTime | Long | 탈퇴 시각 / 탈퇴 시점의 epochTime milliseconds

## getScores (deprecated) 

```ruby
Example Request
POST /service/getScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json
{
    "leaderboardId": "test",
    "playerIds": ["58", "59", "111111"]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
      "scores": {
        "58": 123,
        "59": 1234
      },
      "seasonInfo": {
        "type": "DAILY",
        "resetHour": 12,
        "nextResetTime": 1461726000000,
        "seq": 16918
      },
      "ascending": false
}
```
### Description

요청한 사용자들의 점수를 한꺼번에 얻는다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
playerIds | List<String> | Y |점수를 얻을 사용자 목록

### Response Content
Name | Type | Description
--------- | ------- | ------ 
scores | Map<String, Long> | 점수 목록.
seasonInfo | SeasonInfo | 시즌 정보
ascending | boolean | 정렬 기준 (false일 경우, 점수가 클수록 높은 값)

### SeasonInfo
Name | Type | Description
--------- | ------- | ------ 
type | enum | 시즌 타입 (DAILY, WEEKLY, MONTHLY, ALL)
resetDay | int	 | 주간 랭킹일 경우 리셋 요일 (1~7), 월간 랭킹일 경우 리셋 날짜 (1~30), DAILY, ALL일 경우, 반환하지 않음.
resetHour | int	 | 시즌 랭킹일 경우, 리셋 시각 (0~23) — ALL일 경우, 반환하지 않음.
nextResetTime| | 시즌 랭킹일 경우, 다음번 리셋 시각 — ALL일 경우, 반환하지 않음.
seq | String |시즌 번호. ALL일 경우, 0

## getSeasonalRankedScores (deprecated)

```ruby
Example Request
POST /service/getSeasonalRankedScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json
{
    "leaderboardId": "test",
    "season": 16946
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
      "scores": [
        {
          "rank": 1,
          "value": 59,
          "playerId": "59"
        }
      ]
}
```
### Description

특정 시즌의 순위표 전체를 얻는다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
season | int | Y |0보다 크면, 시즌번호	0이면 현재 시즌	0보다 작으면 현재 시즌과의 차이. 예를들어, 주간랭킹에서 -1이면 지난주 랭킹

### Response Content
Name | Type | Description
--------- | ------- | ------ 
scores | List<Score> | 순위 정보


### Score
Name | Type | Description
--------- | ------- | ------ 
rank | int | 등수
value | long | 점수
playerId | String | 플레이어 ID

## leaderboard/v4/accumulateScore

```ruby
Example Request
POST /service/leaderboard/v4/accumulateScore HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "leaderboardId":"gold",
    "delta":12
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
  
{
}
```
### Description

리더보드 서버에 점수를 등록한다. 이미 기록된 점수가 있을 경우, 기존 점수에 합산하여 기록한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
delta | long | Y | 점수 증분

### Response Content
Name | Type | Description
--------- | ------- | ------ 

## leaderboard/v4/accumulateScore

```ruby
Example Request
POST /service/leaderboard/v4/getRank HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "leaderboardId":"gold"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
      "rank": 1,
      "score": 580,
      "cardinality": 2,
      "property": "오빠",
      "seasonSeq": 0
}
```
### Description

순위와 점수를 얻는다. (친구랭킹에서는 호출 불가)


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
seasonSeq | int | N | 시즌번호. 	0: 현재 시즌 (default) 양수: 지정된 시즌 음수: 지난 시즌. 예를들어, -1은 직전 시즌

### Response Content
Name | Type | Description
--------- | ------- | ------ 
rank | int | 순위
score | long | 점수
cardinality | int | 리더보드에 등록된 총 사용자 수
property | String | 사용자 속성
seasonSeq | int	 | 조회한 시즌 번호

## leaderboard/v4/getRankedScores

```ruby
Example Request
POST /service/leaderboard/v4/getRankedScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json 
{
    "leaderboardId":"gold",
    "fromRank":1,
    "toRank":10
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
      "seasonSeq": 0,
      "scores": [
        {
          "playerId": "58",
          "score": 580,
          "property": "오빠"
        },
        {
          "playerId": "61",
          "score": 610,
          "property": null
        }
      ]
}
```
### Description

순위표를 얻는다. (친구랭킹에서는 호출 불가)


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
fromRank | int | Y | 조회할 시작 등수 (1보다 크거나 같아야 함)
toRank | int | Y | 조회활 마지막 등수 (fromRank보다 크거나 같고 1000보다 작거나 같아야 함)
seasonSeq | int | N | 시즌번호. 	0: 현재 시즌 (default) 양수: 지정된 시즌 음수: 지난 시즌. 예를들어, -1은 직전 시즌

### Response Content
Name | Type | Description
--------- | ------- | ------ 
scores | List<Score> | 순위 정보. — fromRank부터 순서대로 반환한다.
seasonSeq | int	 | 조회한 시즌 번호

### Score
Name | Type | Description
--------- | ------- | ------ 
playerId | String | Player ID
score | String	 | 점수
property | String | 사용자 속성

## leaderboard/v4/getScores

```ruby
Example Request
POST /service/leaderboard/v4/getScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId": "test",
    "playerIds": ["58", "59", "111111"]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
      "seasonSeq": 17007,
      "scores": [
        {
          "playerId": "58",
          "score": 5800,
          "property": "오빠"
        },
        {
          "playerId": "59",
          "score": 5900,
          "property": null
        },
        {
          "playerId": "111111",
          "score": 0,
          "property": null
        },
      ]
}
```
### Description

요청한 사용자들의 점수를 한꺼번에 얻는다. (친구랭킹 전용)


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
seasonSeq | int | N | 시즌번호. 	0: 현재 시즌 (default) 양수: 지정된 시즌 음수: 지난 시즌. 예를들어, -1은 직전 시즌
playerIds | List<String> | Y | 점수를 얻을 사용자 목록

### Response Content
Name | Type | Description
--------- | ------- | ------ 
scores | List<Score> | 점수 목록. (getRankedScore 참조)
seasonSeq | int	 | 시즌 번호

## leaderboard/v4/getScoresWithoutProperty

```ruby
Example Request
POST /service/leaderboard/v4/getScores HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId": "test",
    "playerIds": ["58", "59", "111111"]
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
      "seasonSeq": 17007,
      "scores": [
        5800,
        5900,
        0
      ]
}
```
### Description

프로퍼티를 제외하고 점수 목록만 얻는다. `getScores`보다 효율적으로 동작한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID

### Response Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
seasonSeq | int | N | 시즌번호. 	0: 현재 시즌 (default) 양수: 지정된 시즌 음수: 지난 시즌. 예를들어, -1은 직전 시즌
playerIds | List<String> | Y | 점수를 얻을 사용자 목록

### Response Content
Name | Type | Description
--------- | ------- | ------ 
score | List<Long> | 점수 목록. (요청한 순서대로 반환)
seasonSeq | int	 | 시즌 번호

## leaderboard/v4/makeLeaderboard

```ruby
POST /service/leaderboard/v4/makeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId": "money",
    "desc": "테스트",
    "season": {
      "type": "WEEKLY",
      "resetDay": 2,
      "resetHour": 9
    },
    "sortingType": "DSC",
    "scoreLimit": 1000000,
    "recordType": "BEST",
    "reserveSeasons": 2,
    "regUser": "cantabile"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
}
```
### Description

리더보드를 생성한다.
<aside class=“notice”>일반적인 경우, 게임상에서 동적으로 리더보드를 생성해야 할 경우, 위 API를 사용하여 리더보드를 생성한다.</aside>

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 생성할 리더보드 ID
desc | String | N | 설명
season | TimeScope | N | 시즌 정보 (default = 전체 랭킹)
sortingType | String | N | ”DSC" : 내림차순 (높은 점수가 상위) — “ASC" : 올림차순 (낮은 점수가 상위) — “NONE" : default. 정렬안함 (친구 랭킹)
scoreLimit | Long | N | sortingType=DSC일 경우, 점수 상한값, sortingType=ASC일 경우, 점수 하한값, sortingType=NONE일 경우, 사용 안함 — 지정하지 않으면 제한 없음
recordType | String | N | ”BEST" : default. 가장 좋은 값일 경우만 갱신 — “RECENT": 최근값으로 항상 갱신
reserveSeasons | Integer | N | season=ALL이 아닐경우, 저장할 시즌.
예를들어, 주간랭킹에서 5일경우 과거 5시즌(5주간)의 값을 저장
default = 0 (과거 값을 저장하지 않음)
regUser | String | N | 	등록자

### Response Content
Name | Type | Description
--------- | ------- | ------ 
rank | int | 순위
score | long | 점수
cardinality | int | 리더보드에 등록된 총 사용자 수
property | String | 사용자 속성
seasonSeq | int	 | 조회한 시즌 번호

### TimeScope
Name | Type | Description
--------- | ------- | ------ 
type | String | Y | ”ALL" : 전체 랭킹 "DAILY" : 일간 랭킹 "WEEKLY" : 주간 랭킹 "MONTHLY" : 월간 랭킹
resetDay | int | N | 리셋 일자. (type = ALL, DAILY일 경우, 무시) type = WEEKLY일 경우, 리셋 요일. (일 = 1, 월 = 2, ... 토 = 7) type = MONTHLY일 경우, 리셋 날짜. (1~28 사이의 값)
resetHour | int	 | N | 리셋 시. (type = ALL일 경우, 무시)

## leaderboard/v4/removeLeaderboard

```ruby
Example Request
POST /service/leaderboard/v4/removeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId":"gold"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
}
```
### Description

리더보드를 제거한다.
<aside class=“notice”>일반적인 경우, 게임상에서 `makeLeaderboard`를 호출하여 동적으로 생성한 경우에만 위 API로 제거해야 한다.</aside>



### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 제거할 leaderboardId

### Response Content
Name | Type | Description
--------- | ------- | ------ 

## leaderboard/v4/reportScore

```ruby
Example Request
POST /service/leaderboard/v4/reportScore HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "leaderboardId":"gold",
    "score":123
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
}
```
### Description

리더보드 서버에 점수가 갱신되었음을 보고한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
score | long | Y | 점수

### Response Content
Name | Type | Description
--------- | ------- | ------ 

## loginTestPlayer

```ruby
Example Request
POST /service/loginTestPlayer HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
        "deviceId": "58",
        "serialNo": "5858",
        "accessToken": "58",
        "country": "kr",
        "lang": "ko",
        "market": "googlePlay",
        "appVer": "1.0.0",
        "sdkVer": "1.0.0",
        "os": "android",
        "osVer": "5.0",
        "telecom": "SKT",
        "deviceModel": "aaaa",
        "network": "3g",
        "loginType": "manual"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
            "zatExpiryTime": 1456380855638,
            "zat": "fwPla7fQ8ty9+DZT/lD//nPwfMyErsYRjcbdqKe6KVmAussTUqNRfqoN36zphweWpmc0tDALwQuBhd9NaxtctfpLeR2L7nYKcHixRTaSk+G/E4oa8Rn9QyNhGGOnU9kKjI2UenHk8UOCxRKkz3Z5pYuDKOLfmjEb0bR/6aMPAXpehSZDq8rZd8nNukfafsop/Et4LKt8SpM6Cj9u4C3VR18kuiBxsy1ToDDkIzDXL30=",
            "player": {
                "appId": "test01",
                "status": "normal",
                "idpId": "58",
                "playerId": "58.td.1",
                "idpAlias": "58.td.1"
            }
}
```
### Description
테스트 사용자로 로그인합니다. 만약 이전에 로그인한 적이 없다면 새로운 사용자를 생성합니다.
이 API를 통해 테스트용 사용자를 생성할 수 있습니다.

<aside class="notice"> **제한사항** Live 환경에서는 본 API 호출이 허용되지 않습니다.</aside>


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
deviceId | String | Y | 기기 고유 ID
serialNo | String | Y | 임의의 값 (상관 없음)
accessToken | String | Y | deviceId와 동일한 값
country | String | Y | 국가 코드 (2자리 소문자)
lang | String | Y | 언어 코드 (2자리 소문자)
market | String | Y | 마켓 코드 — appStore, googlePlay, tStore, ollehMarket, uPlus
appVer | String |  Y | 게임 app version
sdkVer | String| Y | SDK version
os |String | Y | OS 종류 ( ios/ android )
osVer | String | Y | OS 버전
telecom | String | Y |통신사
deviceModel | String | Y | 기기 모델명
network| String | Y| 네트워크 상태 (3g/ wifi/ 등등 ) 소문자
loginType | String | Y	|로그인 종류 (auto/ manual) -자동, 수동로그인 구분. 새로운 사용자 생성시에는 반드시 manual로 지정되어야 함.
referrer | String | N | 로그인 리퍼러 (초대공유, URL 프로모션에서 사용)
clientTime | Long | N | 호출시각 epoch time (milliseconds) — 지정되지 않으면 플랫폼 서버 시각 기준으로 세팅됨

### Response Content
Name | Type | Description
--------- | ------- | ------ 
zat | String | 인증토큰
zatExpiryTime | Long | 인증토큰 만료시각
player | Player	 | 생성된 유저

### Player
Name | Type | Description
--------- | ------- | ------ 
appId | String | App ID
playerId | String | Player ID
status | String	 | 플레이어의 상태값 — normal, restricted, removed
idpAlias | String | IDP 계정 정보 (Player ID와 동일)
idpId |String | IDP ID (Device ID와 동일)

## makeLeaderboard (deprecated)

```ruby
Example Request
POST /service/makeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId": "gold",
    "desc": "테스트 리더보드",
    "seasonType": "w",
    "resetDay": 1,
    "resetHour": 5
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
}
```
### Description

리더보드를 생성한다.
<aside class=“notice”>일반적인 경우, 게임상에서 동적으로 리더보드를 생성해야 할 경우, 위 API를 사용하여 리더보드를 생성한다.</aside>



### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId |String | Y | 생성할 리더보드 ID
desc | String | N | 설명
seasonType | String | Y | ”d”: 일간랭킹  "w": 주간랭킹  "m": 월간랭킹  "a": 전기간
resetDay | Integer | N	 |주간, 월간랭킹일 경우 필수  주간랭킹: 일~토 = 1~7  월간랭킹: 1~30
resetHour | Integer | N | ~23 사이의 값.  default = 0
ascending | Boolean | N | true: 오름차순  false: 내림차순  default = false 
forcedUpdate | Boolean | N | true:  최근점수  false: 최고점수  default = false
limit | Long | N | 오름차순일 경우, 최소 한계값  내림차순일 경우, 최고 한계값  한계값을 벗어나는 경우, 무시됨.
regUser | String | N | 등록자

### Response Content
Name | Type | Description
--------- | ------- | ------ 

## makeRelation

```ruby
Example Request
POST /service/makeRelation HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
    "objectPlayerId": "5858",
    "relationType": "BLOCK"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
}
```
### Description

상대방과의 관계를 맺는다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
objectPlayerId | String | Y | 상대방의 Player ID
relationType | String | Y | *”NO_RELATION" *"FOLLOWER" *"FOLLOWEE" *"FRIEND” *"BLOCK"

## markMessagesAsRead
```ruby
Example Request
POST /service/markMessagesAsRead HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
   "messageIds": [
      "3729694e-913b-4b29-94ef-36fd66f44ce8",
      "4234554e-783b-b4g4-3f43-nfc34987fn38"
   ]
}
```
> The above command returns JSON structured like this:

```ruby
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "results": [
        {
            "messageId" : "3729694e-913b-4b29-94ef-36fd66f44ce8",
            "status":200
        },
        {
            "messageId" : "4234554e-783b-b4g4-3f43-nfc34987fn38",
            "status":406
        }
    ]
}
```
### Description

메시지를 읽음 처리한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageIds | List<String> | Y | 읽음 처리할 메시지 ID 목록


### Response Content
Name | Type | Description
--------- | ------- | ------ 
results | List<Result> | 아이템 정보 목록

### Result Content
Name | Type | Description
--------- | ------- | ------ 
messageId | String | messageId
status | Integer | 처리 결과 ( 200 성공 )

## mok/player/notification/send

```ruby
Example Request
POST /service/leaderboard/v4/getRank HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
    "leaderboardId":"gold"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json 
{
      "rank": 1,
      "score": 580,
      "cardinality": 2,
      "property": "오빠",
      "seasonSeq": 0
}
```
### Description

개별 플레이어에게 알림 메시지를 전달한다.
 
플레이어가 온라인상태일때는 세션을 통해서 클라이언 핸들러로 dataMap 이 전달되며 플레이어가 오프라인상태일 경우는 title과 body 내용이 GCM과 APNS를 통해서 push notification 형태로 전달된다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
title | String | Y | 오프라인 푸시 제목
body | String | Y | 오프라인 푸시 본문
sendingOfflinePush |Boolean | N | 오프라인 푸시 발송 여부 *기본값 : true
dataMap | Map | Y | 온라인 알림 데이터

### Response Content
Name | Type | Description
--------- | ------- | ------ 
onlinePlayerIds | List<String>	 | 온라인 푸시 발송 명단
offlinePlayerIds | List<String> | 오프라인 푸시 발송 명단

## mok/world/getPlayers

```ruby
Example Request
POST /service/mok/world/getPlayers HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "worldId": "WORLD-001"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
      "players": [
        "1318862090.zd3.1"
      ]
}
```
### Description

월드맵 데이터를 subscribe 중인 플레이어 목록을 반환한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
worldId | String | Y | 월드맵  ID

### Response Content
Name | Type | Description
--------- | ------- | ------ 
players | List<String>	 | 월드맵 데이터를 subscribe 중인 플레이어 ID 명단

## mok/world/publish

```ruby
Example Request
POST /service/mok/world/publish HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "worldId": "world-1",
    "dataMap": {
      "destination": "W",
      "type": "M",
      "cmd": "C",
      "msg": {
        "mapo_pk": "abc",
        "table_num": "1234"
      }
    }
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
	"count": 10
}
```
### Description

월드맵 채널에 부대이동 등 데이터를 publish한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key


### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
worldId | String | Y | 월드맵  ID
dataMap | Map | Y | 데이터

### Response Content
Name | Type | Description
--------- | ------- | ------ 
count | int  | 전송한 클라이언트 수 *세션이 전송하는 타임에 끊어질수 있어서 완벽하게 일치하지는 않음

## refreshZinnyAccessToken

```ruby
Example Request
POST /service/refreshZinnyAccessToken HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
zat: CTUMCKKSdzX_t7-h83LK4XUuctmutgvoHaNGyqwQQjQAAAFOJAGGzw
```
```json  
{
        "appVer": "1.0.0",
        "sdkVer": "1.0.0",
        "os": "ios",
        "market": "appStore",
        "deviceId": "1234567890"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
	"zat":"7hAZ0miwJ6zIuovFw2JTLC4YmuLDW3mrNBXYHADdaxXT3HYxUm6WF0VFqfiYDHHnIXS01SpKLCR4G/6TBBuGoaiVQdT/zvfOmCQnZxXwmsuiV1VNCYDhUO7tAecePKyOmRLOBKT6nTqu8xS5vBrqg16pDbKECgcVWeoGh59RXYc=",
	"zatExpiryTime": 1436510283357
}
```
### Description

인증 토큰을 갱신한다.

Zinny SDK를 적용한 게임이라면 인증토큰이 필요한 경우, 항상 클라이언트를 통해 인증토큰을 얻어와야하기 때문에 이 API는 서버에서 호출할 필요가 없다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플랫폼에서 발급한 사용자 ID
zat | String | Y | 인증을 확인하기 위한 token

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appVer | String| Y |게임 app version
sdkVer |String |Y |SDK version
os |String |Y |OS 종류 ( ios/ android )
market |String |Y |마켓 코드  *appStore *googlePlay *tStore *ollehMarket, *uPlus
deviceId |String |Y |기기 고유 ID

### Response Content
Name | Type | Description
--------- | ------- | ------ 
zat | String | Zinny Access Token
zatExpiryTime | Long | Zinny Access Token 만료시각

## refreshZinnyDeviceToken

```ruby
Example Request
POST /service/refreshZinnyDeviceToken HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
        "appVer": "1.0.0",
        "sdkVer": "1.0.0",
        "os": "ios",
        "market": "appStore",
        "deviceId": "1234567890",
        "accessToken": "7hAZ0miwJ6zIuovFw2JTLJPCHAESK3bKKiPLjXAacMPYMYlcpvXHM9CHpLv38ww8Dsz5LjiadI6qfOYXWuDSIumESp5sHiy/gf6P2AQV0/usBRi6a/K3jGLrQoxy17UXYmDCvL4Yrj4AYSEwU8kjS+2wpHVY7Dvbnr5WO0/1Tg0="
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
      	"accessToken":"7hAZ0miwJ6zIuovFw2JTLC4YmuLDW3mrNBXYHADdaxXT3HYxUm6WF0VFqfiYDHHnIXS01SpKLCR4G/6TBBuGoaiVQdT/zvfOmCQnZxXwmsuiV1VNCYDhUO7tAecePKyOmRLOBKT6nTqu8xS5vBrqg16pDbKECgcVWeoGh59RXYc=",
	"zdExpiryTime": 1436510283357
}
```
### Description

디바이스 인증 토큰을 갱신한다.

Zinny SDK를 적용한 게임이라면 인증토큰이 필요한 경우, 항상 클라이언트를 통해 인증토큰을 얻어와야하기 때문에 이 API는 서버에서 호출할 필요가 없다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appVer | String| Y |게임 app version
sdkVer |String |Y |SDK version
os |String |Y |OS 종류 ( ios/ android )
market |String |Y |마켓 코드  — appStore, googlePlay, tStore, ollehMarket, uPlus
deviceId |String |Y |기기 고유 ID
accessToken | String | Y | 기존에 발급받은 유효한 인증 토큰


### Response Content
Name | Type | Description
--------- | ------- | ------ 
accessToken | String | 새로 발급한 인증 토큰
zatExpiryTime | Long | Zinny Access Token 만료시각

## removeLeaderboard (deprecated)

```ruby
Example Request
POST /service/removeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "leaderboardId":"gold"
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{

}
```
### Description

리더보드를 제거한다.
<aside class="notice"> 일반적인 경우, 게임상에서 makeLeaderboard를 호출하여 동적으로 생성한 경우에만 위 API로 제거해야 한다.</aside>

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 제거할 leaderboardId

### Response Content
Name | Type | Description
--------- | ------- | ------ 
accessToken | String | 새로 발급한 인증 토큰
zatExpiryTime | Long | Zinny Access Token 만료시각

## removePlayer

```ruby
Example Request
POST /service/removeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
	"idpUnlink": true
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{

}
```
### Description

게임에서 탈퇴한다.
탈퇴된 사용자의 해당 앱에 대한 정보는 즉시 삭제되며 복구할 수 없다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
playerId | String | Y	 | 플랫폼에서 발급한 사용자 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
idpUnlink | Boolean | Y | IDP 탈퇴 수행여부

### Response Content
none(Status code만 사용)

## reportScore (deprecated)

```ruby
Example Request
POST /service/removeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
	"leaderboardId":"gold",
	"score":123
}
```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{

}
```
### Description
리더보드 서버에 점수가 갱신되었음을 보고한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
playerId | String | Y	 | 플랫폼에서 발급한 사용자 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
leaderboardId | String | Y | 등록한 leaderboardId
score | long | Y | 점수 

### Response Content
none(Status code만 사용)

## sendAllOnlinePush

```ruby
Example Request
POST /service/removeLeaderboard HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
	"body" : {
		"notice": "5분 후에 서버 점검이 시작됩니다."
	}
}
```
> Client로 전달되는 Packet:

```json  
[
    "push://v2/online/onMessage",
    {
        "txNo":12345689,
        "sessionId":"192.0.168.123:1232/14982480922"
    },
    { 
        "notice": "5분 후에 서버 점검이 시작됩니다."
    }
]
```
### Description
세션 서버에 현재 접속중인 전체 Client에게 요청한 메시지를 전달한다. 
body 내용은 그대로 클라이언트로 전달된다

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
body | String | Y | 클라이언트에 전달될 data

### Response Content
none(Status code만 사용)

## sendConditionalPush

```ruby
Example Request

POST /service/sendConditionalPush HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "playerIds": [
        "150537458.zd2.1"
    ],
    "onlineMessage": {
        "body": {
            "data": "1234"
        }
    },
    "offlineMessage": {
        "titleMap": {
            "ko": "테스트",
            "en": "test"
        },
        "bodyMap": {
            "ko": "테스트",
            "en": "test"
        }
    }
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "offlinePlayerIds": [
        "150537458.zd2.1"
    ],
    "onlinePlayerIds": [ ]
}
```
### Description
Player의 온라인 상태 여부에 따라 온라인 푸시 또는 오프라인 푸시를 발송한다.

온라인푸시는 요청한 Player ID 목록에 대해서 세션 서버에 현재 접속중인 Client에게 요청한 메시지를 전달한다. body 내용은 Object 형태로 입력 그대로 전달된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
playerIds | List<String> | Y | Player ID목록
onlineMessage | OnlineMessage | Y | online 푸시 메시지
offlineMessage | OfflineMessage | Y | offline 푸시 메시지

### OnlineMessage Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
body | Object | Y | client SDK로 전달되는 body

### OfflineMessage Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
titleMap | Map<String, String> | Y | 언어별 푸시 제목
bodyMap | Map<String, String> | Y | 언어별 푸시 본문
ticker | String	 | N | Ticker (안드로이드 전용)
theme | String | N | 테마 (기본값 : white)
pushType | String | N	 | 푸시 메시지의 내용을 구분하는 값 (공지, 이벤트 알림, 등등) — 기본값 : notice
badge | String | N | 뱃지 카운트
vibrate | String | N | 진동 패턴 (예: 1000,200,1000,200) android 의 진동 패턴 형식 — 미입력시 기본값 : 1000,200,1000,200
smallIcon | String | N	 | 푸시 알림시 아이콘 이미지 — 미입력시 기본값 : ic_noti
sound | String | N | 푸시 알림음 리소스 이름 — 미입력시 기본값 : push
link | String | N | 클릭시 사용한 링크 주소정보
bigText | String | N | bigtext type의 본문
bigImageUrl | String | N | bigtext type의 이미지 주소정보

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
offlinePlayerIds | List<String>  | offline 푸시를 발송한 Player ID목록 
olinePlayerIds | List<String> | online 푸시를 발송한 Player ID목록


## sendMessage

```ruby
Example Request

POST /service/sendMessage HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
```
```json  
{
        "receiverId": "player222",
        "message": {
            "messageBoxId": "inbox",           
            "title": "test message title",
            "body": "test message body",
            "resourceMap": {
                "imageLink": "test://12221212"
            }
        },
        "items": [
            {
                "itemCode": "testItem111",
                "quantity": 100,
                "validityTime": 3600000        
            }
        ]
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"messageId": "c21c6dfe-c13b-46ff-9946-961d28beb914"
}
```
### Description

메시지 배송 서버로 다른 유저에게 메시지를 전달 요청을 한다.
첨부는 item만 등록가능하다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플랫폼에서 발급한 사용자 ID (아이템 발신자)

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
receiverId | String | Y | 수신자 ID
message | Message | Y	 | 메시지
items | List<Item> | N	 | 아이템

### Message
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
messageBoxId | String | Y | 메시지 Box ID
title| String | N | 제목
body | String | N | 본문
resourceMap | Map | N	 | 메시지를 표현할때 필요한 데이터 값 — 제한 사항 *1차원 Key - Value *10개의 Key *각 각의 Value Size 100Byte
expiryTime | Long | N | 만료시각. 지정될 경우, 실제적으로 수령인이 처리할 시간이 필요하기 때문에 등록시점보다 최소 10초 이후의 시각이어야 한다.

### Item
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
itemCode | String | Y | 아이템 코드
quantity | Long | Y | 아이템 수량
validityTime | Long | N | 유효기간  — 미입력시 -1 입력됨 (무제한)

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
messageId | String | 전달되는 메시지의 messageId

## sendOnlinePush

```ruby
Example Request
POST /service/sendOnlinePush HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json 
{
        "playerIds":[
            "12398723.id"
        ],
        "body" : {
            "uri":"achievement://v1/achievement/getRewards",                  
            "rewardsMap": {
                "CWC_ACHV01":[
                    {
                        "masteryGrade": 1,
                        "regTime": 1421893909000
                    },
                    {
                        "masteryGrade": 2,
                        "regTime": 1421893912000
                    }
                ]
            }
        }
}
```
> Client로 전달되는 Packet:

```json
[
    "push://v2/online/onMessage",
    {
        "txNo":12345689,
        "sessionId":"192.0.168.123:1232/14982480922"
    },
    { 
        "uri":"achievement://v1/achievement/getRewards",
        "rewardsMap": {
            "CWC_ACHV01":[
                {
                    "masteryGrade": 1,
                    "regTime": 1421893909000
                },
                {
                    "masteryGrade": 2,
                    "regTime": 1421893912000
                }
            ]
        }
    }
]
```
### Description

목록중 세션 서버에 접속중인 Client에게 메시지를 전달한다.
body 내용은 그대로 클라이언트로 전달된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
playerIds | List<String> | Y	 | 메시지를 보낼 플레이어 ID 목록
body | String | Y | 클라이언트에 전달될 data


### Response Content
Name | Type  | Description
--------- | ------- | ------ 

## sendPush

```ruby
Example Request
POST /service/sendPush HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
    "titleMap":{"ko":"테스트 title"},
    "bodyMap":{"ko":"테스트 body"},
    "ticker":"ticker",
    "playerIds":[
        "125125",
        "232323"
    ]
}

```
> The above command returns JSON structured like this:

```ruby
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "messageId":"90ecf403-2a6e-4e9e-8538-9454695ab5d9"
}
```
### Description

사용자들에게 푸시 메시지를 발송한다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
badge	Integer	N	뱃지 카운트
bigImageUrl	String	N	bigtext type의 이미지 주소정보
bigText	String	N	bigtext type의 본문
bodyMap	Map<String,String>	Y	메시지 본문 (언어별 지원. "ko"는 반드시 존재해야 함.)
link	String	N	클릭시 사용한 링크 주소정보
os	String	N	푸시 토큰을 이용해서 발송시에 입력되어야함 (android, ios)
playerIds	List<String>	N	플레이어 ID 목록. 플레이어 선택 발송시에 입력 되어야함. (최대 1,000명)
pushType	String	N	
푸시 메시지의 내용을 구분하는 값 (공지, 이벤트 알림, 등등)
기본값 : notice
senderGroupId	String	N	전송 요청 그룹 ID ( 예시 client / game server / admin )
sendReport	Boolean	N	
BI 데이터를 위한 리포팅이 필요한지에 대한 여부 (y/n)
y 일때 BI 로그 전송 주소를 알려줌
smallIcon	String	N	
푸시 알림시 아이콘 이미지
미입력시 기본값 : small_icon
sound	String	N	
푸시 알림음 리소스 이름
미입력시 기본값 : push
targetMarkets	Set<String>	N	푸시 발송 대상 마켓. 지정하지 않으면 전체
ticker	String	N	ticker문구. 미입력시 contentTitle이 사용됨
titleMap	Map<String,String>	Y	메시지 타이틀 (언어별 지원)
vibrate	String	N	
진동 패턴 (예: 1000,200,1000,200) android 의 진동 패턴 형식
미입력시 기본값 : 1000,200,1000,200

### Response Content
Name | Type  | Description
--------- | ------- | ------ 

## setReferrer

```ruby
Example Request
POST /service/setReferrer HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "parent": "5858.facebook.1"
}
```
### Description

초대/공유 조건 완료를 보고한다.

공유링크를 통해 앱을 설치한 경우, 이 API가 호출되어야 비로소 초대/공유가 완료되고 social tree에도 관계가 등록된다. (social tree에 등록시 설정에 따라 초대자/가입자에게 각각 보상이 지급) 

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플레이어 ID

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
referrer | String | N	 | 초대/공유 코드. *Android : 플랫폼에서 자동으로 감지하여 등록한다. *iOS : 사용자가 입력한 값을 사용한다.

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
parent | String | 등록 성공한 공유자의 playerId

## unregisterFromApp (deprecated)

```ruby
Example Request
POST /service/setReferrer HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
playerId: 21792586.kakao.1
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
}
```
### Description

게임에서 탈퇴한다.
탈퇴된 사용자의 해당앱에 대한 정보는 즉시 삭제되며 복구할 수 없다.

<aside class="notice"> 탈퇴후 재가입 시 PlayerID: 인증의 종류에 따라 달라질 수 있습니다.
예를들어, KAKAO v1의 경우, 탈퇴후 동일한 카카오 계정으로 로그인하게 되면 동일한 PlayerID를 부여받게 되지만 KAKAO v2의 경우, 카카오에서 다른 ID를 부여하기 때문에 플랫폼에서 부여하는 PlayerID도 다른 값을 갖게됩니다.
현재 플랫폼에서 지원하고 있는 인증 중에 PlayerID가 달라지는 인증은 KAKAO v2, Facebook 인증이며 나머지는 탈퇴후 재가입시 동일한 PlayerID가 부여됩니다.
</aside>

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플랫폼에서 발급한 사용자 ID

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------


### Response Content
Name | Type  | Description
--------- | ------- | ------ 
parent | String | 등록 성공한 공유자의 playerId

## validateZinnyAccessToken

```ruby
Example Request
POST /service/setReferrer HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
playerId: 21792586.kakao.1
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
	"lang": "ko",
	"market": "googlePlay",
	"country": "kr"
}
```
### Description

인증토큰(zat)이 유효한지 체크한다.
Status Code가 200일 경우, 인증된 것으로 처리하면 된다. 

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerId | String | Y | 플랫폼에서 발급한 사용자 ID
zat | String | Y | 인증을 확인하기 위한 token

### Request Body Parameters
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------


### Response Content
Name | Type  | Description
--------- | ------- | ------ 
lang | String | 사용자의 언어코드
country | String | 사용자의 국가코드
market| String | 마켓

## writeBasicLog (deprecated) 

```ruby
Example Request
POST /service/writeBasicLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
```
```json  
{
   "code": "PLAY_RESULT",
   "logBody": {
      "result":"W",
      "earn":1200
   }
}
```
> The above command returns JSON structured like this:

```ruby
Example Response 
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

게임 로그를 남긴다. (비 플레이어 기반)


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key

### Request Body Parameters
Name | Type | Mandatory | Description 
--------- | ------- | ------ | ------------  
code | String | Y | 게임 혹은 플랫폼에서 지정한 로그 코드.
logBody | Map<String, Object>	 | Y | 로그 내용
ttl | long | N | 유효시간 (ms). 지정된 시간이 지나면 자동 삭제.
tag1 | String | N | 태그1
tag2 | String | N | 태그2
src | String | N | 로그 발생원 정보

### Response Content
Name | Type  | Description
--------- | ------- | ------ 

## writeItemLog  

```ruby
Example Request
POST /service/writeItemLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
        "itemType": "grip",
        "itemId": "superultragrip",
        "itemAttr1": "lv4",
        "itemAttr2": "*",
        "quantity": 1,
        "rCurrency": "gold",
        "cost": 1,
        "reason": "buy",
        "subReason": "*"
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

아이템 변동 로그를 기록한다. 
게임 내 상태의 변화(강화, 합성, 삭제  등)나 속성/유형에 따른 분류/관리가 필요한 경우 사용한다.
아이템 변동 로그는 PlayerLog 의 일부이며 (src, code) 로 (g1, item) 를 사용한다.
아이템 변동 로그는 60일 후 자동 삭제 된다.


### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerdId | String | Y | 플레이어 ID

### Request Body Parameters
Name | Type | Mandatory | Description | Available Values
--------- | ------- | ------ | ------------ | ------------ 
grade | Long | N | 플레이어 등급 | 
playerLv | Long | N | 플레이어 레벨 | 	 
itemType | String| N | 아이템 유형 | ex> 그립, 라켓, 스트링, 선수, 드레스, 재료
itemId | String | Y | 아이템 아이디 | 	 
itemAttr1 | String | N | 아이템 추가 속성 1st | 
itemAttr2 | String | N | 아이템 추가 속성 2nd | 
quantity | Long	| Y | 아이템 수량 | 
rCurrency | String | Y	| 아이템 변동 비용에 리소스 화폐 단위 | ex> GOLD, RUBY, TP, EVENT, ADMIN
cost | Long | Y	 | 아이템 변동에 따른 비용 |
reason | String| Y | 아이템 변동 사유 | ex> buy, exchange, get, gift, tuning, broken, make, admin
subReason | String | N | 아이템 변동 상세 사유 | ex> 선수 강화 성공/실패, 장비 튜닝 성공/실패

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
logId | String | 기록된 로그에 부여된 ID. 조회시에 nextPageKey 로 사용될 수 있다.

##writePlayerLog (deprecated)  

```ruby
Example Request
POST /service/writePlayerLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
   "code": "PLAY_RESULT",
   "logBody": {
      "result":"W",
      "earn":1200
   }
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
   "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

게임 로그를 남긴다. (플레이어 기반)



### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerdId | String | Y | 플레이어 ID

### Request Body Parameters

Name | Type | Mandatory | Description
--------- | ------- | ------ | ------ 
code | String | Y | 게임 혹은 플랫폼에서 지정한 로그 코드.
logBod | Map<String, Object> | Y | 로그 내용
ttl | long | N | 유효시간 (ms). 지정된 시간이 지나면 자동 삭제.
tag1 | String | N | 태그1
tag2 | String | N | 태그2
src | String | N | 로그 발생원 정보

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
logId | String | 기록된 로그에 부여된 ID. 조회시에 nextPageKey 로 사용될 수 있다.

## writePurchaseLog   

```ruby
Example Request
POST /service/writePurchaseLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 21792586.kakao.1
zat: CTUMCKKSdzX_t7-h83LK4XUuctmutgvoHaNGyqwQQjQAAAFOJAGGzw
```
```json  
{
    "grade": 100,
    "currency": "USD",
    "price": 12.23, 
    "os": "android",
    "market": "googlePlay",
    "marketOrderId": "123098120938.123123123",
    "marketProductId": "test gem 0003",
    "marketPurchaseTime": 12312312333
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

구매 로그를 기록한다.
유저가 게임에서 구매를 한 경우 호출한다.
구매 로그는 PlayerLog 의 일부이며 (src, code) 로 (p1, purchase) 를 사용한다.
구매 로그에 대해서는 유효 시간에 따른 자동 삭제가 일어나지 않는다.



### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerdId | String | Y | 플레이어 ID

### Request Body Parameters

Name | Type | Mandatory | Description | Available Values
--------- | ------- | ------ | ------------ | ------------ 
grade | Long | N | 플레이어 등급 |  
playerLv | Long	| N | 플레이어 레벨 |  
currency | String | Y | 통화 코드 | ISO 4217 에 명시된 통화 코드
price | BigDecimal | Y | 상품 금액 | 	 
os | String | Y | OS 유형 | android, ios
market | String	| Y | 마켓 유형 | googlePlay, appStore, tStore, ollehMarket, uPlus
marketOrderId | String | Y | 마켓 주문 아이디 |  
marketProductId | String | Y | 마켓 상품 아이디 |  
marketPurchaseTime | Long |Y | 마켓 구매 처리 시각 | epoch time (unit : milli-seconds)
marketPurchaseData | Map<String,Object> | N | 마켓 구매 부가 정보	 | 
purchasePt | Long | N | 구매로 인해 생성된 포인트 | 
purchaseCount | Long | N | 구매 회차 | 

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
logId | String | 기록된 로그에 부여된 ID. 조회시에 nextPageKey 로 사용될 수 있다.

## writeResoruceLog   

```ruby
Example Request
POST /service/writeResourceLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
      "grade":"1",
      "rCurrency" : "al",
      "delta" : 120000,
      "amount" : 56000000,
      "modType" : "add",
      "modTime" : 1444748400000,
      "reason" : "일반알농장",
      "subReason" : "free"
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

재화 변동 로그를 기록한다. 재화 변동 로그는 PlayerLog 의 일부이며 (src, code) 로 (p1, resource) 를 사용한다.
이를 사용하여 재화 변동 로그에 대한 조회를 할 수 있다.
재화 변동 로그는 6개월 후 자동 삭제 된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerdId | String | Y | 플레이어 ID

### Request Body Parameters

Name | Type | Mandatory | Description | Available Values
--------- | ------- | ------ | ------------ | ------------ 
grade | Long | N | 플레이어 등급 | 
playerLv | Long	| N | 플레이어 레벨 | 
currency | String | Y | 리소스 통화 코드 | 
delta | Number | Y | 재화증감분 (+/-) | 
amount | Number	| Y | 현재 재화 보유량 |  
modType | String | Y | 지급/소진/회수 구분 코드 | 
modTime | Long | Y | 발생 시각 | epoch time (unit : milli-seconds)
reason | String| Y | 지급 소진 사유 ex. 재화지급처, ... | ex> 일반리필, 프리미어 리필, ......
subReason | String | N | 지급 소진 상세 사유  ex. 재화 유형, 지급상세사유 코드 | ex> 상자구분코드, 보너스대전아이템구분코드, 루비유무상구분코드

### Response Content
Name | Type  | Description
--------- | ------- | ------ 
logId | String | 기록된 로그에 부여된 ID. 조회시에 nextPageKey 로 사용될 수 있다.

## writeRoundLog   

```ruby
Example Request
POST /service/writeRoundLog HTTP/1.1
Host: openapi-zinny.nzincorp.com
Content-Type: application/json;charset=UTF-8
appId: test01
appSecret: 508c54fbecb6906fcaad2a383eaf3d056776ef36
playerId: 123456.facebook.1
```
```json  
{
    "gameMode": "월드투어",
    "gameModeDtl": "북미오픈 1차전",
    "resultTp": "win",
    "energyAmt": 41234,
    "roundExp": 321,
    "startTime": 1444748400000,
    "endTime": 1444748430000,
    "character1Id": "레이첼",
    "character2Id": "제이슨",
    "character3Id": "세레나",
    "character1Lv": 13,
    "character2Lv": 1,
    "character3Lv": 23
}
```
> The above command returns JSON structured like this:

```ruby 
Example Response
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json  
{
    "logId": "9a57db20-0472-11e5-aed4-b182048b2fd8"
}
```
### Description

판 종료 로그를 기록한다.
게임 한판(round/match 등)이 끝났을 때에 호출한다.
판 종료 로그는 PlayerLog 의 일부이며 (src, code) 로 (g1, round) 를 사용한다.
판 종료 로그는 60일 후 자동 삭제 된다.

### Method
`POST`

### Request Headers
Name | Type | Mandatory | Description
--------- | ------- | ------ | ------------
appId | String | Y | 게임 앱 ID
appSecret | String | Y | 게임마다 발급한 인증 key
playerdId | String | Y | 플레이어 ID

### Request Body Parameters

Name | Type | Mandatory | Description | Available Values
--------- | ------- | ------ | ------------ | ------------ 
grade | Long | N | 플레이어 등급 | 
playerLv | Long	| N | 플레이어 레벨 |   
gameMode | String | Y | 게임 모드 구분 | ex> 월드투어/대전/리그/친선
gameModeDtl | String | N | 게임 모드 하위 구분 | 
roundAttr1 | String | N | 라운드 추가 속성 1st | 
roundAttr2 | String | N | 라운드 추가 속성 2nd | 
betUnit	Long | N | 배팅 단위 | 
betCount | Long	| N | 배팅 단위 개수 | 
resultTp | String | Y | 게임 결과 유형| ex> 승/패/포기
resultAmt | Long | N | 게임 결과 보상량 | 
bonusTp | String | N |	게임 결과 보너스 유형 |  
bonusAmt | Long | N | 게임 결과 보너스 보상량 |  
startTime | Long | Y | 게임 시작 시각 | epoch time (unit : milli-seconds)
endTime	| Long | Y | 게임 종료 시각 | epoch time (unit : milli-seconds)
character1Id | String | N | 첫 번째 캐릭터 아이디 |
character2Id | String | N | 두 번째 캐릭터 아이디 |  
character3Id | String | N | 세 번째 캐릭터 아이디 |  
character1Lv |	Long | N | 첫 번째 캐릭터 레벨 | 
character2Lv | Long | N | 두 번째 캐릭터 레벨 |  
character3Lv | Long | N | 세 번째 캐릭터 레벨 |  
pocketId | String | N | 포켓 아이디 (잭팟러시에서 사용)| 

### Response Content
Name | Type  | Description 
--------- | ------- | ------ 
logId | String | 기록된 로그에 부여된 ID. 조회시에 nextPageKey 로 사용될 수 있다.