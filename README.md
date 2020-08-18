## Web端sdk开发文档

#### 优势

（1） 埋点开发不用关心通用字段，只需要关心业务字段

（2） 埋点开发代码少，按步骤集成sdk后，业务埋点只需要编写一段代码：

```js
// 埋点开发，只需要编写如下代码：
iask_web.track_event('事件ID'， '事件名称'， '事件类型'， '业务字段')

//demo举例
iask_web.track_event('SE002', "buy", "page", {"price": "365.8", "goodID": 'xxxx-xxxx-xxxx'});
```

（3） 自动收集基础pv事件（即NE001事件），不需要额外开发NE001（重要）


#### 开发步骤

1、 集成SDK
在所有需要埋点的页面，页头添加如下代码

```js
<script src="../lib/iask-web-sdk.js" charset="utf-8"></script>
```

2、配置产品信息

在iask-web-sdk.js文件里面，找到如下代码块，根据业务实际情况配置

```js
// 配置产品信息
var PRODUCT_CONFIG = {
    TERMINAL_TYPE: '0',        // 终端类型
    PRODUCT_NAME: 'ishare',    // 产品名称
    SITE_TYPE: '办公频道',      // 站点类型
    PRODUCT_CODE: '0',         // 产品代码
    PRODUCT_VER: 'V1.0.0',     // 产品版本
    APP_CHANNEL: ''            // app应用渠道渠道
};
```

3、数据上报地址修改

在iask-web-sdk.js文件里面，找到如下代码块，根据业务实际情况配置

```js
var DEFAULT_CONFIG = {
    // 上报服务器域名配置
    'track_url': 'http://localhost:3300/', 
```
4、埋点初始化

所有需要埋点的页面，编写如下代码

```js
iask_web.init('iask_web', {
    local_storage: {
        type: 'localStorage'
    },
    debug: true,
});
```

5、业务埋点开发

具体业务埋点开发，只需编写track_event事件，demo如下：

```js
iask_web.track_event('SE002', "buy", "page", {"price": "365.8", "goodID": '123452'});
```

6、用户登录事件

当用户登录时，必须调用如下代码，以设置userID的值：

```js
iask_web.login(userID);  //userID为具体的用户ID值
```

7、用户登出事件

当用户退出登录时，必须调用如下代码，以清空userID的值：

```js
iask_web.logout();
```

8、访客ID设置事件

当需要设置visitID的值时，需要调用如下代码，以设置visitID的值：

```js
iask_web.setVisitID(visitID);  //visitID为具体的访客ID值
```

9、清空访客ID设置事件

当需要清空visitID的值时，需要调用如下代码：

```js
iask_web.clearVisitID();
```

10、session失效时间设置

在iask-web-sdk.js文件里面，找到如下代码块，自定义sessionID失效时间，默认30分钟

```js
// 会话超时时长，默认30分钟
'session_interval_mins': 30,
```

11、cookie失效时间设置

在iask-web-sdk.js文件里面，找到如下代码块，自定义cookie失效时间，默认1年时间

```js
// cookie方法存储时，配置保存过期时间
'cookie_expiration': 31536000
```


