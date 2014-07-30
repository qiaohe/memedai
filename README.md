memedai
=======
# 么么贷学生分期 REST API 接口文档

## 修改记录
* 2013-7-30
> * 增加[学生分期注册](#pop_exhibitions)


* 2014-7-29
> 修改接口`PUT`与`DELETE`调用方式为`POST`以方便传统方式调用。
> 增加学生分期修改结果说明

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
Android Pad应用

|必选|名称|类型|说明|
|-|-|-|-|
| * |type|string|必须为android_Pad|
| * |osVersion|string|Android版本|
| * |appVersion|string|应用版本|
| * |token|string|网卡地址|


### 返回值
json格式配置信息

    {
        "assetServer":"http://127.0.0.1:8080/memedai_asset", //资源服务器地址
        "token":"12341234", //Android应用由服务端生成token.
        "upgrade":"http://server:ip/xxx.apk", //Android pad应用自动更新，为空时表示不需要更新
        "upgradeNote":"新版本发布", //Android应用更新说明
    }

[返回目录](#index)
## [推广员登陆](id:recommender_Login)
推广员（沐风线下团队）登陆，使用后台创建的用户进行登陆

### `${app_server}/rest/recommender/login`
|Method|Content-Type|
|-|-|
|get|text/plain|

### 参数
可选参数未设置时不进行更新

|必选|名称|类型|说明|
|-|-|-|-|
| * |userName|string|推广员用户名|
| * |password|string|推广员登陆密码|


### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|登陆成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "key", //1001
            "error"
        ]
    }

[返回目录](#index)
## [更改密码](id:recommender_ChangePassword)
### `${app_server}/rest/recommender/{recommender}/changePwd`
|Method|Content-Type|
|-|-|
|post|application/json|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |recommander|string|推广员名称|
| * |newPwd|string|修改后新密码|

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|修改成功|
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
## [分期申请](id:installment_application)
### `${app_server}/rest/installmentProduct/`
|Method|
|-|
|post|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|接入方唯一标识，用于区分不同的organization和不同的应用接入么么贷平台|
| * |oid|String|是接入系统方提供的申请分期的会员唯一标识，比如会员唯一编码，|
| * |name|String|申请分期人的姓名|
| * |email|String|申请分期人的email地址|
| * |mobile|string|申请分期人的手机号码|
| * |product_Id|string|申请分期产品货号|
| *  |produtName|string|购买的分期产品的商品名称|
| *  |model|string|购买分期产品的型号|
| *  |price|string|产品价格|
| *  |quantity|string|购买产品的数量|
| *  |total|string|产品的总价|

### 返回值
json格式分期申请

    {
        "link":"${app_server}/rest/installmentProduct/1"
        "token":"xxx", //沐风标识
        "oid":-1, //沐分提供的会员的external ID
        "name":"Johnson"
        "mobile":"13671719882"
        "email":"tusc_heqiao@163.com"
        "product_Id":"SKU00000102"
        "productName"："3C产品"
        "model":"型号30101"
        "price":"1800.00"
        "quantity":"30"
        "total":"54000"
    }
    
## [分期列表](id:installment_application_List)
### `${app_server}/rest/installmentProduct/`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|接入方唯一标识，用于区分不同的organization和不同的应用接入么么贷平台|

### 返回值
json格式分期申请

    {
        "token":"xxx", //沐风标识
        "oid":100301, //沐风提供的会员的external ID
        "name":"Johnson"
        "mobile":"13671719882"
        "email":"tusc_heqiao@163.com"
        "product_Id":"SKU00000102"
        "productName"："3C产品"
        "model":"型号30101"
        "price":"1800.00"
        "quantity":"30"
        "total":"54000"
        status:"0" //状态	0登记  1确认 2取消
        createTime:'2013-09-01 09:00:00' //Datetime格式 yyyy-mm-dd hh:MM:ss
    }  
    
     ## [分期搜索](id:installment_application_search)
### `${app_server}/rest/installmentProduct/search
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|接入方唯一标识，用于区分不同的organization和不同的应用接入么么贷平台|
| * |query|string|接入方唯一标识，用于区分不同的organization和不同的应用接入么么贷平台|


### 返回值
json格式分期申请

    {
        "token":"xxx", //沐风标识
        "oid":-1, //沐风提供的会员的external ID
        "name":"Johnson"
        "mobile":"13671719882"
        "email":"tusc_heqiao@163.com"
        "product_Id":"SKU00000102"
        "productName"："3C产品"
        "model":"型号30101"
        "price":"1800.00"
        "quantity":"30"
        "total":"54000"
        status:"0" //状态	0登记  1确认 2取消
        createTime:'2013-09-01 09:00:00' //Datetime格式 yyyy-mm-dd hh:MM:ss
    }  
     
 ## [分期列表（分页）](id:installment_application_List)
### `${app_server}/rest/installmentProduct/`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |token|string|接入方唯一标识，用于区分不同的organization和不同的应用接入么么贷平台|
| * |query|string|查找字符数 kv eg name=johnson query = all 查询所有结果|
| * |page|string|页码 第几页|
| * |size|string|条数 取多少条|
| * |sort|string|排序字段名称，排序方式 sortField：desc,sortField1:asc|

### 返回值
json格式分期申请(分页)

    {
       "pageCount":"10" //总页数，查询结果集的总页数
       size:"20"        //每页显示条数 目前我们我们的定制设备确定
       [ "token":"xxx", //沐风标识
        "oid":-1, //沐风提供的会员的External ID
        "name":"Johnson"
        "mobile":"13671719882"
        "email":"tusc_heqiao@163.com"
        "product_Id":"SKU00000102"
        "productName"："3C产品"
        "model":"型号30101"
        "price":"1800.00"
        "quantity":"30"
        "total":"54000"
        status:"0" //状态	0登记  1确认 2取消
        createTime:'2013-09-01 09:00:00' //Datetime格式 yyyy-mm-dd hh:MM:ss]
        ..
        
    }  
       
## [会员（学生）信息查询](id:memerInfo)
### `${app_server}/rest/member/`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |memberId|string|会员ID （学生的ID号）|

### 返回值
json格式会员基本信息

    {
           "memberid":"10" 
            "name":"Johnson" //学生姓名
            "university":"云南大学" //学校
            "universitIdy":"X123456" //学校编号
            "room_address":"XXXX Road No.1234"
            "home_Adress":"XXXX Road No.223"
            "degree":"master"
            "department":"社科学院"
            "major":"社科系"
            "class": ""
            student_Id:""
            lengthSchool：""
            enrollTime:""
            register:""
            registerTime:""
            imageUrl1:""
            imageUrl2:"http://"
            keyiner:"Johnson"
            createTime:"2012-09-01 09:00:00"
   }
            
 ## [会员（学生）信息查询](id:memerInfo)
### `${app_server}/rest/member/`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |memberId|string|会员ID （学生的ID号）|

### 返回值
json格式会员基本信息

    {
           "memberid":"10" 
            "name":"Johnson" //学生姓名
            "university":"云南大学" //学校
            "universitIdy":"X123456" //学校编号
            "room_address":"XXXX Road No.1234"
            "home_Adress":"XXXX Road No.223"
            "degree":"master"
            "department":"社科学院"
            "major":"社科系"
            "class": ""
            student_Id:""
            lengthSchool：""
            enrollTime:""
            register:""
            registerTime:""
            imageUrl1:""
            imageUrl2:"http://"
            "dcardFront"："http://"
            "idCardBack":""
            "studentCardPage1":""
            "studentCardPage2":""
            keyiner:"Johnson"
            createTime:"2012-09-01 09:00:00"
   }
            
## [修改会员（学生）基本信息](id:memerInfo)
### `${app_server}/rest/member/`
|Method|
|-|
|post|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |memberId|string|会员ID （学生的ID号）|
| * |name|string|学生姓名|
| * |idCardNo|string|学生身份工作号码 522526197405183017|
| * |authorityDate|string|身份证有限期起止时间 2012.08-2032.08|
| * |phone|string|手机号码|
| * |email|string|email地址|
| * |university|string|学校名称|
| * |universityCode|string|学校编码|
| * |room_address|string|宿舍地址|
| * |home_address|string|家庭住址|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|
| * |memberId|string|会员ID （学生的ID号）|


|Http Status Code|错误原因|
|-|-|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //会员目前尚不存在
            ...
        ]
    }


