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

### 1.5 添加定时预约/周期性/倒计时任务
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/schedulerTask" \ 
  -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxxxxx",
        "taskName" : "xxxxx",
        "type" : "SCHEDULER",
        "cronExpr" : "1 * * * *",
        ...
  }'
```
以上为最普遍的创建定时预约任务调用方式,另外提供2个简易包装的接口
还有`倒计时任务的创建`
#### 1.5.1 添加一次性定时预约
提交内容
```
    -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxx",
        "taskName" : "xxx",
        "type" : "SCHEDULER",
        "triggerTime" : "2016/01/07T23:55:55Z",
        ...
    }'
```
#### 1.5.2 循环任务
提交内容
```
    -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxx",
        "taskName" : "xxx",
        "type" : "SCHEDULER",
        "time": "13:49", *localtime*
        "repeat" : ["MON","THU","SUN"],
        "timeZoneOffset" : 480,
        ....
    }'
```

#### 1.5.3 倒计时任务
提交内容
```
    -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxx",
        "taskName" : "xxx",
        "type" : "COUNTDOWN",
        "countDown" : 120,
        "time": "13:49", *localtime*
        "timeZoneOffset" : 480,
        ....
    }'
```

#### 返回
```
< 201
< 同1.6
```


### 1.6 列举 定时预约/周期性规则
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/schedulerTask?devTid=1234&taskId=1234&state=START&taskType=COUNTDOWN&page=1&size=1"
```
排序规则: START排在前面;最新创建的排在前面;最近编辑的排在前面
倒计时任务
当state为ERROR时指一次性任务(倒计时)指任务未执行且过期
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  true    |  string  |          | 设备唯一id,按其value筛选       |
| taskId  |  true    |  string  |          | 任务id,按其value进行筛选       |
| taskType|  true    |  string  | ['SCHEDULER', 'COUNTDOWN'] | 任务类型,按其value进行筛选 |
| state   |  true    |  string  | ['START', 'END', 'ERROR', 'FROZEN']| 任务状态,按其value进行筛选|
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
      "taskType" : "SCHEDULER",
      "devTid" : "ESP_xxx",
      "cronExpr" : "1 * * * *",
      "nextTriggerTime" : "2014-11-07T14:00:00Z"(UTC时间),
      "state": "START",
      "disable" : false,
      "executeCount" : 10,
      "desc" : "我的的定时关机任务",
      "createTime" : "2014-11-07T14:00:00Z"(UTC时间),
      "modifyTime" : "2014-11-07T14:00:00Z"(UTC时间),
      "standardTime" : "2014-11-07T14:00:00Z"(当前服务器UTC时间)
      ...
    },...
]
```

### 1.7 编辑 定时预约/周期性/倒计时任务
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

### 1.8 删除 定时预约/周期性/倒计时任务
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

