### Web端SDK开发文档

````
一、本SDK的优势

（1） 自动集成收集通用字段，埋点开发不用关心通用字段，只需要关心业务字段

（2） 埋点开发代码少，业务埋点只需要编写一段代码：

      iask_web.track_event('事件ID'， '事件名称'， '事件类型'， '事件字段')

（3） 自动数据校验，减少测试人员工作量

（4） 自动收集基础pv事件（即NE001事件），不需要额外埋点NE001

（5） 自动上报会话开始时间、会话结束事件：可以直接用于落地页清洗、退出页清洗、访问时长清洗、访问次数指标清洗

（6） 自动上报激活事件，可以用于清洗每天新增用户数指标

（7） 会话失效时间，可灵活配置

（8） 具备用户唯一标识deviceID，解决用户唯一性识别困难

（9） 具备事件唯一标识trackID，解决无法识别重复上报问题

（10） 具备siteType字段，解决主站和办公频道识别困难问题

（11） 解决数据不一致问题：visitID不一致、sessionID不一致等

二、开发步骤

1、 集成SDK

全部html文件，页头添加如下代码

<script src="../lib/iask-web-sdk.js" charset="utf-8"></script>


2、配置产品信息

在iask-web-sdk.js文件里面，找到如下代码块，根据业务实际情况配置

// 配置产品信息,业务方根据具体情况修改
var PRODUCT_CONFIG = {
    TERMINAL_TYPE: '0',        // 终端类型
    PRODUCT_NAME: 'ishare',    // 产品名称
    SITE_TYPE: '办公频道',      // 站点类型
    PRODUCT_CODE: '0',         // 产品代码
    PRODUCT_VER: 'V1.0.0',     // 产品版本
    APP_CHANNEL: ''            // app应用渠道渠道
};


3、数据上报地址修改

在iask-web-sdk.js文件里面，找到如下代码块，根据业务实际情况配置

var DEFAULT_CONFIG = {
    // 上报服务器域名配置
    'track_url': 'http://localhost:3300/', 


4、埋点初始化

如果不需要设置visitID，则使用如下方法：

<script>
    iask_web.init('iask_web', {local_storage: {type: 'localStorage'}});
</script>


如果需要事先设置visitID，则使用如下方法：

iask_web.init('iask_web', {
    local_storage: {
        type: 'localStorage'
    },
    loaded: function (sdk) {
        sdk.set_visit_id(visitID);
    }
});


5、业务埋点开发
完成埋点初始化后，具体业务埋点开发，只需编写track_event事件：

iask_web.track_event('事件ID'， '事件名称'， '事件类型'， '事件字段')

注意：事件ID、事件名称、事件类型为必填参数，如果没有事件字段，事件字段可以不传参

//购买点击事件demo举例
document.getElementsByTagName('h2')[3].onclick = function (e) {
    iask_web.track_event('SE002', "buy", "page", {"price": "365.8", "goodID": 'xxxx-xxxx-xxxx'});
}


//如果没有业务字段，业务字段参数可以不要，demo如下：
document.getElementsByTagName('h2')[3].onclick = function (e) {
    iask_web.track_event('SE002', "buy", "page");
}


6、用户登录事件
当用户登录时，必须调用如下代码，以设置userID的值：

iask_web.login(userID);  //userID为具体的用户ID值


7、用户登出事件
当用户退出登录时，必须调用如下代码，以清空userID的值：

iask_web.logout();


8、访客ID设置事件
当需要设置visitID的值时，在sdk初始化的时候，在loaded钩子函数下，编写sdk.set_visit_id(visitID)方法，代码如下：

var visitID = '265989132567913';
iask_web.init('iask_web', {
    local_storage: {
        type: 'localStorage'
    },
    loaded: function (sdk) {
        sdk.set_visit_id(visitID);
    }
});
````