## [上传学生身份证正面](id:upload_idCardFront)
### `${app_server}/rest/member/{id}/idCardFront`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |idCardFront|file|idCard 正面文件 |

### 成功返回值（HTTP statusCode = 200）
json身份证基本信息
    {
           "memberid":"10" 
           "idCardNo":"XXXXXXX"
           "name":"johnson"
           "sex":"Male" //optional Male or Femal
           "birthday":"2008-12-01"
           
   }


### 错误返回值
|Http Status Code|原因|
|-|-|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //修改信息失败，会员不存在
            ...
        ]
    }
    
## [上传学生身份证第一页](id:upload_idCard_back)
### `${app_server}/rest/member/{id}/idCardBack`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |idCardBack|file|idCard 反面文件 |



### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|修改成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //修改信息失败，会员不存在
            ...
        ]
    }

## [上传学生学证第一页](id:upload_studentCard_page1)
### `${app_server}/rest/member/{id}/studentCardPage1`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |studentCardPage1|file|student Card 第一页 |

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|上传成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //学生证信息以及存在
            ...
        ]
    }

## [上传学生学证第二页](id:upload_studentCard_page2)
### `${app_server}/rest/member/{id}/studentCardPage2`
|Method|Content-Type|
|-|-|
|post|multipart/form-data|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |studentCardPage1|file|student Card 第二页 |

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|上传成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //学生证信息以及存在
            ...
        ]
    }

 ## [会员（学生）联系人信息查询](id:contactInfo)
### `${app_server}/rest/member/{memberId}/contact`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |memberId|string|会员ID （学生的ID号）|

