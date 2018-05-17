## Server For 小程序 and App
 - AlibabaCloud:

 1. 公网IP: 39.106.19.44/wxapp.digitgolf.com
 2. 中通奥博完成一级域名digitgolf.com备案
 
- Nginx
1. 设置/etc/nginx/nginx.conf

- SDK/API/Router

| Router             | Method  | Explain                   |
| :----------------  | :-----  | :---------------------    |
| /                  | get     | 获取后台登录页面            |
| /home              | get     | 获取后台首页                |
| /store             | get     | 获取所有舱体                |
| /addstore          | get     | 新建舱体页面                |
| /store/add         | post    | 上传新建舱体信息             |
| /store/repair      | post    | 问题反馈                    |
| /power/switchone   | get     | 切换某一个舱体某一路电控开关  |
| /users/wxlogin     | get     | (微信）根据code获取openid    |
| /users/register    | get     | 用户注册到数据库             |
| /users/getUserInfo | get     | (微信)根据openid获取用户信息 |
| /users/updatemoney | get     | 更新用户账户金额             |
| /pay               | get     | (微信)支付统一下单           |
| /pay/notify        | get     | (微信)支付通知notify_url     |

- Detail Spec
URL: https://wxapp.digitgolf.com

1. web后台
```
url: /
method: get
data: {}
```
**desc: 获取后台主页**
```
url: /home
method: get
data: {
	username: root,
	password: 123456
}
```
**desc:后台主页登录**
```
url: /addstore
method: get
data: {}
```
**desc:获取添加舱体页面**

2. 小程序/App

request:
```
url: /users/wxlogin
method: get
data: {
	code: xxxx
}
```
response:
```
{
code: 0,
data: {
	openid: openid,
	session_key: session_key
 },
msg: 'get openid ok'
}
```
**desc: 小程序根据code获取openid**

request:
```
url: /users/register
method: get
data: {
    nickName： '大土豆'
    gender： 0
    language： 'Chinese'
    city： '北京'
    province： '北京'
    country： 'China'
    avatarUrl：'http://lc-xy0sgv99.cn-n1.lcfile.com/9cec9c166001beb9b8a0.jpg'
    openid：'xxxx'
}
```
response:
```
{
  code:1, //0、1、2
  data:{},
  msg:'xxxx'
}
```
**desc:用户账户注册**

request:
```
url: /users/getUserInfo
method: get
data:{
    openid: 'xxxx'
}
```
response:
```
{
    code: 0, //0、1、2
    data: {},
    timeNode: 'xxxxx',
    msg:'user exist'
}
```
**desc:获取用户信息**

request:
```
url: /users/updatemoney
method: get
data:{
    openid: 'xxx',
    money: 100
    SN: '123456'
}
```
response:
```
{
  code:0,
  data:{
    money: 100
  },
  msg:'扣费成功'
}      
```
**desc:更新用户账户金额**

request:
```
url: /store
method: get
data: {}
```
response:
```
{
    code: 0,
    data: [{}, {},...{}]
    msg: 'get all store info ok'
}
```
**desc:获取所有舱体信息**

request:
```
url: /store/add
method: post
data: {
  SN : 123456,
  type : 0,  //0:not use power box; 1:use power box
  name : '文津一号',
  address : '五道口文津酒店一层',
  longitude : 116.30269,
  latitude : 36.234569;
  price : 60,
  status : 0,  //0:未使用; 1:使用中
  imgurl : 'http://lc-xy0sgv99.cn-n1.lcfile.com/5a5bdb245e8a7b4d886c.jpg'  
}
```
response:
```
{
    code: 1, // 0、1、2
    data: {},
    msg : 'carbin exist,return info'
}
```
**desc:舱体注册到数据库**

request:
```
url: /store/repair
method: post
header: {content-type: application/json}
data: {
    picUris: ['http://lc-xy0sgv99.cn-n1.lcfile.com/5a5bdb245e8a7b4d886c.jpg',                  
    		'http://lc-xy0sgv99.cn-n1.lcfile.com/5a5bdb245e8a7b4d886c.jpg'],
    num: 123456,
    desc: '抓紧来修吧！',
    spec: ['软件打不开', '不出球']
}
```
response:
```
{
    code: 0,
    data: {},
    msg: '谢谢反馈'
}
```
**desc:舱体故障报修**

request:
```
url: /power/switchone
method: get
data:{
    openid: 'fdafdafk456',
    SN: 123456,  
    address: 1, //address: 1 DO1, 2 D02, 3 DO3, 4 DO4, 5 DO5, 6 DO6
    code: 00   //code: 00 关闭, 01 打开
}
```
response:
```
{
    code: 1,  //0、1、2
    data: {
      SN: 123456
    },
    msg: 'use power box'
}
```
**desc:开门操作(控制某路开关)**

request:
```
url: /pay
method: get
data: {
    openid: 'dsafdsaf45',
    total_fee: 100, //单位换算为分
    ip: '129.203.24.101'
}
```
response:
```
{
    code: 0,  //0、1、2
    data: {
        timeStamp: '',
        nonceStr: '',
        signType: 'MD5',
        package: 'prepay_id=xxxx',
        paySign: ''
    }
    msg: 'create unifiedorder ok'
}
```
**desc:小程序支付统一下单**

request:
```
url: /pay/notify
method: post
data: 接收微信反馈信息流
```
**desc:微信官服请求(小程序支付成功后业务处理)**


