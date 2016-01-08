# 云端4.0 webapi 文档草稿-用户管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息

必须有pid,mid可有可无



## 1.用户管理
### 1.1 获取用户档案(名字,性别,头像,生日)
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/{userId}"
```
#### 返回
```
< 200
< {
  "name": "氦氪小李",
  "gender" : "male",
  "birthDay" : "xxxx",
  "avatr" : "http://ufilexxx/xxxxx"
}
```

### 1.2 设置用户偏好
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/user/{userId}/preferences/{pid}/{mid}" \
  -d '{
        "data" : {
        "kv1": "value1",
        ...
      }
    }'
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| pid     |  false   |  string  |          | 厂商id                      |
| mid     |  true    |  string  |          | 型号id                     |

#### 返回
```
< 201
< 创建的偏好数据

```

### 1.3 获取用户偏好设置
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/{userId}/preferences/{pid}/{mid}"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| pid     |  false   |  string  |          | 厂商id                      |
| mid     |  true    |  string  |          | 型号id                     |
#### 返回
```
< 200
< {
  "kv1": "data1",
  "kv2": "data2",
  ...
}
```

### 1.4 修改用户偏好设置
```
curl -v -X PUT/PATCH \
  ... \
  "http://webapi.hekr.me/user/{userId}/preferences/{pid}/{mid}" \
  -d '{
      "kv1" : "xxx",
      ...
    }'
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| pid     |  false   |  string  |          | 厂商id                      |
| mid     |  true    |  string  |          | 型号id                     |
#### 返回
```
< 204
< {
  "kv1": "data1",
  ....
}
```

### 1.5 获取告警信息
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/warnings?waringId=123,456,789&startTime=xxx&endTime=xxx&devTid=1234&mid=midxxx&page=1&size=20"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
|warningId|  true    |  string  |          | 告警id,多个使用逗号分隔    |
| devTid  |  true    |  string  |          | 设备唯一id                   |
| groupId |  true    |  string  |          | 设备群组id                   |
| mid     |  true    |  string  |          | 设备型号id                   |
|startTime|  true    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  true    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |
#### 返回
```
< 200
< [
  {
    "warningId": "xxxx",
    "devTid" : "xxx",
    "content": "xxx",
  }
]
```


### 1.6 设备上报数据统计查询
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/{userId}/deviceStatistics?devTid=1234&startTime=1111&endTime=1111&method=all&mid=xxx&dataTag=pm2.5
  &page=1&size=20"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| userId  |  false   |  string  |          | 用户唯一id                  |
| devTid  |  false   |  string  |          | 设备唯一id                   |
| method  |  false   |  string  |          | 查询方法                     |
| mid     |  false   |  string  |          | 查询设备的型号id              |
| dataTag |  false   |  string  |          | 查询数据的标签                |
|startTime|  true    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  true    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |

#### 返回
```
< 200
< {
  "scale": 20,
  "interval": 20,
  "page" : 1,
  "size" : 20,
  "value": [
    {
      "time": xxx,
      "value" : 22,
      ...
    },...
  ]
}
```

### 1.7 上传用户参数记录
```
curl -v -X POST \
  ...  \
  "http://webapi.hekr.me/user/{userId}/customData/{pid}?mid=mid123" \
  -d '{
    "pm2.5": 100,
    "drink" : 100,
    ...
  }'
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| userId  |  false   |  string  |          | 用户唯一id                  |
| pid     |  false   |  string  |          | 设备的厂商id                |
| mid     |  true    |  string  |          | 设备的型号id                |
#### 返回
```
< 201
< 上传的数据
```

### 1.8 查询用户参数记录
```
curl -v -X GET \
  ...   \
  "http://webapi.hekr.me/user/{userId}/customData/{pid}?mid=1234&startTime=111&endTime=111&page=1&size=1"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| userId  |  false   |  string  |          | 用户唯一id                  |
| pid     |  false   |  string  |          | 设备的厂商id                |
| mid     |  true    |  string  |          | 设备的型号id                |
|startTime|  true    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  true    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |

#### 返回
```
< 200
< {
    "page" : 1,
    "size": 20,
    "recordList": [
        {
          "kv1": "value1",
          "kv2" : "value2"
        },
      ...
    ]
}
```

### 1.9 查询用户参数记录统计
用户APP上报的记录本身部分具有值类型,该接口负责统计该类标签的数据统计
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/{userId}/userStatistics?startTime=1111&endTime=1111&method=all&mid=xxx&dataTag=pm2.5
  &page=1&size=20"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| userId  |  false   |  string  |          | 用户唯一id                  |
| method  |  false   |  string  |          | 查询方法                     |
| mid     |  true   |  string  |          | 查询设备的型号id              |
| method  |  false   |  string  |          | 查询方法                     |
| dataTag |  false   |  string  |          | 查询数据的标签                |
|startTime|  true    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  true    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |

#### 返回
```
< 200
< {
  "scale": 20,
  "interval": 20,
  "page" : 1,
  "size" : 20,
  "value": [
    {
      "time": xxx,
      "value" : 22,
      ...
    },...
  ]
}

```

### 1.10 上传文件(图像/视频/音频)
```
curl -F "file=xxxx;type=xxx" \
  "http://webapi.hekr.me/user/uploadFile"
```
#### 返回
```
< 201
< {
    "url" : "xxxx",
    ....
}
```

### 1.10 列举已上传文件
```
curl -v -X GET \
    ... \
    "http://webapi.hekr.me/user/{userId}?fileName=xxx&page=1&size=20"
```
### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| userId  |  false   |  string  |          | 用户唯一id                  |
| fileName|  true    |  string  |          | 文件名                      |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |
#### 返回
```
< 200
< {
    "page" : 1,
    "size" : 20,
    "fileList" : [
        {
            "fileName": xxx,
            "url": "xxxxx",
            "uploadTime" : "2016/01/07T12:00:00Z",
        },...
    ]
}
```

### 1.11 删除已上传的文件
```
curl -v -X DELETE \
    ... \
    "http://webapi.hekr.me/user/{userId}/{fileName}"
```
#### 返回
```
< 204
< 同1.10
```