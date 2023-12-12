# Iplayable网盟平台下游对接文档
## 1. 概要
本API文档包括线上调用Iplayable网盟平台Offer List的API方法，响应示例，点击传入以及跟踪回掉参数的配置信息

## 2. 查询 offer

请求地址：
```bash
http://callback.flatmobi.com/api/v1/getoffer
```


每次请求 offer 时，API请求参数说明：

| 参数名称 | 参数说明 | 示例 | 是否必须 |
| -------- | -------- | ---- | -------- |
| app_id | 渠道 ID，分配给渠道的唯一标识 | 50 | 是 |
| app_key | 请求 KEY，验证请求的合法性（32位长度字符串） | 098f6bcd4621d373cade4e832627b4f6 | 是 |
| countries | Offer 国家，支持大小写，请使用两位字符国家简写，多值使用逗号隔开 | ID,TH,hk | 否 |
| platform | Offer 操作系统，支持大小写，多值使用逗号隔开 | android,ios | 否 |
| price_type | Offer 出价类型，支持大小写，多值使用逗号隔开 | CPM,CPC,cpi,CPE | 否 |


查询 offer API 调用示例（测试的 API，接入的正式 API 需要申请 app_id 和 app_key）:

http://callback.flatmobi.com/api/v1/getoffer?app_id=1000&app_key=098f6bcd4621d373cade4e832627b4f6&countries=ID&paltform=android&price_type=CPI



## 3. 查询 offer 响应内容

查询结果示例：
```bash
{
    "code": 0,
    "datas": [
        {
            "app_name": "Dream Wars",
            "app_pkg": "com.mmosea.android.dreamwars",
            "click_url": "http://callback.flatmobi.com/api/v1/click?app_id=1000&offer_id=46&clickid={clickid}&gaid={gaid}&idfa={idfa}",
            "countries": [],
            "daily_cap": 100,
            "id": "46",
            "kpitype": "sometestcomment",
            "material": [],
            "min_os_version": "",
            "platform": "android",
            "preview_url": "https://play.google.com/store/apps/details?id=com.mmosea.android.dreamwars",
            "price": 7,
            "pricing_models": "CPI"
        },
        {
            "app_name": "MOXA_id_android",
            "app_pkg": "id.moxa",
            "click_url": "http://callback.flatmobi.com/api/v1/click?app_id=1000&offer_id=22&clickid={clickid}&gaid={gaid}&idfa={idfa}",
            "countries": [
                "IDN",
            ],
            "daily_cap": 10,
            "id": "22",
            "kpitype": "",
            "material": [],
            "min_os_version": "8.0",
            "platform": "android",
            "preview_url": "https://play.google.com/store/apps/details?id=id.moxa",
            "price": 7,
            "pricing_models": "CPI"
        }
    ],
    "status": "success"
}
```

响应内容为 json 字符串，"status": "success" 表示请求成功，"status": "fail" 表示请求失败。datas 为 offer 列表，offer 列表字段说明如下：

| 字段名称 | 字段类型 | 参数说明 |
| ------- | ------ | ------- |
| app_name | string |  app 名称 |
| app_pkg | string | app 包名 |
| click_url | string | Offer 跳转链接 |
| countries | string | 国家 |
| daily_cap | int | Offer 每日预算限制，激活数不能超过这个值 |
| id | string | Offer 唯一标识 |
| kpitype | string | Offer 转化条件、KPI |
| material | []string | Offer 素材图片，包含多个尺寸的图片 |
| min_os_version | string | 最低版本号，版本号低于此值是无效流量 |
| platform | string | 平台， android 或者 ios |
| preview_url | string | 预览链接，点击进入 google play 查看 offer |
| price | float32 | Offer 单价 |
| pricing_models | string | 计费事件 CPI 等 |

## 4. 点击跟踪链接参数说明

发送get请求，请求地址：
```bash
http://callback.flatmobi.com/api/v1/click
```

点击跟踪链接查询参数支持：

| 参数名称 | 宏参 | 参数说明 | 是否必须 |
| -------- | ------ | -------- | ------ |
| app_id | {app_id} | 渠道 id | 是 |
| offer_id | {offer_id} | offerid | 是 |
| gaid | {gaid} | 谷歌广告 ID | android系统必传 |
| idfa | {idfa} | IOS idfa | ios系统必传 |

**注：** 点击跟踪链接中的查询参数的名称和宏参必须对应（如上），并且不支持更改。

**示例：**

下游调用查询 offer API 得到的 click_url ：

http://callback.flatmobi.com/api/v1/click?app_id=1000&offer_id=22&&gaid={gaid}&idfa={idfa}

其中app_id 和 offer_id 查询参数的值为 Iplayable 网盟平台进行填写，填写后在下游查询offer时返回，请看第3节；gaid、idfa 查询参数的值为下游进行填写替换，替换为对应值后发送给 Iplayable 网盟平台，例如下游填写替换点击跟踪链接后发送给 Iplayable 网盟平台的内容应为以下格式。

**下游发送给 Iplayable 平台的点击跟踪链接示例：**

http://callback.flatmobi.com/api/v1/click?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&idfa={idfa}

<!-- 注：para1、para2、para3 为预留自定义参数，若需要在 postback 中回传一些额外内容可在此添加，添加格式如上方示例 url ，并与 Iplayable 平台客户经理联系在 postback 中配置，配置细节将在第 5 节中说明。 -->




## 5. Postback 配置说明

Iplayable 网盟平台支持的回调链接宏参：

**a.** 点击跟踪链接信息回传宏参

