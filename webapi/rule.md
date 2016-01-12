# 云端4.0 webapi 文档草稿-规则管理
## 注意
> 以下示例中均忽略http请求头部信息以及AWT认证等信息
ruleKey 由客户端生成并保证唯一性,创建任务后无法更改



## 1.规则管理
### 1.1 添加事件型规则(IFTTT)
force参数设置为true后将强制覆盖已存在的事件型规则
```
curl -v -X POST \
  ... \
  "http://webapi.hekr.me/eventRule" \
  -d '{
        "devTid" : "ESP_xxx",
        "code" : "xxxxxxx",
        "pid" : "xxxx",
        "ruleName" : "xxxxx",
        "ruleKey" : "xxxx",
        "force" : true,
        ...
  }'
```
#### 返回
```
< 201
< 同1.2返回
```

### 1.2 列举事件型规则
若不指定分页参数,则最多返回50条记录
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/eventRule?devTid=1234&ruleId=1234&page=1&size=1"
```
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  可选    |  string  |          | 设备唯一id                   |
| ruleId  |  可选    |  string  |          | 规则id                       |
| groupId |  可选    |  string  |          | 设备群组id                   |
| page    |  可选    |  int     |  [1,?]        | 分页参数                     |
| size    |  可选    |  int     |  [1,50]       | 分页参数                     |
#### 返回
```
< 200
< [
    {
      "ruleId" : "xxxx",
      "ruleName" : "xxxx",
      "code" : "xxx",
      "ruleKey" : "xxxx",
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
force参数设置为true后将强制覆盖已存在的预约
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
        "ruleKey" : "xxxx",
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
        "timeZoneOffset" : -480, * 东八区时区偏移,
        "ruleKey" : "xxxx",
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
        "ruleKey" : "xxxx",
        ....
    }'
```

#### 返回
```
< 201
< 同1.6
```


### 1.6 列举预约任务
若不指定分页信息,则最多返回50条记录
```
curl -v -X GET \
  ... \
  "http://webapi.hekr.me/schedulerTask?devTid=1234&taskId=1234&state=START&taskType=LOOP&ruleKey=xxxx&page=1&size=1"
```
排序规则: START排在前面;最新创建的排在前面;最近编辑的排在前面
#### 参数
| 参数名  | 是否可选 | 参数类型 | 取值范围 | 说明                         |
|:--------|:--------:|:--------:|---------:|:-----------------------------|
| devTid  |  可选    |  string  |          | 设备唯一id,按其value筛选       |
| taskId  |  可选    |  string  |          | 任务id,按其value进行筛选       |
| taskType|  可选    |  string  | ['ONCE', 'LOOP'] | 任务类型,按其value进行筛选 |
| state   |  可选    |  string  | ['SCHEDULING','FROZEN']| 任务状态,按其value进行筛选|
| ruleKey |  可选    |  string  |          | 按其value进行筛选            |
| page    |  可选    |  int     |          | 分页参数                     |
| size    |  可选    |  int     |          | 分页参数                     |
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
      "ruleKey" : "xxxx",
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