### 返回值
json格式学生联系人基本信息列表（Json Array）

    {
        [
           "memberid":"10" 
            "contactId":"1010" //联系人Id
            "name_1":"小张" //姓名
            "relation1":"X123456" //关系1父母 2兄弟姐妹 3其他亲属 4同学 5同事 6朋友 7其他
            "fixedPhone1":"010-000000"
            "mobilePhone1":"13671719882"
             "name_1":"云南大学"
            "relation1":"X123456" 
            "fixedPhone1":"ba ba ba"
            "mobilePhone1":"ba ba ba"
            "register":"Johnson"
            "registerTime":"2014-09-25"
            "createTime":"2014-09-25 09:00:00"],
            ...
   }
  
  ## [会员（学生）联系人添加](id:addContactInfo)
### `${app_server}/rest/member/{id}/contact`
|Method|
|-|
|put|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |name1|string|联系人一姓名|
| * |relation1|string|联系人一关系	1父母 2兄弟姐妹 3其他亲属 4同学 5同事 6朋友 7其他|
| * |fixPhone1|string|联系人一固定电话|
| * |mobilePhone1|string|联系人一手机|
| * |name1|string|联系人二姓名|
| * |relation1|string|联系人二关系	1父母 2兄弟姐妹 3其他亲属 4同学 5同事 6朋友 7其他|
| * |fixPhone1|string|联系人二固定电话|
| * |mobilePhone1|string|联系人二手机|

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|添加学生联系人成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "key", //1001
            "error"
        ]
    }

## [分期列表](id:member_products)
### `${app_server}/rest/member/{memberId}/product`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |memberId|string|么么贷会员ID|

### 返回值
json格式会员（学生）购买分期产品列表

    {
    [
        "product":"12345", 
        "brand":-1, 
        "productName":"Johnson"
        "model":"13671719882"
        "price":"tusc_heqiao@163.com"
        "quantity":"SKU00000102"
        "total"："3C产品"
        ]
        ...
    }  
## [提交分期申请](id:submit_Application)
### `${app_server}/rest/member/{memberid}/application`
|Method|Content-Type|
|-|-|
|post|application/json|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |amount|string|分期金额|
| * |term|string|期数|
| * |apr|string|分期费率|
| * |referenceId|string|沐风App提交分期申请关联ID|

### 成功返回值（HTTP statusCode = 200）
json身份证基本信息
    {
           "applicationNo":"XXXXXXX" //申请书编号
           "applyDate":"johnson"     //申请时间
   }

### 错误返回值
|Http Status Code|原因|
|-|-|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "", //申请已经被取消
            ...
        ]
    }

## [取消分期申请](id:cancel_Application)
### `${app_server}/rest/member/{memberid}/application`
|Method|Content-Type|
|-|-|
|delete|application/json|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |referenceId|string|沐风App提交分期申请关联ID|


### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|取消分期申请成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "", //申请已经被取消
            ...
        ]
    }

## [查看借款协议](id:agreement)
### `${app_server}/rest/member/{memberid}/agreement`
|Method|Content-Type|
|-|-|
|get|txt/html|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |referenceId|string|沐风App提交分期申请关联ID|


### 返回值
Agreement HTML page generated By template

## [确认收货](id:receive_Product)
### `${app_server}/rest/member/{id}/receivedProduct`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |confirmPhoto|file| 本人与货品照片文件|
| * |referenceId|string|沐风App提交分期申请关联ID|

### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|确认收货成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "2001", //申请以及被取消
            ...
        ]
    }
    
 ## [分期还款计划](id:repay_Plan_List)
### `${app_server}/rest/application/{applicationNo}/repayplan`
|Method|
|-|
|get|

### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |applicationNo|string|由么么贷平台生成的申请书编号|


### 返回值
json格式分期还款计划列表

    {
     [
        "dueDate"："2012-09-01"
        "princiapal"："200.03"
        "interest"："30.13"
        "total"："230.16"
     ]
        ..
    } 
    
### 错误返回值
|Http Status Code|原因|
|-|-|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "5001", //当前不需要还款
            ...
        ]
    }
    
## [申请环节--补件](id:application_rfe)
### `${app_server}/rest/member/{id}/application/{applicationNo}/rfe`
|Method|Content-Type|
|-|-|
|post|application/x-www-form-urlencoded|


### 参数
|必选|名称|类型|说明|
|-|-|-|-|
| * |requirement|String|补件要求|
| * |inforamtion|string|补件信息|
| * |file|multipart/form-data|补件照片1|
| * |file|multipart/form-data|补件照片2|
| * |file|multipart/form-data|补件照片3|



### 返回值
|Http Status Code|原因|
|-|-|
|200 - OK|补件收货成功|
|400 - Bad Request|参数错误，详细信息由`ResponseBody`描述|
|500 - Internal Server Error|服务器内部错误，详细信息由`ResponseBody`描述|

ResponseBody

    {
        errors:[
            "6001", //当前提供的补件信息错误。
            ...
        ]
    }
        
    

[返回目录](#index)
