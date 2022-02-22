## 邀请码

### banner管理（管理端）



![image-20210126104140092](C:\Users\charlie\AppData\Roaming\Typora\typora-user-images\image-20210126104140092.png)

在banner管理 新增 商户id, banner 登录页统一管理

ALTER  TABLE  dbgrow.tb_bot_banner  ADD  COLUMN   MERCHANT_ID  varchar(32)  DEFAULT '0' COMMENT  '商户id';

### 用户管理(商户端)

1. 新增表

   CREATE TABLE `tb_chnl_bind` (
     `BIND_ID` varchar(32) NOT NULL,
     `PHONE` varchar(20) DEFAULT NULL,
     `CHNL_CODE` varchar(32) DEFAULT NULL,
     `STATUS` char(2) DEFAULT NULL COMMENT '1:正常：0:禁止登陆',
     `DT_UPDATE` datetime DEFAULT NULL,
     `DT_CREATE` datetime DEFAULT NULL,
     PRIMARY KEY (`BIND_ID`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='渠道绑定信息';

   2. 禁用

   http://g.shangjin618.com/mng/index.html#/warn/setting

   ![image-20210126105359521](C:\Users\charlie\AppData\Roaming\Typora\typora-user-images\image-20210126105359521.png)

   通过点击禁用操作，同时更新表tb_user_third:LOGIN_STATUS，tb_chnl_bind :STATUS

   3.  （导入，新增）权限控制

      ```api
      接口 ：third/merchantCreateLogin
      ```

      status：  字段：0,不显示，1， 显示

      **配置管理**（管理端）:

      新增渠道 JSON 配置

      ![image-20210126105939989](C:\Users\charlie\AppData\Roaming\Typora\typora-user-images\image-20210126105939989.png)

   

   **错误码**

   - 首次登陆 `tb_chnl_bind` 表中无数据，返回错误码：3500 ，提示需要输入服务码

   - 当 tb_chnl_bind 字段status 状态为0禁止登陆时，错误码: 3501, 对不起，您的账户目前属于冻结状态

     如有疑问，请联系心理服务工作人员

   - 当 tb_user_third  表通过手机号，渠道查询  不存在用户，返回错误码：3502，账户正确，该渠道下未创建账户

   - 商户下不存在该服务码，返回 3503,服务码错误，请重新输入！