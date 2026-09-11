

---

<!-- 原文定位：绩效互通能力&开发文档.md -->
# 绩效互通能力&开发文档

原文链接：https://alidocs.dingtalk.com/i/nodes/QG53mjyd80Rj9xEaTlxvE6QOV6zbX04v?utm_scene=team_space

# 绩效互通能力&开发文档

> 来源：https://alidocs.dingtalk.com/i/nodes/QG53mjyd80Rj9xEaTlxvE6QOV6zbX04v?utm_scene=team_space
> 知识库路径：智能绩效帮助中心【专业版】

调用接口前需获取安全密钥，请参考下文获取。

[获取安全密钥](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/YQBnd5ExVEwmG2reH0rbjenA8yeZqMmz)


考核接口：

[考核结果数据同步接入文档](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/7NkDwLng8ZM3Epg7HaGMb0wOJKMEvZBY)

[结果值外部结果同步接入文档](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/7NkDwLng8ZM3Epg7HaGMrLeAJKMEvZBY)


OKR接口：

[获取项目列表](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/7NkDwLng8ZM3Epg7HaGe0P0RJKMEvZBY)

[获取周期列表](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/QG53mjyd80Rj9xEaTlxbQqZmV6zbX04v)

[获取周期内目标列表](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/jb9Y4gmKWr7lAED9IQwpXll1VGXn6lpz)

[获取目标下的任务列表](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/Obva6QBXJw9lAEkNFQm6X6eYWn4qY5Pr)

[获取目标操作记录](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/G1DKw2zgV2RXgOEnTBlD2pNvVB5r9YAn)

[更新关键结果进度](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/7NkDwLng8ZM3Epg7HaGME7ZYJKMEvZBY)


---

<!-- 原文定位：绩效互通能力&开发文档/待办回调接入文档.md -->
# 待办回调接入文档

原文链接：https://alidocs.dingtalk.com/i/nodes/kDnRL6jAJM3AE0ngUwD4v9mnWyMoPYe1?utm_scene=team_space

# 待办回调接入文档

> 来源：https://alidocs.dingtalk.com/i/nodes/kDnRL6jAJM3AE0ngUwD4v9mnWyMoPYe1?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### **签名**
```java
/**
* @param secret app的密钥，申请app时给出
* @param timestamp 时间戳
* @param appId appId
* @param corpId 授权的企业corpId
*/
public static String calcSignature1(String secret, long timestamp,
  String appId, String corpId) throws NoSuchAlgorithmException, InvalidKeyException{
  Mac mac = Mac.getInstance("HmacSHA256");
  SecretKeySpec key = new SecretKeySpec(secret.getBytes(), "HmacSHA256");
  mac.init(key);
  mac.update(appId.getBytes());
  mac.update(corpId.getBytes());
  byte[] bytes = mac.doFinal(Long.toString(timestamp).getBytes());
  return Base64.getEncoder().encodeToString(bytes);
}
```

### <span style="color: rgb(37, 39, 42);">**配置HTTP数据推送**</span>

#### <span style="color: rgb(37, 39, 42);">**回调说明**</span>

智能绩效会向第三方企业应用推送订阅的回调事件，目前仅提供考核结果订阅。通过订阅这些事件， 

在绩效内完成考核，结果变更，已完成的考核删除时，会将相关的结果和状态数据推送到第三方企业 

中，以便企业完成自己的业务。目前支持的推送方法是HTTP推送 

#### <span style="color: rgb(37, 39, 42);">**注册回调事件流程**</span>

<span style="color: rgb(37, 39, 42);">回调事件订阅的流程如下图所示。</span> 

<span style="color: rgb(37, 39, 42);">首先，开发者需要在绩效设置页面配置HTTP请求地址用于接收推送的事件数据。在配置完请求地址</span> 

<span style="color: rgb(37, 39, 42);">后，绩效应用会向该地址发送POST请求，只有在规定时间内正确返回了"success"字符串才完成事件</span> 

<span style="color: rgb(37, 39, 42);">订阅。</span> 

![image.png](assets/0efddb6df561c4f16bdea11613b60bcc.jpg)

#### <span style="color: rgb(37, 39, 42);">**配置请求地址和订阅回调事件**</span>

<span style="color: rgb(37, 39, 42);">企业需要完成“申请开放服务”，获取分配的appId和secret，开通企业开放功能，配置HTTP回调</span> 

<span style="color: rgb(37, 39, 42);">URL。当企业订阅的事件触发时，绩效系统会向该网址发送相应的 HTTP POST 请求。</span> 

<span style="color: rgb(37, 39, 42);">开通好企业开放服务后，管理员可以在设置页面查看到开放信息，注册回调时，需要将回调地址配置</span> 

<span style="color: rgb(37, 39, 42);">到页面的“请求网址URL”中</span> 

![image.png](assets/01a7b5a4f1fc1b345c19ca0e93b7d556.jpg)

配置完成后，单击确认/编辑按钮时，系统会向你配置的网址推送一个application/json格式的 POST 

请求, 用于验证你配置的网址的合法性。如下所示：
```json
{
  "corpId": "dingxxxxxx7fe",
  "data": "见 推送数据格式 jsonString",
  "eventName": "CHECK_CALLBACK_URL",
  "recordId": "1ftp6l87d1tmw1e8i1w26c0fd53433jj",
  "signature": "bOYWT3fjQtUQ1bDHmR6pa7UqjGtn4ZrMA0xtVlFy3jg=",
  "timestamp": 1646892063003
}
```

#### <span style="color: rgb(37, 39, 42);">**推送数据说明**</span>
- corpId：企业 id
- data：<span style="color: rgb(37, 39, 42);">推送数据内容，json格式的字符串，根据不同的eventName，内容不同</span>
- eventName：<span style="color: rgb(37, 39, 42);">推送的事件名称</span>
- recordId：<span style="color: rgb(37, 39, 42);">本次推送的recordId</span>
- <span style="color: rgb(37, 39, 42);">signature</span>：<span style="color: rgb(37, 39, 42);">签名，详见接口文档中的签名规则说明</span> 
- timestamp：<span style="color: rgb(37, 39, 42);">时间戳</span>

#### **推送数据格式**

##### **待办**

创建/更新待办
```json
{
  "id": "",
  "eventType": "todo.task.create",
  "title": "",
  "clickType": "",
  "clickTypeName": "",
  "userId": "",
  "url": ""
}
```

完成/删除待办
```json
{
  "id": "",
  "eventType": "todo.task.complete|todo.task.delete",
  "userId": ""
}
```

