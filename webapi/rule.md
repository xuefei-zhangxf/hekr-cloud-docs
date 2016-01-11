# 云端4.0 webapi 文档草稿-规则管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息


## 1.规则管理
### 1.1 添加事件型规则(IFTTT)
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/eventRule" \
  -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxxxxx",
        "pid" : "xxxx",
        "ruleName" : "xxxxx",
        ...
  }'
```
#### 返回
```
< 201
< 同1.2返回
```

### 1.2 列举事件型规则
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/eventRule?devTid=1234&ruleId=1234&page=1&size=1"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  true    |  string  |          | 设备唯一id                   |
| ruleId  |  true    |  string  |          | 规则id                       |
| groupId |  true    |  string  |          | 设备群组id                   |
| size    |  true    |  int     |          | 分页参数                     |
#### 返回
```
< 200
< [
    {
      "ruleId" : "xxxx",
      "ruleName" : "xxxx",
      "code" : "xxx",
      ...
    },...
]
```

### 1.3 编辑事件型规则
只支持完整性更改提交
```
curl -v -X PUT \
  ... \
  "http://webapi.hekr.me/eventRule/{ruleId}" \
  -d '{
        "ruleName": "xxx",
        "code" : "xxx",
        ...
    }'
```
#### 返回
```
< 204
< 同1.2
```

### 1.4 删除事件型规则
```
curl -v -X DELETE \
  ... \
  "http://webapi.hekr.me/eventRule/{ruleId}"
```
#### 返回
```
< 204
< 同1.2
```

### 1.5 添加预约任务
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/schedulerTask" \ 
  -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxxxxx",
        "taskName" : "xxxxx",
        "taskType" : "LOOP",
        "cronExpr" : "1 * * * *",
        ...
  }'
```
以上为最通用的创建定时预约任务调用方式,另外提供2个简易包装的接口;当以下2个接口无法满足条件时只能调用上面的方法进行创建
#### 1.5.1 添加一次性定时预约
```
    -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxx",
        "taskName" : "xxx",
        "taskType" : "ONCE",
        "triggerTime" : "2016/01/07T12:00:00", * localDate,默认东八区时间
        "timeZoneOffset" : -480, * 东八区时区偏移
        ...
    }'
```
#### 1.5.2 循环预约
```
    -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxx",
        "taskName" : "xxx",
        "taskType" : "LOOP",
        "time": "13:49", *localTime,默认东八区时间 
        "repeat" : ["MON","THU","SUN"],
        "timeZoneOffset" : -480,
        ....
    }'
```

#### 返回
```
< 201
< 同1.6
```


### 1.6 列举预约任务
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/schedulerTask?devTid=1234&taskId=1234&state=START&taskType=LOOP&page=1&size=1"
```
排序规则: START排在前面;最新创建的排在前面;最近编辑的排在前面
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  true    |  string  |          | 设备唯一id,按其value筛选       |
| taskId  |  true    |  string  |          | 任务id,按其value进行筛选       |
| taskType|  true    |  string  | ['ONCE', 'LOOP'] | 任务类型,按其value进行筛选 |
| state   |  true    |  string  | ['SCHEDULING','FROZEN']| 任务状态,按其value进行筛选|
| page    |  true    |  int     |          | 分页参数                     |
| size    |  true    |  int     |          | 分页参数                     |
#### 返回
```
< 200
< [
    {
      "taskId" : 1,
      "taskCode" : "power-off",
      "taskName" : "定时001",
      "taskType" : "LOOP",
      "devTid" : "ESP_xxx",
      "cronExpr" : "1 * * * *",
      "nextTriggerTime" : "2016/01/08T12:00:00", localDate,默认东八区时间
      "state": "SCHEDULING",
      "disable" : false,
      "expired" : false,
      "executeCount" : 10,
      "desc" : "我的的定时关机任务",
      "createTime" : "2016/01/07T12:00:00", localDate,默认东八区时间
      "modifyTime" : "2016/01/07T12:00:00", localDate,默认东八区时间
      "serverTime" : "2016/01/07T12:00:00", localDate,默认东八区时间
      "serverTimeZoneOffset" : -480,
      ...
    },...
]
```

### 1.7 编辑预约任务
禁用/恢复 任务时采用PATCH方法提交且只能提交disable值
```
curl -v -X PUT/PATCH \
  ...
  "http://webapi.hekr.me/schedulerTask/{taskId}"
  -d '{
        "taskName": "xxx",
        "taskCode" : "xxx",
        ...
    }'
```

#### 返回
```
< 204
< 同1.6
```

### 1.8 删除预约任务
```
curl -v -X DELETE \
  ... \
  "http://webapi.hekr.me/schedulerTask/{taskId}"
```
#### 返回
```
< 204
< 同1.6
```

