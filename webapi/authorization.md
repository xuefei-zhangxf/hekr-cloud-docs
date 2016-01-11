# 云端4.0 webapi 文档草稿-授权
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息


## 1.授权
### 1.1 创建授权
每个设备最多只能授权给n个人,这个n由设备具体型号决定
```
curl -v -X POST \
  ...
  "http://webapi.hekr.me/authorization" \
  -d '{
        "grantor" : "uid1",
        "grantee" : "uid2",
        "devTid" : "ESP_xxx",
        "expire" : 300,
        "mode" : "ALL",
        "desc": "xxxx"
  }'

```

以上的case为全部指令授权时 ,若需要创建指定若干指令的授权,则选择以下方式提交

```
  ...
  -d '{
      "grantor" : "uid1",
      "grantee" : "uid2",
      "devTid" : "ESP_xxx",
      "expire" : {
        "brush" : 200,
        "power-off" : 100,
        ...
      },
      "mode" : "PARTIAL",
      "desc" : "xxxxx"
    }'
```

#### 返回
```
< 201
< {
    # 同1.5返回
}
```

### 1.2 反向授权创建
反向授权提交后,向设备拥有者推送一个授权设备的请求,随后grantor收到之后,点击同意则调用1.1 授权
```
curl -v -X POST \
  ...
  "http://webapi.hekr.me/authorization" \
  -d '{
        "devTid" : "ESP_xxx",
        "grantee" : "uid1"
    }'
```
#### 返回
```
< 201
< No Content
```

### 1.3 编辑授权
只支持完整提交
```
curl -v -x PUT \
  ...
  "http://webapi.hekr.me/authorization" \
  -d '{
      ...
      #同1.1完整授权对象
  }'
```
#### 返回
```
< 204
< {
  # 同1.5返回
}
```

### 1.4 取消指定授权（一个或多个)
取消是指取消整体的授权关系,倘若需要取消具体某个指令的授权请使用1.3
不传递grantee参数则为 `取消指定设备上对所有用户的全部授权`
```
curl -v -x DELETE \
  ...
  "http://webapi.hekr.me/authorization?grantor=xxx&grantee=123,456,789&devTid=ESP_xxx"
```

#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| grantor |  false   |  string  |          | 授权者uid                    |
| grantee |  false   |  string  |          | 被授权者uid,多个使用逗号分隔    |
| devTid  |  false   |  string  |          | 设备唯一id                   |


#### 返回
```
< 204
< {
  # 同1.5返回
}
```

### 1.5 列举指定设备上的授权信息

```
curl -v -X GET \
  ...
  "http://webapi.hekr.me/authorization?grantor=xxx&devTid=ESP_xxx&&grantee=yyy,xxx,123"
```

#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| grantor |  false   |  string  |          | 授权者uid                    |
| grantee |   true   |  string  |          | 被授权者uid,多个使用逗号分隔 |
| devTid  |  false   |  string  |          | 设备唯一id                   |

#### 返回
```
< 200
<
[
  {
        "grantor" : "uid1",
        "grantorName": "小明",
        "grantee" : "uid2",
        "granteeName": "小李",
        "devTid" : "ESP_xxx",
        "expire" : 300,
        "desc": "test"
      },
      ...
]
```


### 修订记录
| 修订人 |    日期    | 备注 |
|:-------|:----------:|:----:|
| 张雪飞 | 2016/01/07 | 创建 |