注:
```java
/**制定*/
MAKE("制定"),
/**评分*/
CHECK_TASK("评分"),
/**确认*/
CHECK("确认"),
/**结果值录入*/
CHECK_RESULT("结果值录入"),
/**审批*/
APPROVAL("审批"),
/**目标确认*/
TARGET_CONFIRM("目标确认"),
/**等级校验*/
CHECK_GRADE("等级校验"),

/**积分制结果值*/
RESULT_UPDATE("积分制结果值"),

   /**结果确认**/
   CONFIRM("结果确认"),
   /**结果申诉**/
   APPEAL("结果申诉"),
   /**结果再次确认**/
   CONFIRM_AGAIN("结果再次确认"),

/**用户调转 */
USER_TRANSFER("用户调转"),

/**修订审批 */
REVISION_REVIEW("修订审批"),

/** 面谈 */
INTERVIEW("面谈"),

/** 360 邀请 14 */
INVITE_COMMENTS("360 邀请"),

/** 360 确认 15 */
INVITE_CONFIRM("360 确认"),

/** 360 评分 16 */
INVITE_EVAL("360 评分"),

   /**OKR任务**/
   OKR_TASK("OKR任务"),

   /**okr目标审批**/
   OKR_APPROVAL("okr目标审批"),

/**指标制定驳回 19*/
MAKE_REJECT("指标制定驳回")
```


---

<!-- 原文定位：绩效互通能力&开发文档/智能绩效互通能力.md -->
# 智能绩效互通能力

原文链接：https://alidocs.dingtalk.com/i/nodes/o14dA3GK8g5N2E7lSKkL0goRV9ekBD76?utm_scene=team_space

# 智能绩效互通能力

> 来源：https://alidocs.dingtalk.com/i/nodes/o14dA3GK8g5N2E7lSKkL0goRV9ekBD76?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### **绩效与AI表格**

#### **功能介绍**

智能绩效与钉钉AI表格打通，可以与钉钉AI表格进行数据互通

包括将钉钉AI表格的数据引入作为考核之用，也可以将考核数据同步至钉钉AI表格进行数据统计和分析

#### **操作说明**

##### **2.1 创建企业内部应用，并开通相应权限**

创建企业内部应用

操作路径：

