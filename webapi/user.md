# 云端4.0 webapi 文档草稿-用户管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息



## 1.用户管理
### 1.1 获取用户档案(名字,性别,头像,生日)
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user"
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
### 1.2 更新用户档案
```
curl -v -X PUT/PATCH \
    ... \
    "http://webapi.hekr.me/user"
    -d '{
        "name" : "氦氪小毛",
        ....
    }'
```
#### 返回
```
< 204
< No Content
```

### 1.3 设置用户偏好
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/user/preferences/{mid}" \
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
| mid     |  可选    |  string  |          | 型号id                     |

#### 返回
```
< 201
< 创建的偏好数据
```

### 1.4 获取用户偏好设置
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/preferences/{mid}"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| mid     |  可选    |  string  |          | 型号id                     |
#### 返回
```
< 200
< {
  "kv1": "data1",
  "kv2": "data2",
  ...
}
```
若mid为空则
```
< 200
< {
    "global": {
        "kv1": "data1",
        "kv2" : "data2",
    },
    "mid1": {
        "kv1": "data1",
        "kv2": "data2",
        ...
    },
    "mid2" : {
        ...
    },
   }
```

### 1.5 修改用户偏好设置
```
curl -v -X PUT/PATCH \
  ... \
  "http://webapi.hekr.me/user/preferences/{mid}" \
  -d '{
      "kv1" : "xxx",
      ...
    }'
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| mid     |  可选    |  string  |          | 型号id                     |
#### 返回
```
< 204
< {
    "kv1": "data1",
  ....
}
```

### 1.6 获取告警信息
```
不指定分页信息则默认最多返回最新50条记录
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/warnings?waringId=123,456,789&startTime=xxx&endTime=xxx&devTid=1234&mid=midxxx&page=1&size=20"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|:--------:|:-----------------------------|
|warningId|  可选    |  string  |          | 告警id,多个使用逗号分隔    |
| devTid  |  可选    |  string  |          | 设备唯一id                   |
| groupId |  可选    |  string  |          | 设备群组id                   |
| mid     |  可选    |  string  |          | 设备型号id                   |
|startTime|  可选    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  可选    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  可选    |  int     |   [1,?]  | 分页参数                     |
| size    |  可选    |  int     |   [1,50] | 分页参数                     |
#### 返回
```
< 200
< {
    "page": 1,
    "size" : 20,
    "valueList" : [
      {
        "warningId": "xxxx",
        "devTid" : "xxx",
        "content": "xxx",
      },...
  ]
}
```


### 1.7 设备参数统计查询

![alt text](http://zxfcmd.oss-cn-hangzhou.aliyuncs.com/%E7%BB%9F%E8%AE%A1.png "title")
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/deviceStatistics?devTid=1234&startTime=1111&endTime=1111&method=all&mid=xxx&dataTag=pm2.5"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|:---------|:-----------------------------|
| devTid  |  必选    |  string  |          | 设备唯一id                   |
| method  |  必选    |  string  |['MAX','MIN','AVG','SUM','COUNT']| 统计方法 |
| mid     |  必选    |  string  |          | 查询设备的型号id              |
| dataTag |  必选    |  string  |          | 查询数据的标签                |
|startTime|  必选    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  可选    |  long    |          | 查询结束时间,毫秒时间戳        |

#### 返回
```
< 200
< {
    "scale": 20,
    "interval": 20,
    "page" : 1,
    "size" : 20,
    "valueList": \[
        {
          "time": xxx,
          "value" : 22,
          ...
        },...
      \],
}
```

### 1.8 上传用户参数记录
mid可选,例如用户需要定位到具体某个型号的详细记录
```
curl -v -X POST \
  ...  \
  "http://webapi.hekr.me/user/customData?mid=mid123" \
  -d '{
        "pm2.5": 100,
        "drink" : 100,
        "devTid" : "ESP_xxxx",
        ...
    }'
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| mid     |  可选    |  string  |          | 设备的型号id                |
#### 返回
```
< 201
< 上传的数据
```

### 1.9 查询用户参数记录
该接口用于查询记录;若不指定分页参数则默认返回最近50条记录
```
curl -v -X GET \
  ...   \
  "http://webapi.hekr.me/user/customData?mid=1234&startTime=111&endTime=111&page=1&size=1"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| mid     |  可选    |  string  |          | 设备的型号id                |
|startTime|  可选    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  可选    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  可选    |  int     |  [1,?]   | 分页参数                     |
| size    |  可选    |  int     |  [1,50]  | 分页参数                     |

#### 返回
```
< 200
< {
    "page" : 1,
    "size": 20,
    "valueList": [
        {
          "kv1": "value1",
          "kv2" : "value2"
        },
      ...
    ]
}
```

### 1.10 用户参数统计查询
用户APP上报的记录本身部分具有值类型,该接口负责统计该类标签的数据统计

![alt text](http://zxfcmd.oss-cn-hangzhou.aliyuncs.com/%E7%BB%9F%E8%AE%A1.png "title")
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/user/userStatistics?startTime=1111&endTime=1111&method=all&mid=xxx&dataTag=pm2.5
  &page=1&size=20"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| mid     |  可选    |  string  |          | 查询设备的型号id              |
| method  |  必选    |  string  |  ['MAX','MIN','AVG','SUM','COUNT']|查询方法|
| dataTag |  必选    |  string  |          | 查询数据的标签                |
|startTime|  必选    |  long    |          | 查询起始时间,毫秒时间戳        |
|endTime  |  可选    |  long    |          | 查询结束时间,毫秒时间戳        |
| page    |  可选    |  int     |          | 分页参数                     |
| size    |  可选    |  int     |          | 分页参数                     |

#### 返回
```
< 200
< {
    "scale": 20,
    "interval": 20,
    "page" : 1,
    "size" : 20,
    "valueList": [
        {
          "time": xxx,
          "value" : 22,
          ...
        },...
  ]
}

```

### 1.11 上传文件(图像/视频/音频)
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

### 1.12 列举已上传文件
不指定分页参数,默认最多返回50条记录
```
curl -v -X GET \
    ... \
    "http://webapi.hekr.me/user/file?fileName=xxx&page=1&size=20"
```
### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| fileName|  可选    |  string  |          | 文件名                      |
| page    |  可选    |  int     | [1,?]          | 分页参数                     |
| size    |  可选    |  int     | [1,50]     | 分页参数                     |
#### 返回
```
< 200
< {
    "page" : 1,
    "size" : 20,
    "valueList" : [
        {
            "fileName": xxx,
            "url": "xxxxx",
            "uploadTime" : "2016/01/07T12:00:00Z",
        },...
    ]
}
```

### 1.13 删除已上传的文件
```
curl -v -X DELETE \
    ... \
    "http://webapi.hekr.me/user/file/{fileName}"
```
#### 返回
```
< 204
< 同1.11
```