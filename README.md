# Iplayable网盟平台下游对接文档
## 1. 概要
本API文档包括线上调用Iplayable网盟平台Offer List的API方法，响应实例，点击传入以及跟踪回掉参数的配置信息

## 2. 查询 offer

请求地址：
```bash
http://callback.flatmobi.com/offer/getoffer
```


每次请求 offer 时，API请求参数说明：

| 参数名称 | 参数说明 | 示例 | 是否必须 |
| -------- | -------- | ---- | -------- |
| app_id | 渠道 ID，分配给渠道的唯一标识 | 50 | 是 |
| app_key | 请求 KEY，验证请求的合法性（32位长度字符串） | 098f6bcd4621d373cade4e832627b4f6 | 是 |
| countries | Offer 国家，两位大写字符国家简写，多个国家用逗号隔开 | ID,TH,HK | 否 |
| platform | Offer 操作系统，多系统使用逗号隔开 | android,ios | 否 |
| price_type | Offer 出价类型，大写，多值使用逗号隔开 | CPM,CPC,CPI,CPE | 否 |


查询 offer API 调用示例（测试的 API，接入的正式 API 需要申请 app_id 和 app_key）:
```bash
http://callback.flatmobi.com/offer/getoffer?app_id=1000&app_key=098f6bcd4621d373cade4e832627b4f6&countries=ID&paltform=android&price_type=CPI
```


## 3. 查询 offer 响应内容

查询结果示例：
```bash
{
    "code": 0,
    "datas": [
        {
            "app_icon": "",
            "app_name": "Dream Wars",
            "app_pkg": "com.mmosea.android.dreamwars",
            "blockChannel": "",
            "category": "",
            "click_url": "http://flatmobi.com/aff/click?app_id=1000&offer_id=46&clickid={clickid}&gaid={gaid}&android={android}&idfa={idfa}&subid={subid}&affsub={affsub}",
            "countries": [],
            "daily_cap": 100,
            "des": "",
            "id": "46",
            "impression_url": "",
            "isincent": "0",
            "kpitype": "sometestcomment",
            "material": [],
            "min_os_version": "",
            "platform": "android",
            "preview_url": "https://play.google.com/store/apps/details?id=com.mmosea.android.dreamwars",
            "price": 7,
            "pricing_models": "CPI"
        },
        {
            "app_icon": "",
            "app_name": "MOXA_id_android",
            "app_pkg": "id.moxa",
            "blockChannel": "",
            "category": "",
            "click_url": "http://flatmobi.com/aff/click?app_id=1000&offer_id=22&clickid={clickid}&gaid={gaid}&android={android}&idfa={idfa}&subid={subid}&affsub={affsub}",
            "countries": [
                "IDN",
            ],
            "daily_cap": 10,
            "des": "",
            "id": "22",
            "impression_url": "",
            "isincent": "0",
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

响应内容为 json 字符串，"status": "success" 表示请求成功，"status": "fail" 表示请求失败，total 表示 offer 总数。datas 为 offer 列表，offer 列表说明如下：

| 字段名称 | 参数说明 |
| -------- | -------- |
| id | Offer 唯一标识 |
| app_name |  名称 |
| app_pkg | 包名 |
| click_url | Offer 跳转链接 |
| daily_cap | Offer 每日预算限制，激活数不能超过这个值 |
| countries | 国家 |
| isincent | 激励类型，1=激励 offer、0=非激励 offer |
| des | offer 描述信息 |
| kpitype | Offer 转化条件、KPI |
| material | Offer 素材图片，包含多个尺寸的图片 |
| min_os_version | 最低版本号，版本号低于此值是无效流量 |
| platform | 平台， android 或者 ios |
| preview_url | 预览链接，点击进入 google play 查看 offer |
| price | Offer 单价 |
| pricing_models | 计费事件CPC CPI等 |

## 4. Click_url 参数说明

发送get请求，请求地址：
```bash
http://callback.flatmobi.com/aff/click
```

| 数据名称 | 参数说明 | 可选性 |
| -------- | -------- | ------ |
| app_id | 渠道 id | 必传 |
| offer_id | offerid | 必传 |
| gaid | 谷歌广告 ID | android系统必传 |
| idfa | IOS idfa | ios系统必传 |
| 其他任何参数1 |  |  |
| 其他任何参数2 |  |  |
| 其他任何参数n |  |  |

示例：
```bash
http://callback.flatmobi.com/aff/click?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&any_para1=somevalue1&any_para2=somevalue2
```

## 5. Postback 参数说明

Offer 激活之后，可以向 postback 地址发送一次请求，postback 时除了会带上第4部分点击里的所有参数外还会会传入以下参数：

| 数据名称 | 参数说明 |
| -------- | -------- |
| ClickID |  |
| InstallTimeStamp |  |
| BlockedReason |  |
| BlockedSubReason |  |
| BlockedReasonValue |  |
| EventName |  |
| EventRevenue |  |
| EventTime |  |
| EventValue |  |
| payout | Offer 收益，货币为美金 |

Postback 示例 ：
```bash
http://your.domain.com/postback?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&any_para1=somevalue1&any_para2=somevalue2&payout=1.5
```
