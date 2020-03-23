# 游戏中心

本文档主要讲述了游戏中心的信息

## HTTP 方法说明

HTTP 请求方法有如下语义：

方法 | 语义
-----|-----
GET  | 获取对象的信息，比用户信息，不会修改资源的任何状态
POST | 创建对象，待创建对象的属性需要在 json 格式的body中传入。
PUT  | 更新对象的某个属性，将对象的某个属性设置成特定值。PUT 类接口是幂等的，所以在调用前可以不检查属性是否已经是特定值。
DELETE| 删除对象，如果对象不存在，则会返回 HTTP 404
HEAD | 获取某个对象的原信息，比如查看某个 token 是否合法。


## HTTP API 认证

H5页面需要需要在有两个query string 

1. uid表示用户id
2. channel_id 表示渠道的id

## HTTP Error - 错误约定

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "error": {
        "code": 400,
        "message": "ai"
    }
}
```

## HTTP Success - 成功接口约定

> 列表类型数据


```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "[]"
}
```

> 对象类型数据


```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

> 可以忽略的data直接使用是空字符串

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": ""
}
```


### 说明

1. code只会是200，表明成功
2. data 是一个json.dumps 之后的字符串，游戏段自行解析

## 获取用户信息

```http
GET /v1/gamecenter/user_info?uid=uid&channel_id=channel_id HTTP/1.1
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### data 中的字段说明

|参数|说明|
|-----|-----|
|uid	|玩家id|
|name	|玩家名字|
|channel_id	|渠道Id|
|game_id|	游戏id|
|icon	|头像地址|

## 用户登出

```http
GET /v1/gamecenter/user_logout?uid=uid&channel_id=channel_id HTTP/1.1
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### Query String

| 参数| 说明|
|-----|-----|
| uid | 用户id|
| channel_id | 渠道id|


### data 中的字段说明

无

## 获取游戏列表

```http
GET /v1/gamecenter/game_list?uid=uid&channel_id=channel_id HTTP/1.1
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "[]"
}
```


### Request Body

| 参数| 说明|
|-----|-----|
| uid | 用户id|
| channel_id | 渠道id|

### data中的字段说明

1. data 经过解析之后是一个列表，包含了游戏的信息

|参数|说明|
|-------|------|
|desc| 游戏描述|
|icon| 游戏的图标|
| id| 游戏的id|
|name| 游戏名称|


## 获取游戏下的房间列表

```http
GET /v1/gamecenter/room_list?game_id=13&uid=uid&channel_id=channel_id  HTTP/1.1
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "[]"
}
```

### query string

|参数|是否必填| 说明|
|----|-----|----|
| uid |是| 用户id|
| channel_id |是| 渠道id|
| game_id| 是| 游戏的id|


### data中的字段说明

1. data 经过解析之后是一个列表，包含了游戏的信息

|参数|说明|
|-------|------|
| id| 房间id|
| name | 房间名称|
| participators| 参与者人数|
| viewers| 观战人数 |
| people| 可以参与的人数|
| status| 0 等待, 1 开始|


## 创建游戏房间

```http
POST /v1/gamecenter/room?uid=uid&channel_id=channel_id  HTTP/1.1
Content-Type: application/json

{
    "game_id" : 1
    "people": 4
}

```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"  
}
```

### Query String

|参数|是否必填|说明|
|-----|-----|----|
| uid |是| 用户id|
| channel_id |是| 渠道id|

### Request Body

|参数|是否必填|说明|
|-----|-----|----|
|game_id| 是| 游戏的id|
|people|是| 可以参与的人数|

### data 说明

|参数|说明|
|-------|------|
| id| 房间id|
| name | 房间名称|
| participators| 参与者人数|
| viewers| 观战人数 |
| status| 0 等待, 1 开始|


## 游戏房间详情

```http
GET /v1/gamecenter/room/<room_id>?uid=uid&channel_id=channel_id HTTP/1.1
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### URL Parameter

|参数|说明|
|-----|----|
|game_id| 游戏的id|
|room_id| 房间id|

### Request Body

|参数|是否必填|说明|
|-----|-----|----|
| uid |是| 用户id|
| channel_id |是| 渠道id|

### data 说明

|参数|说明|
|-------|------|
| id| 房间id|
| name | 房间名称|
| participators| 参与者人数|
| viewers| 观战人数 |
| status| 0 等待, 1 开始|
| people| 可以参与的人数|


## 观看加入游戏房间

> 加入

```http
POST /v1/gamecenter/join_room?uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1
}
```

> 观看


```http
POST /v1/gamecenter/view_room?uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### URL paramter

|参数|说明|
|-----|-----|
| room_id| 房间号|


### Request Body

|参数|是否必填|说明|
|-----|-----|----|
| uid |是| 用户id|
| channel_id |是| 渠道id|


### data说明

无


## 退出和关闭房间

> 退出

```http
POST /v1/gamecenter/quit_room?uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1
}
```

> 关闭

```http
POST /v1/gamecenter/close_room?uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1
}
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### URL paramter

|参数|说明|
|-----|-----|
| room_id| 房间号|


### Request Body

|参数|是否必填|说明|
|-----|-----|----|
| uid |是| 用户id|
| channel_id |是| 渠道id|


### data说明

无

## 游戏日志存储

```http
POST /v1/gamecenter/game_logs?uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1,
    "game_id": 1,
    "logs": "",
    "timestamp": 11313131
}
```


```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### Request Body


|参数|说明|
|-----|-----|
| room_id| 房间号|
| game_id| 游戏的id|
| timestamp| 时间戳|
| logs| 游戏端口随意写 |


## 游戏日志获取

```http
GET /v1/gamecenter/game_logs?room_id=1&start=13131313131&end=131313131&uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "[]"
}
```

### Query String

|参数|说明|
|-----|-----|
| room_id| 房间号|
| start| 开始时间戳|
| end| 截止时间戳|


### data 说明

data 是一个字典列表，包含

|参数|说明|
|-------|------|
| timestamp| 时间戳|
| logs| 游戏端口随意写的返回值 |


## 实时数据更新


```http
POST /v1/gamecenter/game_current_logs&uid=uid&channel_id=channel_id HTTP/1.1 
Content-Type: application/json

{
    "room_id": 1,
    "logs": "",
    "timestamp": 11313131
}
```


```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### Request Body


|参数|说明|
|-----|-----|
| room_id| 房间号|
| timestamp| 时间戳|
| logs| 游戏端口随意写 |


## 实时数据获取

```http
GET /v1/gamecenter/game_current_logs?room_id=1&start=13131313131&end=131313131 HTTP/1.1 
Content-Type: application/json

```


```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "code": 200,
    "data": "{}"
}
```

### data 说明

data，包含

|参数|说明|
|-------|------|
| timestamp| 时间戳|
| logs| 游戏端口随意写的返回值|
