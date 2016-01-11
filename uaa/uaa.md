# 接口文档
## 验证码相关API
### 发送验证码
#### URL
|请求方式|地址|规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/sms/getVerifyCode?phoneNumber={}&pid=00000000000&type=register  |HTTP/1.1|
#### 请求头
|字段|值|
|:--|:--|
|Accept|application/json|
#### 请求参数
|key|类型及范围|说明|
|:--|:--|:--|
|type|String|验证码用途类型(register\|resetPassword)|
|phoneNumber|String|用户手机号码|
---------------------------------------------
### 校验验证码（返回注册使用的Token）
#### URL
|请求方式|地址|规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/sms/checkVerifyCode?phoneNumber={}&code={}  |HTTP/1.1|
#### 请求头
|字段|值|
|:--|:--|
|Accept|application/json|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|phoneNumber|String|用户手机号码|
|code|String|用户收到的验证码(六位验证码)|
#### 返回结果
```
{
  "phoneNumber": "13021298993",
  "verifyCode": "101436",
  "token": "dd27b62f--ce29--4411--9bc2--d1e3386e176f",
  "expireTime": 1452073528209
}
```
#### 返回字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|phoneNumber|Int|用户手机号|
|verifyCode|String|验证码|
|token|String|注册使用的token|
|expireTime|String|token过期时间|
---------------------------------------------
## 用户相关API
### 注册用户
#### URL
|请求方式|地址|规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/register?type=(phone\|email)  |HTTP/1.1|
#### 请求头
|字段 |值 |描述|
|:--|:--|:--|
|Accept|application/json|接受的数据类型|
|Content--Type|application/json|返回的数据类型|
#### 请求参数
|key|类型及范围|说明|
|:--|:--|:--|
|type|phone\|email|用户注册类型|
#### 请求体
*type=phone*
```
{
    "pid":"00000000000",
    "password":"1qazxsw2",
    "dataCenterNode":"CN",
    "phoneNumber":"13021298993",
    "token": "dd27b62f--ce29--4411--9bc2--d1e3386e176f"
}
```
*type=email*
```
{
    "pid":"00000000000",
    "password":"1qazxsw2",
    "dataCenterNode":"CN",
    "email":"zhsyourai@163.com"
}
```
#### 请求体字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|pid|String|厂商Id|
|password|String|用户的密码|
|token|String|校验验证码返回的注册TokenToken|
|dataCenterNode|String|数据节点|
|phoneNumber|String|用户注册手机号|
|email|String|用户邮箱|
#### 返回结果
```
{
  "uid": "51727051945"
}
```
#### 返回字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|uid|String|用户ID|
---------------------------------------------
### 用户登录
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/oauth/token  |HTTP/1.1|
#### 请求头
|字段 |值 |说明|
|:--|:--|:--|
|Authorization|Basic MjMwMDAwMDAwMDpFUzVWRk5QVzQzTDZNOFJCMjdVWFExMFlaT0NHREg5S0lUQUo=|该值生成公式为Base + " " + base64("clientId" + ":" + "clientSecret")|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|username|String|用户名|
|password|String|用户密码|
|scope|String|权限固定为 read write trust|
|grant_type|String|授权方式 填写 password|
#### 返回结果
```
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9.eyJleHAiOjE0NDA4NDAzMjgsInVzZXJfbmFtZSI6IjIwMDk1NjAzNjE3IiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6IjcwOWYyYjc4LTdlNzUtNGIzNC1hYzAxLTM3MzJiZTJlMjZhMiIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAiLCJzY29wZSI6WyJyZWFkIiwidHJ1c3QiLCJ3cml0ZSJdfQ.AlVLFfOHbXAWx96apvFNFshcpK1m--AaM17tjfnnhcJOhKwuz1gNieDxiOIkl0AjNOt0Ch3r5TRe5q7df--hpuCvnFUYUSuK0zouOMx4u1dkcpjxTWhdt3sb25g1mzyL0fouEoHiWs4PMpduPZJcYsas_OoUmFp6Sd8btfdwP2jl_I1FZTmMNonSF44h62Sp9R3KF7gAZ7DX9hR4a_SpFyQULcNRrHjqGh5nXoAH1m5s6lGYGdbQ8MM_X5gX0zzzWNGxRbqOtlF6qdJIASuHn2YIgMjYUiKZssqvAXOiGisNymqpDf_jy1sTl2puH44OH0--EM--XKsjyznjx--3kpe3RZg",
  "token_type": "bearer",
  "refresh_token": "eyJhbGciOiJSUzI1NiJ9.eyJ1c2VyX25hbWUiOiIyMDA5NTYwMzYxNyIsInNjb3BlIjpbInJlYWQiLCJ0cnVzdCIsIndyaXRlIl0sImF0aSI6IjcwOWYyYjc4LTdlNzUtNGIzNC1hYzAxLTM3MzJiZTJlMjZhMiIsImV4cCI6MTQ0MzQyODcyOCwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6ImM1ZjljYjI2LTI2NGEtNDA5My1hNWIwLTJkNDY4YzBjMTI0MCIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAifQ.hhPMoaBTmLt4mBC3euSKa--xxTG1qtZuFW_5xW24otHzx9Ru31A5kGnWkrz54Ro--1nOUHSrnQ9dF9V72JjARU5GpqFgkMX3_s6VLMrsxN6ttls7J96mVE97RZ86aRn4qKI8iJNlbG1QGFX_6zCtjXDQg1u--Z6UdSLc_IJ3S--8D_GmT0sZ6H--61drJlHO6xeShJPU9l0BUbFJRhkHa--JEmtnQ8zeqqL3lin_1WLRViG_MrrCn5F7qPzJphQfmqZfBpDxb3kUwD9Xb--kLs8e6MkmFuv8dHzF9ZtHi_zdrtdprWLzN9M_czKA6rpECBcQXKSFg5MYqzUOMH_adQKb3a0jw",
  "expires_in": 3599,
  "scope": "read trust write",
  "jti": "709f2b78--7e75--4b34--ac01--3732be2e26a2"
}
```
#### 返回字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|access_token|String|授权的访问token|
|token_type|String|与登录时传值相同，或者权限更低|
|refresh_token|String|刷新token|
|expires_in|String|access_token 过期时间|
|scope|String|与登录时传值相同|
---------------------------------------------
### 刷新access_token
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/oauth/token |HTTP/1.1|
#### 请求头a
|字段 |值 |说明
|:--|:--|:--|
|Authorization|Basic MjMwMDAwMDAwMDpFUzVWRk5QVzQzTDZNOFJCMjdVWFExMFlaT0NHREg5S0lUQUo=|该值生成公式为Base + " " + base64("clientId" + ":" + "clientSecret")|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|refresh_token|String|刷新Token|
|grant_type|String|授权方式 填写 refresh_token|
#### 返回结果
```
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9.eyJleHAiOjE0NDA4NDIyMTYsInVzZXJfbmFtZSI6IjIwMDk1NjAzNjE3IiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6ImRmMGEzMWZmLWZkZWUtNDFlNS1hMWFhLTgzYzBmZjRmMmVmMiIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAiLCJzY29wZSI6WyJyZWFkIiwidHJ1c3QiLCJ3cml0ZSJdfQ.I4eXfzQDrph1yL5lO8Ny4_dKKjlvddK6xpl--bdbvmfPAzqj56XV--gQ5KBAMpgivQmadfWtYKpvshf6LOdNq--olVAd6iZOoodkprZiX2C--6PfB2y45pOBsGrOdohat8zz5AkspDmruegt2aMQnmum_A8C9d5L7mEEAXHxf3p4rk89Rg6ah0Z0f3qokiPPCca1ROF9fUsAQ5ExZByPnDt4J3lSE6zpBEy2uWeldSExM5kOernGmcgkFgOIVNlo--9dwGhRuD5IQWyita9TR5LFTF1jGZ0i0bn--wXSmJdzOnk3TL9GNecsg15oeLnJHqbh3B8FUi_vDneM6_k2qbzrkmEw",
  "token_type": "bearer",
  "refresh_token": "eyJhbGciOiJSUzI1NiJ9.eyJ1c2VyX25hbWUiOiIyMDA5NTYwMzYxNyIsInNjb3BlIjpbInJlYWQiLCJ0cnVzdCIsIndyaXRlIl0sImF0aSI6ImRmMGEzMWZmLWZkZWUtNDFlNS1hMWFhLTgzYzBmZjRmMmVmMiIsImV4cCI6MTQ0MzQzMDI1NiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6IjE5NDM3YzU0LTY2MjEtNGM1Ni04Y2RlLTY3ZTgxNjc1YzQ2MyIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAifQ.flBLnFwE2DyHIISQfA6X1mxugzBJf0_GYPcCcA1JYaHUIjq2LFfjfKQnGZRdJBDmNFp2wlCMSUpV5Lzuqj1S82LeLlEaH--W7--XMdF8MZFYiN1_iVmNJOu2IwoESoUerh3zPD16IDJUqHEBDz9dDWzmxUXbmpasfOrwLGTj4eIRUo4xcbru2ASaombrdAC8HBPTTOE1_qle6edc7AMxdAXq0nOmYaJGOB03PEDPwl8KXaAPth4JyubusqNO68VxxOv358amJvt2YQpaDlIC--X5CT2nJYVrEb0apDKvamFshyC4REMHjC3tnmYjuY9G8SUzEHufgPNZIFbnP029E42DA",
  "expires_in": 3599,
  "scope": "read trust write",
  "jti": "df0a31ff--fdee--41e5--a1aa--83c0ff4f2ef2"
}
```

