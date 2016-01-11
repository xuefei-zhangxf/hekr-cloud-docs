# 云端4.0 webapi 文档草稿-设备管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息


## 1.设备管理
### 1.1 绑定设备
```
curl -v -X POST \
  ... \
  -H "Content-Type: text/plain" \
  "http://webapi.hekr.me/device" \
  -d "devTid"
```
### 返回
```
< 201
< 同1.2返回
```

### 1.2 列举设备列表
不指定分页信息则默认返回50条记录
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/device?devTid=tid1234,tid22,tid55&folderId=fid1234&groupId=gid1234&page=1&size=10"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  true    |  string  |          | 设备唯一id,多个使用逗号分隔    |
| folderId|  true    |  string  |          | 目录id                   |
| groupId |  true    |  string  |          | 群id                   |
| page    |  true    |  int     |  [1,?]   | 分页参数                     |
| size    |  true    |  int     |  [1,50] | 分页参数                     |
#### 返回
```
< 200
< [
    {
      "ownerUid" : "uid1234",
      "devTid" : "tid1234",
      "deviceName": "玩具001",
      "folderId": "fid1234",
      "folderName": "客厅",
      "groupInfo" : [
        {
            *4.1版本支持*
            "groupId" : "gid1234",
            "groupName" : "联动小组001"
        },...
      ]
      "binVersion": "3.0.a",
      "SSID" : "NETGEAR48-5G",
      "lastLoginTime": "2016-01-06T12:00:00Z",
      "lastUpdateTime": "2015-12-25T10:00:00Z",
      "bindTime": "2015-10-01T10:00:00Z",
      "isGranted": true,
      "isSetSchedulerTask": true,
      ...
    },
    ...
]
```

### 1.3 删除设备
此处删除设备即为彻底解绑,并无之前版本的二义性
```
curl -v -X DELETE \
  ... \
  "http://webapi.hekr.me/device/{devTid}"
```
#### 返回
```
< 204
< 同1.2
```

### 1.4 更改设备名称
```
curl -v -X PUT \
    ... \
    -H "Content-Type: text/plain" \
    "http://webapi.hekr.me/device/{devTid}/deviceName" \
    -d "New Name"
```
#### 返回
```
< 204
< 同1.2
```

### 修订记录
| 修订人 |    日期    | 备注 |
|:-------|:----------:|:----:|
| 张雪飞 | 2016/01/07 | 创建 |