| 宏参 | 参数说明 | 是否必须 |
| --- | ------- | ------- |
| {aff_id} | 渠道 ID，分配给渠道的唯一标识 | 否 |
| {campaign_id} | 单子唯一标识 | 否 |
| {gaid} | 谷歌广告 ID | 否 |
| {idfa} | IOS idfa | 否 |

**注：** Iplayable 网盟平台支持将点击跟踪链接请求的的信息回传给下游。宏参{aff_id}、{campaign_id}、{gaid}、{idfa} 对应的值来源于下游发送的 click_url ，见第4节，分别对应第 4 节中的宏参 {app_id} 、 {offer_id}、{gaid}、{idfa}。

**b.** 上游/三方安装信息回传宏参

| 宏参 | 参数说明 | 是否必须 |
| --- | ------- | ------- |
| {insts} | 安装时间戳，仅支持安装事件回传 | 否 |
| {breason} | Block reason，仅支持安装事件回传 | 否 |
| {bsub} | Block sub reason，仅支持安装事件回传 | 否 |
| {bvalue} | Block value，仅支持安装事件回传 | 否 |

**注：** 该部分宏参对应的值来自于上游/三方的安装回调，因此仅支持安装事件信息的回传。

**c.** 上游/三方后链路事件信息回传宏参

| 宏参 | 参数说明 | 是否必须 |
| --- | ------- | ------- |
| {ename} | 后链路事件，注册或者付费等，仅支持后链路事件回传 | 否 |
| {erev} | 后链路事件收益，仅支持后链路事件回传 | 否 |
| {etime} | 后链路事件时间戳，仅支持后链路事件回传 | 否 |
| {evalue} | 后链路事件value，仅支持后链路事件回传 | 否 |

**注：** 该部分宏参对应的值来自于上游/三方的后链路事件回调，因此仅支持后链路事件信息的回传。

**d.** 其他宏参

| 宏参 | 参数说明 | 是否必须 |
| --- | ------- | ------- |
| {payout} | Offer 收益，货币为美金 | 否 |


**安装回调链接示例：**

http://your.domain.com/your/route?your_aff_para={app_id}&your_camp_para={campaign_id}&your_gaid_para={gaid}&your_blockreason_para={breason}&your_payout_para={payout}&other_fix=fixvalue

**注：** 请将 your.domain.com、your/route 替换为自己的真实回调域名和路径；your_aff_para、your_camp_para、your_gaid_para、your_blockreason_para、your_payout_para 请替换为自己的查询参数名称；other_fix为可配置的固定回传参数，配置时请替换为自己的固定查询参数名称，并配置固定查询参数值。

**后链路事件回调链接示例：**

http://your.domain.com/your/route?your_aff_para={app_id}&your_camp_para={campaign_id}&your_gaid_para={gaid}&your_eventname_para={ename}&your_payout_para={payout}&other_fix=fixvalue

**注：** 替换逻辑同安装回调链接示例。

<!-- 在进行安装回传时，需要三步，首先需要在 Iplayable 平台配置 postback url ，然后 Iplayable 平台会根据下游发送的点击以及上游或三方传来的回调数据提取宏参，最后 IPlayable 平台会使用提取的填充 postback url 使用填充后的 postback url 对下游进行get请求，**示例如下：**

### a. Iplayable 系统配置postback url，例如：

http://your.domain.com/your/route?your_click_para={clickid}&your_aff_para={app_id}&your_camp_para={offer_id}&your_blockreason_para={breason}&your_payout_para={payout}&other_fix=fixvalue&other_fix2=fixvalue2&your_name={para1}

**Postback url注解：**

在真实生产环境中配置时，请将 your.domain.com、your/route 替换为自己的回调主域名和路径；your_click_para、your_aff_para、your_camp_para、your_blockreason_para、your_payout_para 请替换为自己的参数名；other_fix、other_fix2 为可配置的固定回传参数，配置时请替换为自己的固定参数名称；your_name 为第 4 步中接收到的预留自定义参数，请替换为自己的自定义参数名称；

### b. Iplayable 网盟平台从下游发送的的 Click_url 以及上游或者三方的回调提取宏参，例如：

Iplayable 平台接收到下游发送的 Click_url 如第4步所示：

http://callback.flatmobi.com/api/v1/click?app_id=1000&offer_id=22&clickid=abc&gaid=4716d154-7232-11ee-800e-e9037848532a&android={android}&idfa={idfa}&subid={subid}&affsub=testabc&para1=somevalue1&para2=somevalue2

Iplayable 平台接收到的上游或者三方的回调：

http://callback.flatmobi.com/our_rout?our_click_param=123&istall_time=2023-11-11&block_reason=test1&payout=1.5

继而提取到宏参如下：

| 宏参 | 宏参值 |
| --- | ------ |
| {app_id} | 1000 |
| {offer_id} | 22 |
| {clickid} | abc |
| {gaid} | 4716d154-7232-11ee-800e-e9037848532a |
| {affsub} | testabc |
| {insts} | 2023-11-11 |
| {breason} | test1 |
| {payout} | 1.5 |
| {para1} | somevalue1 |
| {para2} | somevalue2 |

### c. Iplayable 平台系统使用步骤 b 提取到的宏参填充步骤a中配置的 postback url ，并使用填充后的 url 进行 get 请求，填充结果如下：

http://your.domain.com/your/route?your_click_para=abc&your_aff_para=1000&your_camp_para=22&your_blockreason_para=test1&your_payout_para=1.5&&other_fix=fixvalue&other_fix2=fixvalue2&your_name=somevalue1 -->