1、登录钉钉开发者后台 [https://open.dingtalk.com/](https://open.dingtalk.com/)

2、进入应用开发→企业内部应用→钉钉应用，点击创建应用

3、填写相应的应用信息，完成内部应用的创建

![image.png](assets/3ed85b8909b8dd2000a7a28840a24618.jpg)

4、在应用详情页面，点击权限管理，选择开通“AI表格应用读权限”、“AI表格应用写权限”、“通讯录部门信息读权限”、“成员信息读权限”、“通讯录部门成员读权限”

![image.png](assets/ff0ac83a1a444e33c79a97665d4ec5d0.jpg)


5、配置接口权限范围，建议选择“全部员工”，若选择“部分员工”，非权限范围内员工的A表格数据将无法获取，非权限范围内的管理员无法同步AI表格数据。

![image.png](assets/5f8a650f3789252d490bcfc1336714ff.jpg)


6、发布应用

在应用的“版本管理与发布”页面，点击“创建新版本”，发布版本。发布版本之后，对应的配置修改才能生效

![image.png](assets/d231975664d0df7125a1b6efd760f0b4.jpg)


7、获取凭证信息

点击“凭证与基础信息”，记录对应的AppKey和AppSecret，作为开启钉钉AI表格的凭证信息。

![image.png](assets/373ba16ace238678f42c0c75f1245dc0.jpg)


##### **2.2 开启钉钉AI表格数据同步**

操作路径：

1、企业设置→数据连接

2、开启钉钉AI表格数据同步

3、填写之前记录的企业内部应用的AppKey和AppSecret

![image.png](assets/39440f5753c2229a3a5e035062810736.jpg)


5、可提前添加AI表格，只需要填写对应的AI表格链接即可

![image.png](assets/21ae764a93793d9ab6644699e4c55d31.jpg)


##### **2.3 引入钉钉AI表格数据作为考核指标结果值**

1、开启钉钉AI表格数据同步之后，指标设置结果值录入人，可选择“系统”

2、在对应的数据源中，选择“钉钉AI表格”

3、选择对应的AI表格、数据表，以及数据表内对应的字段作为结果值

4、需要设置数据匹配逻辑，即取AI表格中的哪些数据作为被考核人该指标的结果值

5、配置好之后，发起考核，在结果值录入节点，会自动读取AI表格对应的数据作为考核指标的结果值

![image.png](assets/8f146eb67afc6e57bbe9748c28599043.jpg)


##### **2.4 同步绩效数据至钉钉AI表格**

操作路径：

1、在自定义报表页面，点击“同步”按钮，进入“同步至AI钉钉表格”页面

![image.png](assets/e372312d2ba64dee9ccd10aef110a720.jpg)

2、在同步页面，添加同步任务

![image.png](assets/9a92d31619a1360f8d002d017bfbe1de.jpg)


3、新建同步任务，要选择对应的AI表格及数据表，以及对应的自定义报表、对应同步的周期、同步的人员

4、需要做自定义报表和AI表格的数据匹配

5、主键字段设置，想通的主键字段，在同步的时候会选择更新，否则就会新增数据

6、配置好同步任务之后，初始会进行一次全量同步，后续数据更新的时候会自动同步

![image.png](assets/a7ede647ef13cba940a4eeb99fb50201.jpg)


##### **2.5 AI表格数据连接中心【开发中，敬请期待】**

在钉钉AI表格左下角，有一个“数据连接中心”，点击之后，可在数据连接中心找到“智能绩效”的数据插件，点击“使用”，可选择同步对应的数据表，以及要同步的数据范围，设置自动同步，后续对应的数据将会自动同步至钉钉AI表格

注意：使用钉钉AI表格的数据连接中心智能绩效的数据源，需要先开通智能绩效的API接口权限，具体请咨询对应的智能绩效的服务人员

![image.png](assets/93a7e66b484b0184c47f2bc90a7ef4c5.jpg)

![Frame 10000106332.png](assets/ca52ff75b6e802dbb2b4bcc9d8aad4fa.jpg)


### **绩效与考勤**

**应用场景：引入钉钉考勤作为指标结果值。**

想要将钉钉考勤引入作为指标结果值，需要现在“企业设置→数据连接→钉钉考勤数据”开启同步功能。

同步功能开启之后，暂不支持直接关闭，若要关闭可联系工作人员进行关闭。开启之后，绩效并不会直接同步考勤数据，而只是在考核发起结果值录入的时候，如果有指标开启了引入钉钉考勤数据作为结果值，智能绩效会同步对应被考核人的考勤数据

![image.png](assets/a9a74cf478f0b3394f422896ceba01d1.jpg)


开启同步钉钉考勤数据之后，可在考评表中，设置对应的指标的结果值，引入考勤数据。

设置步骤如下：

1、对应考勤指标的结果值录入人选择“系统”

![image.png](assets/20a798e2c13e956755218a49172b53be.jpg)


2、在系统录入结果值弹窗，数据源选择“钉钉考勤数据”

![image.png](assets/931bbc6e026102d9db65dcbffe9a47a4.jpg)


3、然后在数据范围选择对应需要的考勤数据

![image.png](assets/b0a483f71ad9bc68fea3a47c964c5fd4.jpg)


4、然后根据选择的考勤数据，配置对应的计算公式，计算出来的结果作为结果值

![image.png](assets/6618904bc6af472fdf0d83ec48071fa8.jpg)


<span style="color: #FB8C00;">**注意：只读取对应被考核人与考核周期相同时间范围内的考勤数据，因此自定义周期且无起止时间的考核无法获取考勤数据**</span>

### **绩效与**智能薪酬

#### **应用场景：考核结果、绩效等级、绩效系数等数据同步到智能薪酬用于算薪。**

使用说明链接：[同步智能绩效数据使用说明](https://alidocs.dingtalk.com/i/nodes/P7QG4Yx2Jp7mZD2bIkoAgAoEV9dEq3XD?cid=76087812%3A260406729&doc_type=wiki_doc&utm_source=im&utm_scene=team_space&iframeQuery=utm_medium%3Dim_card%26utm_source%3Dim&utm_medium=im_card&corpId=ding41944dc8209e65a835c2f4657eb6378f)


### **绩效的API接口**

#### **应用场景一：绩效考核结果等业务数据对接外部系统；**
1. 订阅、批量查询用户考核结果支持说明文档，见附件；

[考核结果数据同步接入文档2025.08.21.pdf](https://alidocs2.oss-cn-zhangjiakou.aliyuncs.com/res/5VLqXLbvoNRYxqX1/att/d4df058a-4059-48e3-aee7-04197535b467.pdf?Expires=1789103093&OSSAccessKeyId=LTAI5tKTjg4Kq1HCdBJ8qpSp&Signature=d2fPWyHFoiLTp7tDh%2FV0Sef7I7k%3D)

2. 联系智能绩效售后申请开放API接口；
3. API接口开放后，查看路径：设置-企业设置-其他-数据连接-开发者信息![api接口查询路径.png](assets/e76e2c5ded29e004afea234857c33dc1.jpg)


#### **应用场景二：外部系统数据作为指标的结果值引入系统。（规划开发中，敬请期待**[这边请]**）**


### **A1面谈小助理**

#### **快速了解**

钉钉 AI 面谈小助理是<span style="color: #1564DC;">**DingTalk A1**</span>与<span style="color: #FB8C00;">**智能绩效**</span>强强联手打造的一款软硬件结合产品，通过<span style="color: #D16A15;">自动记录面谈沟通对话，自动生成 AI 面谈纪要、待办任务与分析报告</span>，并无缝链接智能绩效，智能生成绩效面谈和改进计划，极大提升日常面谈效率。

![image.png](assets/8e2c32f704647ff0ca790ddb79e8a0c6.jpg)


#### **核心功能与价值**

:::
<span style="color: linear-gradient(152deg, #C676FF 0%, #654CFF 41%, #405EFF 75%, #007FFF 99%);">**1、DingTalkA1 - 随时随地无感记录沟通信息**</span>

**支持通话录音**外放录音和通话录音都自动支持，无需操作切换；
**支持面对面沟通翻译**不管是客户到公司还是参加展会，都可以面对面相互翻译，进行无障碍沟通；
**磁吸手机无感随身携带**磁吸手机后背携带，无需单独携带，随时开启沟通记录，是一款 AI 手机伴侣；
**60 天超长待机**个人使用无需担心频繁充电使用，超长 60 天待机，可连续录制 45 小时以上；660 毫安电池
:::

:::
**支持 30\+AI 总结模板**各种会议类型都支持针对性总结模板，不同类型的会议使用不同模板，生成总结更精准；
**生成钉钉待办**会议总结生成的内容支持转为钉钉待办，让会议待处理任务继续到人推进解决；
**跟随会议日程自动录音**可跟随钉钉日程，开启后可以日程开始后同步自动开启录音，结束后停止录音；
**自动生成会议纪要**默认自动生成会议纪要，无需其他操作，纪要生成后消息自动通知，点击查看和分享即可；
:::

:::
<span style="color: linear-gradient(152deg, #C676FF 0%, #654CFF 41%, #405EFF 75%, #007FFF 99%);">**2、A1面谈小助理 - 让 AI 参与每次面谈并输出专业分析与洞察**</span>

**智能生成面谈总结**不仅仅输出面谈纪要，还可以针对面谈过程输出专业的面谈分析报告，让 AI 和你一起参与面谈并给出建议；
**无缝衔接智能绩效**绩效面谈过程不再需要花精力记录整理，专注与被考核人进行沟通即可，会后自动生成面谈总结和改进计划；
**提升面谈效率**一小时面谈过程 5 分钟完成全部分析解读，快速输出面谈分析报告，同步支持企业自定义分析指标，高效完成每一次面谈；
**适配更多场景**不仅支持绩效面谈，过程辅导、晋升/转正面谈、调岗沟通、离职面谈等场景均适配。
:::


#### **核心操作**

##### **3.1 新产品激活**

第一步：长按设备录音键 3 秒开机，或通过 Type-c 充电线连通电源开机![image.png](assets/f8eb3161b8aaccccb3dbb21f59fedf8a.jpg)长按 3 秒录音键即可开机
第二步：拆开包装，快速启用 卡片可以看到产品激活码；![image.png](assets/28db93eb07f538802a211aaacd05c557.jpg)激活卡片示意图
第三步：下载安装钉钉，登陆后打开点击右上角扫一扫，扫描专属激活码即可![image.png](assets/083c3e624c8058dfffa5cf4a00ed4054.jpg)扫一扫产品专属激活码

第四步：开始扫描查找设备，选择对应的设备连接：![蓝牙扫描_识别设备@1x (2).png](assets/3d41605cc71c3f17234cc8534c64150f.jpg)
第五步：选择绑定组织（如果没有组织可以创建一个，后续钉钉功能依赖组织）![绑定组织@1x (2).png](assets/5d6fb6810daad04de24f9a37af51d7dd.jpg)
第六步：看到祝贺弹窗就表示完成激活绑定，可以正常使用了![激活完成弹窗@1x (1).png](assets/372c6fbae3b67d1936cebb97d8b277c8.jpg)

##### **3.2 录音记录**

设备端长按设备录音键 2 秒开始或停止录音；APP 端通过单击录音按钮开始或停止录音；

| **设备端操作指导** | **APP 端操作指导** |
|-------------------------|-----------------------|
| **A1 旗舰版指导：**<br>长按设备录音键 2 秒开始或停止录音<br>![image.png](assets/f8eb3161b8aaccccb3dbb21f59fedf8a.jpg) | **APP 端 开始录音操作指导：** |

##### **3.3 AI 面谈分析报告**

**1）开启A1面谈助理，需要首先在智能绩效应用内进行设置：**

首先需要在企业设置→数据连接中，开启钉钉A1![image.png](assets/94b99d353696be47b95fc9f23db32b5a.jpg)
在面谈模板中设置面谈参与人是否允许使用A1![image.png](assets/b60fb96b05d11fdff9533429f62e48ba.jpg)

<span style="color: #FFA726;">注意：并不是所有的人在面谈的时候可以使用钉钉A1，只有在模板中开启了“允许使用钉钉A1”，对应的面谈参与人才可以使用A1面谈小助理提交面谈内容。</span>

**2）A1 支持 AI 面谈小助理对记录的内容进行自动化分析，分析结果会自动同步至智能绩效系统：**

1、在A1录音文件详情页访问——分析![IMG_2726.png](assets/50ee963c34bf67ab23fc109a2be147aa.jpg)
2、点击设置按钮打开后，开启「 A1面谈助理」![image.png](assets/d06b0b72893a532ccff884666ca685a7.jpg)
3、开启后，关联对应的考核周期![image.png](assets/fb4f6791d98d8c1728458068c9a1b42f.jpg)
4、生成绩效面谈总结（支持对生成的面谈分析进行编辑）![/Users/ailikesi/Downloads/修改文字内容.png](assets/0b50d1aad734e6d4d3451a32893b49a9.jpg)

**3）支持移动端绩效面谈直接唤起A1**

1、在移动端填写面谈页面点击「开始A1录音」![image.png](assets/3b7f68e69e7dd6567794346b70fd4656.jpg)
2、第二步：成功连接A1后，点击「开始录音」![image.png](assets/5f5e7164dd8bf10d105a99883484e4cf.jpg)
3、录音完成，系统自动生成面谈总结![image.png](assets/5030890e292da3439f5c67806bbce866.jpg)
4、内容确认后，点击「插入A1分析内容」![image.png](assets/2268800241adbd20a7126f9e9be8b2f2.jpg)


---

<!-- 原文定位：绩效互通能力&开发文档/更新关键结果进度.md -->
# 更新关键结果进度

原文链接：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGME7ZYJKMEvZBY?utm_scene=team_space

# 更新关键结果进度

> 来源：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGME7ZYJKMEvZBY?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com/openapi/perf-okr/objective/changeProgress
```

### 请求方法

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

#### Body

| **名称** | **类型** | **是否必须** | **默认值** | **备注** |
|----------|----------|----------------|-------------|----------|
| krId | string | 必须 |  | 关键结果id |
| currentValue | double | 必须 |  | 当前值 |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object</span> |  |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |


---

<!-- 原文定位：绩效互通能力&开发文档/结果值外部结果同步接入文档.md -->
# 结果值外部结果同步接入文档

原文链接：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGMrLeAJKMEvZBY?utm_scene=team_space

# 结果值外部结果同步接入文档

> 来源：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGMrLeAJKMEvZBY?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 具体逻辑
1. 需要先获取考核周期列表，参考/openapi/perf/findCheckRsQueryList[考核结果数据同步接入文档](https://alidocs.dingtalk.com/i/p/lPDmrWQYAAe9dGxd/docs/7NkDwLng8ZM3Epg7HaGMb0wOJKMEvZBY)
2. 获取可以填写的结果值数据（选择了外部录入的指标），按照被考核人和指标的纬度，查询到可以操作的数据，满足条件为被考核人当前在结果值确认中，并且结果值确认中的指标为外部录入，并且外部录入的数据没有执行完成或者被跳过
3. 执行提交分数接口，进行结果值外部数值写入，提交内容包含操作类型，暂存或者提交
4. 支持外部录入的跳过，和驳回，外部录入的数值只有通过接口可以完成提交

### 接口请求域名：

token获取以及签名计算规则，参考其他文档
```DEFAULT_LANGUAGE
https://oapi.dingteam.com
```

### 接口部分

---


##### <span style="background-color: #91D5FF;">在结果值节点外部录入列表</span>

获取到token之后，放在其他接口的请求Header中：x-dingteam-access-token：your token

**请求方式：**POST

**接口地址：**<span style="color: rgb(33, 33, 33);">/perf/findResultInputStep</span>

**POST请求包结构体：**

| **Headers** |  |  |  |
|-----------|---|---|---|
| **参数名称** | **参数值** | **是否必须** | <span style="background-color: #FFFFFF;">**备注**</span> |
| x-dingteam-access-token | 企业凭证token | 是 | 企业凭证接口获取 |
```DEFAULT_LANGUAGE
{
    "queryIds": [
        "1-2022-M2",
        "1-2023-M8",
        "1-2022-M1",
    ],
    "userIds": [
        "01556612501306099"
    ]
}
```

| body参数说明 |  |
|----------------|---|
| queryIds | 周期数组，最大长度不能超过10个 |
| userIds | 查询的考核人员id，最大长度不能超过50 |
```js
{
    "code": 0,
    "msg": "成功",
    "data": [
        {
            "queryId": "2-2021-Q2",
            "userId": "01556612501306099",
            "checkTitle": "2021年第2季度绩效考核",
          	"targetList":[{
              	"targetId":"1gdn3jmov57vwlsiu1w21skmsb20i4jr",
              	"targetName":"本月销售额",
              	"remark":"完成率<60%，不得分",
              	"targetCode":""
            },
            {
              	"targetId":"1gdn3jmov57vwlsiurykhgvdsfyjdagd",
              	"targetName":"日常工作细则或违规",
              	"remark":"根据公司规章制度或部门操作细则执行",
              	"targetCode":"1232dsafdsadsiurykhgvdsfyjdagd"
            }]
           
        },
        {
            "queryId": "1-2030-M6",
            "userId": "01556612501306099",
            "checkTitle": "2030年06月绩效考核",
            "targetList":[{
              	"targetId":"1gdn3jmov57vwlsiudsadsadsadtrkkop",
              	"targetName":"本月销售额",
              	"remark":"完成率<60%，不得分",
              	"targetCode":""
            },
            {
              	"targetId":"1gdn3jmov57vwlsikllkfdopaskghpjdp",
              	"targetName":"日常工作细则或违规",
              	"remark":"根据公司规章制度或部门操作细则执行",
              	"targetCode":""
            }]
        }
    ],
    "errorData": null,
    "traceId": "1ftpbtuac1v2w89w26u7fis2cu88ee3p"
}
```


##### <span style="background-color: #91D5FF;">提交外部录入的结果值</span>

获取到token之后，放在其他接口的请求Header中：x-dingteam-access-token：your token

**请求方式：**POST

**接口地址：**<span style="color: rgb(33, 33, 33);">/perf/executeInputResultBatch</span>

**POST请求包结构体：**

| **Headers** |  |  |  |
|-----------|---|---|---|
| **参数名称** | **参数值** | **是否必须** | <span style="background-color: #FFFFFF;">**备注**</span> |
| x-dingteam-access-token | 企业凭证token | 是 | 企业凭证接口获取 |

**请求参数说明：**

要求外部录入的指标，在本次录入时，必须全部填充，否则无法完成提交和通过执行中

| Object\[\] |  |  |  |  |
|----------|---|---|---|---|
|  | tyoe | 提交类型 | 支持 submit |  |
|  | <span style="color: rgb(18, 20, 22);">inputResultDataList</span> | 结果值录入对象数组 | Object\[\] |  |
|  | <ul><li><span style="color: rgb(18, 20, 22);">queryId</span></li></ul> | 周期id | String |  |
|  | <ul><li><span style="color: rgb(18, 20, 22);">rsUserId</span></li></ul> | 被考核人userId | String |  |
|  | <ul><li><span style="color: rgb(18, 20, 22);">targetResultValues</span></li></ul> | 录入指标明细 | Object\[\] |  |
|  | -- <span style="color: rgb(18, 20, 22);">targetId</span> | 指标id | String |  |
|  | -- <span style="color: rgb(18, 20, 22);">itemResultValue</span> | 结果值 | String |  |
|  | -- <span style="color: rgb(18, 20, 22);">itemResultRemark</span> | 结果值说明 | String |  |
```js
{
    "type":"submit",
    "inputResultDataList":[{

        "queryId":"7-0-Y2022M11D9/Y2022M11D10-2022-11-09至2022-11-10绩效考核",
        "rsUserId":"01151652685998",
        "targetResultValues":[{
            "targetId":"1ghdonvru77aw1e975w486lml3rj9igb",
            "itemResultValue":"100",
            "itemResultRemark":"外部2，提交后过节点"
        }]
    }]

}
```

**返回值：**
```js
{
    "code": 0,
    "msg": "成功",
    "data": true,
    "errorData": null,
    "traceId": "1ghdou1eq77ow1gw28c0g13380uq95u3"
}
```


---

<!-- 原文定位：绩效互通能力&开发文档/考核结果数据同步接入文档.md -->
# 考核结果数据同步接入文档

原文链接：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGMb0wOJKMEvZBY?utm_scene=team_space

# 考核结果数据同步接入文档

> 来源：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGMb0wOJKMEvZBY?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

#### 签名计算规则

java计算方式 
```DEFAULT_LANGUAGE
/**
 * @param secret    app的密钥，申请app时给出
 * @param timestamp 时间戳
 * @param appId     appId
 * @param corpId    授权的企业corpId
 */
public static String calcSignature1(String secret,
                                    long timestamp,
                                    String appId,
                                    String corpId)
             throws NoSuchAlgorithmException, InvalidKeyException{
    
    Mac mac = Mac.getInstance("HmacSHA256");
    SecretKeySpec key = new SecretKeySpec(secret.getBytes(), "HmacSHA256");
    mac.init(key);
    mac.update(appId.getBytes());
    mac.update(corpId.getBytes());
    byte[] bytes = mac.doFinal(Long.toString(timestamp).getBytes());
    return Base64.getEncoder().encodeToString(bytes);
}
```

#### 接口请求域名：
```DEFAULT_LANGUAGE
https://oapi.dingteam.com
```

### 配置HTTP数据推送

#### 回调说明


智能绩效会向第三方企业应用推送订阅的回调事件，目前仅提供考核结果订阅。通过订阅这些事件，在绩效内完成考核，结果变更，已完成的考核删除时，会将相关的结果和状态数据推送到第三方企业中，以便企业完成自己的业务。目前支持的推送方法是HTTP推送

#### 注册回调事件流程

回调事件订阅的流程如下图所示。

首先，开发者需要在绩效设置页面配置HTTP请求地址用于接收推送的事件数据。在配置完请求地址后，绩效应用会向该地址发送POST请求，只有在规定时间内正确返回了"success"字符串才完成事件订阅。


![](assets/6d0be8c9c23533ad1ad982984d08683c.jpg)

#### 配置请求地址和订阅回调事件

企业需要完成“申请开放服务”，获取分配的appId和secret，开通企业开放功能，配置HTTP回调URL。当企业订阅的事件触发时，绩效系统会向该网址发送相应的 HTTP POST 请求。

开通好企业开放服务后，管理员可以在设置页面查看到开放信息，注册回调时，需要将回调地址配置到页面的“请求网址URL”中

![](assets/c5b1641ebd1fa5ca76540e1473931275.jpg)


配置完成后，单击**确认/编辑**按钮时，系统会向你配置的网址推送一个application/json格式的 POST请求, 用于验证你配置的网址的合法性。如下所示：
```DEFAULT_LANGUAGE

{
  "corpId": "dingxxxxxx7fe",
  "data": "null",
  "eventName": "CHECK_CALLBACK_URL",
  "recordId": "1ftp6l87d1tmw1e8i1w26c0fd53433jj",
  "signature": "bOYWT3fjQtUQ1bDHmR6pa7UqjGtn4ZrMA0xtVlFy3jg=",
  "timestamp": 1646892063003
}
```

#### 推送数据说明
- corpId	:	企业id
- data	:	推送数据内容，json格式的字符串，根据不同的eventName，内容不同
- eventName	：	推送的事件名称
- recordId	:	本次推送的recordId
- signature	：签名，详见接口文档中的签名规则说明
- timestamp	：	时间戳


#### 数据格式

#####   CHECK\_CALLBACK\_URL ：验证回调地址


1. 注册回调地址时，立即触发，返回期望内容后，完成回调地址的注册


##### CHECK\_RS\_RESULT ： 推送考核结果
1. 考核结果产生，结果调整，删除，重制重新产生结果后，立即触发一次数据同步
2. 主动触发一次数据同步失败后，系统将在1-3分钟内，完成三次重试


推送data数据格式：json格式字符串
```DEFAULT_LANGUAGE
{
	"checkIsDelete": false,
    "recordType": "1",
	"checkTitle": "2022年08月绩效考核",
    "beginDate": "1646892063003",
    "endDate": "1646892063003",
	"perfGrade": "合格 7<x<8",
	"perfFactor": "1.2",
	"quantizeScore": "300",
    "behaviorScore": "70",
    "recordState": "1",
	"queryId": "1-2022-M8",
	"userId": "0635116061639258"
}
```


 推送data数据说明
- checkIsDelete：是否删除
- <span style="color: #E53935;">recordType</span>：<span style="color: #E53935;">推送的考核类型，为空默认是人员考核</span>
- checkTitle：考核标题
- <span style="color: #E53935;">beginDate</span>：<span style="color: #E53935;">时间戳，自定义类型可能为空</span>
- <span style="color: #E53935;">endDate</span>：<span style="color: #E53935;">时间戳，自定义类型可能为空</span>
- perfGrade：绩效等级
- <span style="color: #E53935;">perfFactor</span>：<span style="color: #E53935;">绩效系数</span>
- quantizeScore：量化分
- behaviorScore：行为分
- <span style="color: #E53935;">recordState：考核状态</span>
- <span style="color: ;">queryId：考核周期</span>
- <span style="color: #E53935;">userid：如果是人员考核，返回的就是人员id；如果是部门考核，返回的就是部门id</span>


| <span style="color: #E53935;">recordState：考核状态</span> |  |
|---------------------------------------------------------------|---|
| 1 | 进行中 |
| 2 | 已完成（所有节点都已完成） |
| 3 | 已停止（考核异常终止） |


| <span style="color: #E53935;">recordType：参数说明</span> |  |
|--------------------------------------------------------------|---|
| 1 | 人员考核 |
| 2 | 部门考核 |


### 考核结果查询

#### 说明

用来补偿推送不成功时，第三方主动调用接口，查询用户的考核结果

#### 接口部分

---

###### <span style="background-color: #91D5FF;">获取企业凭证</span>

获取到token之后，放在其他接口的请求Header中：x-dingteam-access-token：your token

**请求方式：**POST

**接口地址：**/openapi/auth/getToken

**POST请求包结构体：**
```DEFAULT_LANGUAGE
{
  "appId":"123",
  "corpId":"233",
  "timestamp":1580009901,
  "signature":"sadsadhgkdhsajkdhskajcisa"
}
```

| **请求参数说明** |  |  |  |
|----------------------|---|---|---|
| **参数名称** | **参数类型** | **是否必须** | <span style="background-color: #FFFFFF;">**备注**</span> |
| appId | String | 是 | 分配的APPID |
| corpId | String | <span style="color: #404040;">是</span> | 企业id |
| timestamp | Date | <span style="color: #404040;">是</span> | 时间戳 |
| signature | Sreing | <span style="color: #404040;">是</span> | 签名串 |

**请求参数说明：**

| **返回数据说明** |  |  |
|----------------------|---|---|
| **参数名称** | **类型** | <span style="background-color: #FFFFFF;">**备注**</span> |
| code | String | 返回的响应码 |
| msg | String | 响应码对应的消息 |
| data | String | 返回的数据 |
| -token | String | 授权企业调用接口的token |
| -expiredTime | Number | token有效期 |

---

###### <span style="background-color: #91D5FF;">获取企业考核周期</span>

获取到token之后，放在其他接口的请求Header中：x-dingteam-access-token：your token

**请求方式：**POST

**接口地址：**/openapi/perf/findCheckRsQueryList

**POST请求包结构体：**

| <span style="color: #E03E3E;">**Headers**</span> |  |  |  |
|------------------------------------------------|---|---|---|
| **参数名称** | **参数值** | **是否必须** | <span style="background-color: #FFFFFF;">**备注**</span> |
| x-dingteam-access-token | 企业凭证token | 是 | 企业凭证接口获取 |
```DEFAULT_LANGUAGE
{
    “recordType”:"1"
    "checkType":"1"
}
```


数据说明
- <span style="color: #E53935;">recordType：考核类型</span>
- checkType：周期类型


| <span style="color: #E53935;">recordType：参数说明</span> |  |
|--------------------------------------------------------------|---|
| 1 | 人员考核 |
| 2 | 部门考核 |

| checkType：参数说明 |  |
|------------------------|---|
| 1 | 月度 |
| 2 | 季度 |
| 3 | 半年度 |
| 4 | 年度 |
| 5 | 试用期 |
| 6 | 日度 |
| 7 | 自定义 |
```DEFAULT_LANGUAGE
{
	"code": 0,
	"msg": "成功",
	"data": [{
			"queryId": "1-2030-M4",
			"name": "2030年04月绩效考核"
            "beginDate": "1646892063003"
            "endDate": "1646892063003"  
            
		},
		{
			"queryId": "1-2010-M8",
			"name": "2010年08月绩效考核"
            "beginDate": "1646892063003"
            "endDate": "1646892063003"
		},
		{
			"queryId": "1-2030-M9",
			"name": "2030年09月绩效考核"
            "beginDate": "1646892063003"
            "endDate": "1646892063003"
		},
		{
			"queryId": "1-2037-M4",
			"name": "2037年04月绩效考核"
            "beginDate": "1646892063003"
            "endDate": "1646892063003"
		}
	],
	"errorData": null,
	"traceId": "1ftxxxxxx2vhe"
}
```


#### Date数据说明
- queryId：考核周期
- name：考核周期名称
- <span style="color: #E53935;">beginDate</span>：<span style="color: #E53935;">时间戳，自定义类型可能为空</span>
- <span style="color: #E53935;">endDate</span>：<span style="color: #E53935;">时间戳，自定义类型可能为空</span>


###### <span style="background-color: #91D5FF;">批量查询用户考核结果</span>

获取到token之后，放在其他接口的请求Header中：x-dingteam-access-token：your token

**请求方式：**POST

**接口地址：**/openapi/perf/findCheckRsResult

**POST请求包结构体：**

| **Headers** |  |  |  |
|-----------|---|---|---|
| **参数名称** | **参数值** | **是否必须** | <span style="background-color: #FFFFFF;">**备注**</span> |
| x-dingteam-access-token | 企业凭证token | 是 | 企业凭证接口获取 |
```DEFAULT_LANGUAGE
{
    "queryIds": [
        "1-2022-M2",
        "1-2023-M8",
        "1-2022-M1",
    ],
    "userIds": [
        "01556612501306099"
    ]
}
```

| body参数说明 |  |
|----------------|---|
| queryIds | 周期数组，最大长度不能超过10个 |
| userIds | 查询的考核人员id/部门id，最大长度不能超过50 |
```json
{
    "code": 0,
    "msg": "成功",
    "data": [
        {
            "queryId": "2-2021-Q2",
            "userId": "01556612501306099",
            "checkTitle": "2021年第2季度绩效考核",
            "recordState": "1",
            "perfGrade": "合格 7<x<8",
            "perfFactor": "1.2",
            "quantizeScore": "900.00",
            "behaviorScore": null,
            "perfGrade": "萨满博尔赫斯pinkfol",
            "checkIsDelete": false,
            "stepTaskModels": [
                          {
                              "targetId": null,  
                              "stepId": "1h65ambf8hiwgv59w1vnvlap26tiamn1",  // 大节点ID
                              "weight": null,
                              "taskId": "1h65ambobhiwgv5pw3pg9dns3bbqkni3",  // 小节点ID
                              "checkStep": 105,   // 节点类型
                              "title": "评分",    // 节点名称
                              "userId": "5067184826464388",  // 评分人ID
                              "name": "梁孟珂",    // 评分人名称
                              "deptName": "无敌2",   // 评分人部门
                              "avatar": "",   
                              "score": "   66",   // 分数
                              "illustration": "不不不不不不不不不",   // 评分说明
                              "fileEntityList": null,
                              "grade": null,
                              "scoreAuth": null,
                              "scoreInfoAuth": null,
                              "isRead": null,
                              "createdate": 1690248622000     // 时间
                          },
                          *****
            ]
        },
        {
            "queryId": "1-2030-M6",
            "userId": "01556612501306099",
            "checkTitle": "2030年06月绩效考核",
            "recordState": "1",
            "perfGrade": "合格 7<x<8",
            "perfFactor": "1.2",
            "quantizeScore": "11",
            "behaviorScore": "2",
            "perfGrade": "C",
            "checkIsDelete": false,
             "stepTaskModels": []
    ],
    "errorData": null,
    "traceId": "1ftpbtuac1v2w89w26u7fis2cu88ee3p"
}
```

| 返回字段说明 |  |
|------------------|---|
| queryId | 考核周期 |
| <span style="color: #E53935;">userId</span> | <span style="color: #E53935;">考核用户userId/部门id</span> |
| <span style="color: #E53935;">recordState</span> | <span style="color: #E53935;">考核状态</span> |
| checkTitle | 考核标题 |
| perfGrade | 绩效等级 |
| <span style="color: #E53935;">perfFactor</span> | <span style="color: #E53935;">绩效系数</span> |
| quantizeScore | 量化分 |
| behaviorScore | 行为分 |
| checkIsDelete | 是否删除 |


---

<!-- 原文定位：绩效互通能力&开发文档/获取周期内目标列表.md -->
# 获取周期内目标列表

原文链接：https://alidocs.dingtalk.com/i/nodes/jb9Y4gmKWr7lAED9IQwpXll1VGXn6lpz?utm_scene=team_space

# 获取周期内目标列表

> 来源：https://alidocs.dingtalk.com/i/nodes/jb9Y4gmKWr7lAED9IQwpXll1VGXn6lpz?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com /openapi/perf-okr/objective/findAllWithKrs
```

### 请求方法

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

#### Body

| **名称** | **类型** | **是否必须** | **默认值** | **备注** |
|----------|----------|----------------|-------------|----------|
| okrId | string | 必须 |  | 周期id |
| pageNo | string | 必须 |  |  |
| pageSize | string | 非必须 |  | 最多40条每页（不指定默认40条） |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object\[\]</span> |  |
| <span style="color: #262626;">\- pageNo</span> | <span style="color: #262626;">numver</span> |  |
| <span style="color: #262626;">\- pageSize</span> | number |  |
| <span style="color: #262626;">\- list</span> | object\[\] |  |
| - id | string | 目标id |
| -  name | string | 目标名称 |
| <span style="color: #262626;">- type</span> | number | 目标类型 1公司级 2部门级 3个人级 |
| <span style="color: #262626;">- progress</span> | number | 进度 |
| <span style="color: #262626;">- owner</span> | object |  |
| <span style="color: #262626;">- userId</span> | string | 负责人id |
| <span style="color: #262626;">- name</span> | string | 负责人姓名 |
| <span style="color: #262626;">- deptNames</span> | string\[\] | 所在部门list |
| <span style="color: #262626;">- krs</span> | object\[\] |  |
| <span style="color: #262626;">- id</span> | string | 关键结果id |
| <span style="color: #262626;">- name</span> | string | 关键结果名称 |
| <span style="color: #262626;">- progress</span> | number | 关键结果进度 |
| <span style="color: #262626;">- weight</span> | number | 权重 |
| <span style="color: #262626;">- createdAt</span> | date | 创建时间 |
| <span style="color: #262626;">- totalPages</span> | number |  |
| <span style="color: #262626;">- totalCount</span> | number |  |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |


---

<!-- 原文定位：绩效互通能力&开发文档/获取周期列表.md -->
# 获取周期列表

原文链接：https://alidocs.dingtalk.com/i/nodes/QG53mjyd80Rj9xEaTlxbQqZmV6zbX04v?utm_scene=team_space

# 获取周期列表

> 来源：https://alidocs.dingtalk.com/i/nodes/QG53mjyd80Rj9xEaTlxbQqZmV6zbX04v?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com /openapi/perf-okr/main/findAll
```

### 请求方法

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object\[\]</span> |  |
| <span style="color: #262626;">\- id</span> | <span style="color: #262626;">string</span> | <span style="color: #262626;">周期id</span> |
| <span style="color: #262626;">\- name</span> | <span style="color: #262626;">string</span> | <span style="color: #262626;">周期名称</span> |
| <span style="color: #262626;">\- type</span> | <span style="color: #262626;">number</span> | <span style="color: #262626;">周期类型 1月度 2季度 3半年度 4年度 5自定义</span> |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |


---

<!-- 原文定位：绩效互通能力&开发文档/获取安全密钥.md -->
# 获取安全密钥

原文链接：https://alidocs.dingtalk.com/i/nodes/YQBnd5ExVEwmG2reH0rbjenA8yeZqMmz?utm_scene=team_space

# 获取安全密钥

> 来源：https://alidocs.dingtalk.com/i/nodes/YQBnd5ExVEwmG2reH0rbjenA8yeZqMmz?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

#### 签名计算规则

java计算方式 
```DEFAULT_LANGUAGE
/**
 * @param secret    app的密钥，申请app时给出
 * @param timestamp 时间戳
 * @param appId     appId
 * @param corpId    授权的企业corpId
 */
public static String calcSignature1(String secret,
                                    long timestamp,
                                    String appId,
                                    String corpId)
             throws NoSuchAlgorithmException, InvalidKeyException{
    
    Mac mac = Mac.getInstance("HmacSHA256");
    SecretKeySpec key = new SecretKeySpec(secret.getBytes(), "HmacSHA256");
    mac.init(key);
    mac.update(appId.getBytes());
    mac.update(corpId.getBytes());
    byte[] bytes = mac.doFinal(Long.toString(timestamp).getBytes());
    return Base64.getEncoder().encodeToString(bytes);
}
```

#### 接口请求域名：
```DEFAULT_LANGUAGE
https://oapi.dingteam.com
```

### 配置HTTP数据推送

#### 回调说明


智能绩效会向第三方企业应用推送订阅的回调事件，目前仅提供考核结果订阅。通过订阅这些事件，在绩效内完成考核，结果变更，已完成的考核删除时，会将相关的结果和状态数据推送到第三方企业中，以便企业完成自己的业务。目前支持的推送方法是HTTP推送

#### 注册回调事件流程

回调事件订阅的流程如下图所示。

首先，开发者需要在绩效设置页面配置HTTP请求地址用于接收推送的事件数据。在配置完请求地址后，绩效应用会向该地址发送POST请求，只有在规定时间内正确返回了"success"字符串才完成事件订阅。


![](assets/6d0be8c9c23533ad1ad982984d08683c.jpg)

#### 配置请求地址和订阅回调事件

企业需要完成“申请开放服务”，获取分配的appId和secret，开通企业开放功能，配置HTTP回调URL。当企业订阅的事件触发时，绩效系统会向该网址发送相应的 HTTP POST 请求。

开通好企业开放服务后，管理员可以在设置页面查看到开放信息，注册回调时，需要将回调地址配置到页面的“回调地址”中

![image.png](assets/d6859bfdedd6eb5a75e915a528846a65.jpg)


配置完成后，单击**确认/编辑**按钮时，系统会向你配置的网址推送一个application/json格式的 POST请求, 用于验证你配置的网址的合法性。如下所示：
```DEFAULT_LANGUAGE

{
  "corpId": "dingxxxxxx7fe",
  "data": "null",
  "eventName": "CHECK_CALLBACK_URL",
  "recordId": "1ftp6l87d1tmw1e8i1w26c0fd53433jj",
  "signature": "bOYWT3fjQtUQ1bDHmR6pa7UqjGtn4ZrMA0xtVlFy3jg=",
  "timestamp": 1646892063003
}
```

#### 推送数据说明
- corpId	:	企业id
- data	:	推送数据内容，json格式的字符串，根据不同的eventName，内容不同
- eventName	：	推送的事件名称
- recordId	:	本次推送的recordId
- signature	：签名，详见接口文档中的签名规则说明
- timestamp	：	时间戳


#####   CHECK\_CALLBACK\_URL ：验证回调地址
1. 注册回调地址时，立即触发，返回期望内容后，完成回调地址的注册


---

<!-- 原文定位：绩效互通能力&开发文档/获取目标下的任务列表.md -->
# 获取目标下的任务列表

原文链接：https://alidocs.dingtalk.com/i/nodes/Obva6QBXJw9lAEkNFQm6X6eYWn4qY5Pr?utm_scene=team_space

# 获取目标下的任务列表

> 来源：https://alidocs.dingtalk.com/i/nodes/Obva6QBXJw9lAEkNFQm6X6eYWn4qY5Pr?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com/openapi/perf-okr/objective/findObjTasks
```

### 请求方法

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

#### Body

| **名称** | **类型** | **是否必须** | **默认值** | **备注** |
|----------|----------|----------------|-------------|----------|
| objectiveId | string | 必须 |  | 目标id |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object\[\]</span> |  |
| <span style="color: #262626;">\- objectiveId</span> | string | 目标id |
| \- name | string | 目标名称 |
| \- krs | object\[\] | 关键结果list |
| - krId | string | 关键结果id |
| - name | string | 关键结果名称 |
| - tasks | object\[\] | 任务list |
| - taskId | string | 任务id |
| - name | string | 任务名称 |
| - deadline | number | 截止时间 |
| - ownerName | string | 负责人姓名 |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |

##


---

<!-- 原文定位：绩效互通能力&开发文档/获取目标操作记录.md -->
# 获取目标操作记录

原文链接：https://alidocs.dingtalk.com/i/nodes/G1DKw2zgV2RXgOEnTBlD2pNvVB5r9YAn?utm_scene=team_space

# 获取目标操作记录

> 来源：https://alidocs.dingtalk.com/i/nodes/G1DKw2zgV2RXgOEnTBlD2pNvVB5r9YAn?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com/openapi/perf-okr/objective/findObjLogs
```

### 请求方法 

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

#### Body

| **名称** | **类型** | **是否必须** | **默认值** | **备注** |
|----------|----------|----------------|-------------|----------|
| objectiveId | string | 必须 |  | 目标id |
| logTypeCells | object\[\] |  |  | 日志筛选 |
| \- moduleType | number | 必须 |  | 模块类型 1评论2记录3文件 |
| \- type | number |  |  | 子类型（记录支持子类型）<br>1创建 2更新 3修改进度 4删除<br>5评论 7添加关键结果 8删除关键结果 9更新关键结果 10评分<br>21添加任务 22编辑任务<br>23删除任务 24完成任务<br>25重启任务 |
| pageNo | string | 必须 | 1 |  |
| pageSize | string | 非必须 |  | 最多40条每页（不指定默认40条） |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object\[\]</span> |  |
| <span style="color: #262626;">\- objectiveId</span> | string | 目标id |
| \- name | string | 目标名称 |
| \- pageNo | number | 页数 |
| \- pageSize | number | 单页大小 |
| \- logs | object\[\] | 日志list |
| - content | string | 日志内容 |
| - createdAt | number | 创建时间 |
| - creator | string | 创建者 |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |


---

<!-- 原文定位：绩效互通能力&开发文档/获取项目列表.md -->
# 获取项目列表

原文链接：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGe0P0RJKMEvZBY?utm_scene=team_space

# 获取项目列表

> 来源：https://alidocs.dingtalk.com/i/nodes/7NkDwLng8ZM3Epg7HaGe0P0RJKMEvZBY?utm_scene=team_space
> 知识库路径：绩效互通能力&开发文档

### 请求地址
```DEFAULT_LANGUAGE
https://oapi.dingteam.com /openapi/perf-okr/project/listPage
```

### 请求方法 

POST

### 请求参数

#### Headers

| <span style="color: #262626;">**参数名称**</span> | <span style="color: #262626;">**参数值**</span> | <span style="color: #262626;">**是否必须**</span> | <span style="color: #262626;">**示例**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------------|--------------------------------------------------|-----------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">x-dingteam-access-token</span> | <span style="color: #262626;">企业凭证token</span> | <span style="color: #262626;">是</span> |  |  |

#### Body

| **名称** | **类型** | **是否必须** | **默认值** | **备注** |
|----------|----------|----------------|-------------|----------|
| pageNo | number | 是 | 1 | 页数 |
| pageSize | number |  | 10 | 一页的大小 |
| spaceId | string | 是 |  | 空间id，绩效不支持多空间，请设置为：1 |
| name | string |  |  | 项目名称（搜索时使用） |

### 返回数据

| <span style="color: #262626;">**名称**</span> | <span style="color: #262626;">**类型**</span> | <span style="color: #262626;">**备注**</span> |
|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| <span style="color: #262626;">code</span> | <span style="color: #262626;">number</span> |  |
| <span style="color: #262626;">msg</span> | <span style="color: #262626;">string</span> |  |
| <span style="color: #262626;">data</span> | <span style="color: #262626;">object\[\]</span> |  |
| \- pageNo | number | 页数 |
| \- pageSize | number | 单页大小 |
| \- <span style="background-color: rgb(255, 255, 254);">totalPages</span> | number | 总页数 |
| \- <span style="background-color: rgb(255, 255, 254);">totalCount</span> | number | 总数量 |
| \- list | object\[\] | 项目信息列表 |
| - <span style="background-color: rgb(255, 255, 254);">okrProjectId</span> | string | 项目id |
| -  <span style="background-color: rgb(255, 255, 254);">name</span> | string | 项目名称 |
| - creator | object\[\] | 创建人信息 |
| - id | string | 人员id |
| -  <span style="background-color: rgb(255, 255, 254);">name</span> | string | 姓名 |
| -  <span style="background-color: rgb(255, 255, 254);">avatar</span> | string | 头像 |
|    \-  <span style="background-color: rgb(255, 255, 254);">owner</span> | object\[\] | 负责人信息 |
| - id | string | 人员id |
| -  <span style="background-color: rgb(255, 255, 254);">name</span> | string | 姓名 |
| -  <span style="background-color: rgb(255, 255, 254);">avatar</span> | string | 头像 |
| <span style="color: #262626;">errorData</span> | <span style="color: #262626;">null</span> |  |
| <span style="color: #262626;">traceId</span> | <span style="color: #262626;">string</span> |  |