#### 返回字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|access_token|String|授权的访问token|
|token_type|String|与登录时传值相同，或者权限更低|
|refresh_token|String|刷新token|
|expires_in|String|access_token 过期时间|
|scope|String|与登录时传值相同|
---------------------------------------------
### 刷新access_token
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/oauth/token |HTTP/1.1|
#### 请求头
|字段 |值 |说明
|:--|:--|:--|
|Authorization|Basic MjMwMDAwMDAwMDpFUzVWRk5QVzQzTDZNOFJCMjdVWFExMFlaT0NHREg5S0lUQUo=|该值生成公式为Base + " " + base64("clientId" + ":" + "clientSecret")|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|refresh_token|String|刷新Token|
|grant_type|String|授权方式 填写 refresh_token|
#### 返回结果
```
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9.eyJleHAiOjE0NDA4NDIyMTYsInVzZXJfbmFtZSI6IjIwMDk1NjAzNjE3IiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6ImRmMGEzMWZmLWZkZWUtNDFlNS1hMWFhLTgzYzBmZjRmMmVmMiIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAiLCJzY29wZSI6WyJyZWFkIiwidHJ1c3QiLCJ3cml0ZSJdfQ.I4eXfzQDrph1yL5lO8Ny4_dKKjlvddK6xpl--bdbvmfPAzqj56XV--gQ5KBAMpgivQmadfWtYKpvshf6LOdNq--olVAd6iZOoodkprZiX2C--6PfB2y45pOBsGrOdohat8zz5AkspDmruegt2aMQnmum_A8C9d5L7mEEAXHxf3p4rk89Rg6ah0Z0f3qokiPPCca1ROF9fUsAQ5ExZByPnDt4J3lSE6zpBEy2uWeldSExM5kOernGmcgkFgOIVNlo--9dwGhRuD5IQWyita9TR5LFTF1jGZ0i0bn--wXSmJdzOnk3TL9GNecsg15oeLnJHqbh3B8FUi_vDneM6_k2qbzrkmEw",
  "token_type": "bearer",
  "refresh_token": "eyJhbGciOiJSUzI1NiJ9.eyJ1c2VyX25hbWUiOiIyMDA5NTYwMzYxNyIsInNjb3BlIjpbInJlYWQiLCJ0cnVzdCIsIndyaXRlIl0sImF0aSI6ImRmMGEzMWZmLWZkZWUtNDFlNS1hMWFhLTgzYzBmZjRmMmVmMiIsImV4cCI6MTQ0MzQzMDI1NiwiYXV0aG9yaXRpZXMiOlsiUk9MRV9VU0VSIl0sImp0aSI6IjE5NDM3YzU0LTY2MjEtNGM1Ni04Y2RlLTY3ZTgxNjc1YzQ2MyIsImNsaWVudF9pZCI6IjIzMDAwMDAwMDAifQ.flBLnFwE2DyHIISQfA6X1mxugzBJf0_GYPcCcA1JYaHUIjq2LFfjfKQnGZRdJBDmNFp2wlCMSUpV5Lzuqj1S82LeLlEaH--W7--XMdF8MZFYiN1_iVmNJOu2IwoESoUerh3zPD16IDJUqHEBDz9dDWzmxUXbmpasfOrwLGTj4eIRUo4xcbru2ASaombrdAC8HBPTTOE1_qle6edc7AMxdAXq0nOmYaJGOB03PEDPwl8KXaAPth4JyubusqNO68VxxOv358amJvt2YQpaDlIC--X5CT2nJYVrEb0apDKvamFshyC4REMHjC3tnmYjuY9G8SUzEHufgPNZIFbnP029E42DA",
  "expires_in": 3599,
  "scope": "read trust write",
  "jti": "df0a31ff--fdee--41e5--a1aa--83c0ff4f2ef2"
}
```
#### 返回字段说明
|返回值字段 |字段类型 |字段说明|
|:--|:--|:--|
|access_token|String|授权的访问token|
|token_type|String|与登录时传值相同，或者权限更低|
|refresh_token|String|刷新token|
|expires_in|String|access_token 过期时间|
|scope|String|与登录时传值相同|
---------------------------------------------
### 重置密码
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/resetPassword?type=(phone\|email) |HTTP/1.1|
#### 请求参数（type=phone）
|key |类型及范围 |说明|
|:--|:--|:--|
|phoneNumber|String|用户手机号|
|verifyCode|String|验证码|
|pid|String|pid|
|password|String|需要重置的密码|
#### 请求参数（type=email）
|key |类型及范围 |说明|
|:--|:--|:--|
|pid|String|pid|
|token|String|发送重置邮件时，发送到邮件内的token|
|password|String|需要重置的密码|
---------------------------------------------
### 修改密码（需要登录）
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/changePassword |HTTP/1.1|
#### 请求头
|字段 |值 |说明
|:--|:--|:--|
|Authorization|Bearer MjMwMDAwMDAwMDpFUzVWRk5QVzQzTDZNOFJCMjdVWFExMFlaT0NHREg5S0lUQUo=|登陆时返回的access_token|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|oldPassword|String|旧密码|
|newPassword|String|新密码|
|pid|String|pid|
---------------------------------------------
### 修改手机（需要登录）
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/changePhoneNumber |HTTP/1.1|
#### 请求头
|字段 |值 |说明
|:--|:--|:--|
|Authorization|Bearer MjMwMDAwMDAwMDpFUzVWRk5QVzQzTDZNOFJCMjdVWFExMFlaT0NHREg5S0lUQUo=|登陆时返回的access_token|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|token|String|通过 校验验证码 接口获取的token|
|phoneNumber|String|手机号|
|verifyCode|String|验证码|
|pid|String|pid|
---------------------------------------------
### 修改邮箱
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|POST|http://uaa.hekr.me/uaa/changeEmail |HTTP/1.1|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|token|String|发送到邮箱内的token|
|pid|String|pid|
---------------------------------------------
### 发送重置密码邮件
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/sendResetPasswordEmail |HTTP/1.1|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|email|String|邮箱|
|pid|String|pid|
---------------------------------------------
### 重新发送确认邮件
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/resendVerifiedEmail |HTTP/1.1|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|email|String|邮箱|
|pid|String|pid|
---------------------------------------------
### 发送修改邮箱邮件
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/sendChangeEmailStep1Email |HTTP/1.1|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|email|String|邮箱|
|pid|String|pid|
---------------------------------------------
### 发送新邮箱确认邮件
#### URL
|请求方式 |地址 |规范|
|:--|:--|:--|
|GET|http://uaa.hekr.me/uaa/sendChangeEmailStep2Email |HTTP/1.1|
#### 请求参数
|key |类型及范围 |说明|
|:--|:--|:--|
|email|String|邮箱|
|token|String|上一步发到邮箱内链接的token|
|pid|String|pid|
---------------------------------------------