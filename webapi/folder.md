# 云端4.0 webapi 文档草稿-目录管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息
设备群组(group)本期内并不添加,故文档暂不涉及

## 1.目录管理
### 1.1 添加目录
一个用户最多只能建立128个目录
```
curl -v -X POST \
  ... \
  -H "Content-Type: text/plain" \
  "http://webapi.hekr.me/folder" \
  -d "folderName"
```
#### 返回
```
< 201
< 同1.2返回
```

### 1.2 列举目录(设备资源浏览器 deviceExplorer)
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/folder?folderId=1234&page=1&size=1"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| folderId|  true    |  string  |          | 设备分组id                   |
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |
#### 返回
```
< 200
< [
    {
      "folderId" : "xxxx",
      "folderName" : "xxx",
      "deviceList" : ["ESP_xxx1", "ESP_xxx2", "ESP_xxx3"]
      ...
    },...
]
```

### 1.3 修改目录名称
目录的名称最长不得超过128个字符
```
curl -v -X PUT \
  ... \
  -H "Content-Type: text/plain" \
  "http://webapi.hekr.me/folder/{folderId}" \
  -d "newFolderName"
```
#### 返回
```
< 204
< 同1.2
```

### 1.4 删除目录
目录下有设备也可以删除,后续动作是把这些设备挪到根目录下
```
curl -v -X DELETE \
  ... \
  "http://webapi.hekr.me/folder/{folderId}"
```
#### 返回
```
< 204
< 同1.2
```

### 1.5 将设备挪到指定目录
一个目录(含根目录)下最多可以放置512个设备
设备可以存在在根目录也可以存在在其他目录中
```
curl -v -X POST \
  ... \
  -H "Content-Type: text/plain" \
  "http://webapi.hekr.me/folder/{folderId}" \
  -d "devTid"
```
#### 返回
```
< 201
< 同1.2返回
```

### 1.6 将设备从目录挪到根目录下
```
curl -v -X DELETE \
  ... \
  "http://webapi.hekr.me/folder/{folderId}/{devTid}"
```
#### 返回
```
< 204
< 同1.2返回
```
