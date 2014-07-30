memedai
=======
# 么么贷学生分期REST API 接口文档

## 修改记录
* 2013-7-30
> * 增加[推荐展会](#pop_exhibitions)


* 2013-6-6
> 修改接口`PUT`与`DELETE`调用方式为`POST`以方便传统方式调用。
> 增加展会修改结果说明

### todo
对接第三方支付



[返回目录](#index)
## [文档参数定义](id:doc_param)
### `${app_server}`
应用服务器，例：  
应用发布至tomcat下memedai_target目录，本机访问时`${app_server}`为`http://127.0.0.1:8080/memedai`  
则`${app_server}/rest` = `http://127.0.0.1:8080/exhibition/rest`

### `${asset_server}`
资源服务器，应用程序启动时应读取全局配置信息设置该值  
资源服务器与应用服务器分别部署，资源文件放置于资源服务器，例：  
资源服务器放置于tomcat下memedai_asset目录，则`${asset_server}/icon/student.png` = `http://127.0.0.1:8080/memedai_asset/icon/a.png`

[返回目录](#index)
## [全局信息](id:config)
应用启动时需要读取全局配置信息
### `${app_server}/rest/configurations/get`
|Method|
|-|
|get|

### 参数
Android pad应用

|必选|名称|类型|说明|
|-|-|-|-|
| * |type|string|必须为android_phone|
| * |osVer|string|Android版本|
| * |ver|string|应用版本|
| * |token|string|网卡地址|


### 返回值
json格式配置信息

    {
        "assetServer":"http://127.0.0.1:8080/memedai_asset", //资源服务器地址
        "token":"12341234", //Android应用由服务端生成token，iOS为空
        "upgrade":"http://server:ip/xxx.apk", //Android pad应用自动更新，为空时表示不需要更新
        "upgradeNote":"新版本发布", //Android应用更新说明
    }

[返回目录](#index)
## [创建、修改展会](id:create_exhibition)
展会的创建与修改使用同一接口，调用时如果`exKey`对应的展会不存在则创建新展会，否则更新已有展会内容

### `${app_server}/rest/exhibitions/put`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|

### 参数
可选参数未设置时不进行更新

|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识，**只能包含字母与数字**|
|   |icon|file|展会图标，**png格式**|
|   |name|string|展会名称|
|   |dateFrom|string|展会开始日期，格式为`yyyy-M-d`，例：2013-1-23|
|   |dateTo|string|展会结束日期，格式为`yyyy-M-d`，例：2013-1-23|
|   |address|string|展会地址|
|   |organizer|string|主办单位|
|   |brief|file|展会简介文件，**html格式**|
|   |schedule|file|展会日程文件，**html格式**|

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|已更新展会|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "", //错误信息
            ...
        ]
    }

[返回目录](#index)
## [删除展会](id:delete_exhibition)
### `${app_server}/rest/exhibitions/delete`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识|

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|已删除展会|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "", //错误信息
            ...
        ]
    }

[返回目录](#index)
## [展会列表](id:find_exhibitions)
### `${app_server}/rest/exhibitions/find`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|手机唯一标识码，用于过滤已报名展会|
| * |size|int|返回记录条数，`size = -1`时返回不分页的所有数据|
| * |last|long|展会创建时间戳，用于不间断滚动，返回记录的创建时间早于此时间。<br>**第一页数据设置为`last = -1`**，第二页设置为第一页最后一条记录的`createAt`字段值|
|   |name|string|展会名关键字，搜索时按此字段值匹配，不设置时不进行过滤|
|   |dateFrom|string|展会开始日期，格式2012-3-4|
|   |dateTo|string|展会结束日期，格式2012-3-4|
|   |exKey|string|按exKey搜索，如果此字段被设置，则忽略name, dateFrom, dateTo|

### 返回值
json格式展会列表

    {
        "name":"xxx", //手机发送请求时的name字段值
        "last":-1, //手机发送请求时的last字段值
        "list":[
            {
                "exKey":"ccbn", //展会标识
                "name":"xxx展会", //展会名称
                "date":"时间：2013年11月13日---2013年11月15日", //展会日期
                "address":"地址：上海国际展览中心", //展会地址
                "organizer":"主办单位：XXX", //主办单位
                "createdAt":1370338650895, //展会创建时间戳
                "applied":"Y", //是否已报名，Y 已报名，N 未报名
                "count":0, //未读消息数
                "status": "A", //审核状态，A 审核通过，P 处理中，D 未通过
            },
            ...
        ]
    }

### 常量定义
以下常量由手机端自行拼接使用  
`${exKey}`展会标识

* `${asset_server}/${exKey}/icon.png`  
展会列表图标

* `${asset_server}/${exKey}/brief.html`  
展会简介页面

* `${asset_server}/${exKey}/schedule.html`  
展会日程页面

* `${asset_server}/${exKey}/qrcode.png`  
展会二维码图片路径

[返回目录](#index)

## [推荐展会](id:pop_exhibitions)
### `${app_server}/rest/exhibitions/pop`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|手机唯一标识码，用于标识已报名展会|
| * |size|int|返回记录条数，`size = -1`时返回不分页的所有数据|
| * |last|int|排序字段，用于不间断滚动，用法类似展会列表中last参数。取值为返回结果中的 orderNo 字段|

### 返回值
json格式展会列表

    {
        "last":-1, //手机发送请求时的last字段值
        "list":[
            {
                "id":3, //推荐 id
                "exKey":"ccbn", //展会标识
                "name":"xxx展会", //展会名称
                "date":"时间：2013年11月13日---2013年11月15日", //展会日期
                "address":"地址：上海国际展览中心", //展会地址
                "organizer":"主办单位：XXX", //主办单位
                "orderNo":123, //推荐列表排序字段，asc
                "applied":"Y", //是否已报名，Y 已报名，N 未报名
                "count":0, //未读消息数
                "status": "A", //审核状态，A 审核通过，P 处理中，D 未通过
            },
            ...
        ]
    }

[返回目录](#index)

## [已报名展会列表](id:find_applied_exhibitions)
### `${app_server}/rest/exhibitions/find_applied`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|手机唯一标识码|

### 返回值
json格式已报名展会列表

    [
        {
            "exKey":"ccbn", //展会标识
            "name":"xxx展会", //展会名称
            "date":"时间：2013年11月13日---2013年11月15日", //展会日期
            "address":"地址：上海国际展览中心", //展会地址
            "organizer":"主办单位：XXX", //主办单位
            "status":"P", //审核状态，P 审核中(Processing)，A 审核通过(Approved)，D 审核未通过(Denied),
            "count":0, //未读消息数
        },
        ...
    ]

## [创建、修改展会新闻](id:create_news)
新闻的创建与修改使用同一接口，调用时如果`newsKey`对应的新闻不存在则创建新闻，否则更新已有新闻内容

### `${app_server}/rest/news/put`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|

### 参数
可选参数未设置时不进行更新

|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识，**只能包含字母与数字**，必须先创建展会|
| * |newsKey|string|新闻标识，**只能包含字母与数字**|
|   |icon|file|新闻图标，**png格式**|
|   |title|string|新闻标题|
|   |content|file|新闻内容文件，**html格式**|

[返回目录](#index)
## [删除新闻](id:delete_news)
### `${app_server}/rest/news/delete`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识|
| * |newsKey|string|新闻标识|

[返回目录](#index)
## [新闻列表](id:find_news)
### `${app_server}/rest/news/find`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |exKey|string|展会标识|

### 返回值
json格式

    {
        "exKey":"ccbn", //展会标识
        "list":[
            {
                "newsKey":"n1s", //新闻标识
                "title":"xxxx", //新闻标题
                "createdAt":1370338650895, //新闻发布时间
            },
            ...
        ]
    }

### 常量定义
以下参数由手机应用定义并自行调用  
`${exKey}`展会标识  
`${newsKey}`新闻标识  

* `${asset_server}/${exKey}/news/${newsKey}.png`  
新闻列表图标

* `${asset_server}/${exKey}/news/${newsKey}.html`  
新闻内容

[返回目录](#index)
## [展会报名](id:apply)
### `${app_server}/rest/applies/put`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |exKey|string|展会标识|
| * |token|string|手机标识|
| * |name|string|姓名|
| * |mobile|string|手机|
| * |email|string|邮箱|
| * |type|string|参展者类型，A 参观展会(Attendee)，E 展示产品(Exhibitor)|

[返回目录](#index)
## [展会报名送审](id:apply_process)
*todo* __由369会网后台提供__

[返回目录](#index)
## [展会审批结果](id:apply_update)
### `${app_server}/rest/applies/update`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识|
| * |token|string|手机标识|
| * |status|string|审核结果<br>A 审核通过(Approved)，D 审核未通过(Denied)|
|   |reason|string|审核变更原因，手机端显示|

[返回目录](#index)
## [审核状态显示](id:apply_status)
手机端显示审核状态页面时，需要先调用接口获得当前展会审核状态
### `${app_server}/rest/applies/get`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |exKey|string|展会标识|
| * |token|string|手机标识|

### 返回值
json格式，手机端通过`status`字段判断显示页面，审核记录存放于`logs`字段

    {
        "exKey":"ccbn", //展会标识
        "status":"N", //审核状态，N 未报名(Not Applied)，P 审核中，A 审核通过，D 未通过
        "logs":[ //审核历史纪录
            "您于xxxx年xx月xx日提交资料",
            "我们对您的资料审核完毕",
            ...
        ]
    }

[返回目录](#index)
## [二维码显示](id:qrcode)
二维码存放于资源服务器，手机端通过拼接图片文件地址访问，如果图片不存在（返回404），则提示用户报名，否则显示图片
### `${asset_server}/${exKey}/qrcode/${token}.png`

[返回目录](#index)
## [展会消息发送](id:msg_send)
iOS消息发送由苹果服务器提供  
Android消息发送由自建消息服务器发送  
消息发送后，服务器更新消息已发送状态，消息列表由服务器提供  
### `${app_server}/rest/messages/send`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |pwd|string|服务器校验密码，防止恶意用户变更数据|
| * |exKey|string|展会标识|
| * |msgKey|string|消息标识|
| * |content|string|消息内容|
|   |token|string|手机标识，不设置时发送给该展会下所有报名人员|

[返回目录](#index)
## [手机消息列表](id:msg_find)
### `${app_server}/rest/messages/find`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |exKey|string|展会标识|
| * |token|string|手机标识|

### 返回值
json格式列表

    {
        "exKey":"ccbn", //展会标识
        "list":[
            {
                "msgKey":"xx", //消息标识
                "content":"请接收二维码", //消息内容
                "createdAt":1370338650895, //消息创建时间
                "read":"Y", //已读状态，N 未读，Y 已读
            }
        ]
    }

[返回目录](#index)
## [已读消息回执](id:msg_read)
手机端在用户读取消息后，向服务器发送已读状态，需要注意离线阅读后在连线时更新
### `${app_server}/rest/messages/read`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |exKey|string|展会标识|
| * |token|string|手机标识|
| * |msgKey|string|消息标识|

[返回目录](#index)
## [消息已读状态更新](id:msg_update_read)
*todo* __由369会网提供调用接口__

[返回目录](#index)
## [消息发送状态更新](id:msg_update_status)
*todo* __由369会网提供__

[返回目录](#index)
