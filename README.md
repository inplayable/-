# Iplayable网盟平台下游对接文档
## 1. 概要
本API文档包括线上调用Iplayable网盟平台Offer List的API方法，响应实例，点击传入以及跟踪回掉参数的配置信息

## 2. 查询 offer

请求地址：
```bash
http://47.90.165.0/api/v1/getoffer
```


每次请求 offer 时，API请求参数说明：

| 参数名称 | 参数说明 | 示例 | 是否必须 |
| -------- | -------- | ---- | -------- |
| app_id | 渠道 ID，分配给渠道的唯一标识 | 50 | 是 |
| app_key | 请求 KEY，验证请求的合法性（32位长度字符串） | 098f6bcd4621d373cade4e832627b4f6 | 是 |
| countries | Offer 国家，支持大小写，两位字符国家简写，多值使用逗号隔开 | ID,TH,HK | 否 |
| platform | Offer 操作系统，支持大小写，多值使用逗号隔开 | android,ios | 否 |
| price_type | Offer 出价类型，支持大小写，多值使用逗号隔开 | CPM,CPC,CPI,CPE | 否 |


查询 offer API 调用示例（测试的 API，接入的正式 API 需要申请 app_id 和 app_key）:
```bash
http://47.90.165.0/api/v1/getoffer?app_id=1000&app_key=098f6bcd4621d373cade4e832627b4f6&countries=ID&paltform=android&price_type=CPI
```


## 3. 查询 offer 响应内容

查询结果示例：
```bash
{
    "code": 0,
    "datas": [
        {
            "app_name": "Dream Wars",
            "app_pkg": "com.mmosea.android.dreamwars",
            "click_url": "http://47.90.165.0/api/v1/getoffer?app_id=1000&offer_id=46&clickid={clickid}&gaid={gaid}&android={android}&idfa={idfa}&subid={subid}&affsub={affsub}",
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
            "click_url": "http://47.90.165.0/api/v1/getoffer?app_id=1000&offer_id=22&clickid={clickid}&gaid={gaid}&android={android}&idfa={idfa}&subid={subid}&affsub={affsub}",
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

响应内容为 json 字符串，"status": "success" 表示请求成功，"status": "fail" 表示请求失败，total 表示 offer 总数。datas 为 offer 列表，offer 列表说明如下：

| 字段名称 | 参数说明 |
| -------- | -------- |
| app_name |  名称 |
| app_pkg | 包名 |
| click_url | Offer 跳转链接 |
| countries | 国家 |
| daily_cap | Offer 每日预算限制，激活数不能超过这个值 |
| id | Offer 唯一标识 |
| kpitype | Offer 转化条件、KPI |
| material | Offer 素材图片，包含多个尺寸的图片 |
| min_os_version | 最低版本号，版本号低于此值是无效流量 |
| platform | 平台， android 或者 ios |
| preview_url | 预览链接，点击进入 google play 查看 offer |
| price | Offer 单价 |
| pricing_models | 计费事件 CPI 等 |

## 4. Click_url 参数说明

发送get请求，请求地址：
```bash
http://47.90.165.0/api/v1/click
```

| 字段名称 | 参数说明 | 是否必须 |
| -------- | -------- | ------ |
| app_id | 渠道 id | 是 |
| offer_id | offerid | 是 |
| gaid | 谷歌广告 ID | android系统必传 |
| idfa | IOS idfa | ios系统必传 |
| para1 | 自定义参数1 | 否 |
| para2 | 自定义参数2 | 否 |
| paran | 自定义参数n | 否 |

示例：
```bash
http://47.90.165.0/api/v1/click?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&para1=somevalue1&para1=somevalue2
```

## 5. Postback 参数说明
Offer 激活之后，可以向 postback 地址发送一次请求，首先会在postback中填写第4部分点击里的所有参数，如5.1所示；其次，还会写入5.2所示参数。

5.1

a.下游发送的的Click_url

5.2
postback 时除了会带上第4部分点击里的所有参数外还会会传入以下参数：

| 字段名称 | 参数说明 |
| -------- | -------- |
| clickid | 点击id，安装的唯一标识 |
| insts | 安装时间 |
| breason | 点击拒绝原因代号 |
| bsub | 点击拒绝子原因代号 |
| bvalue | 点击拒绝原因 |
| ename | 后链路事件，注册或者付费等 |
| erev | 后链路事件收益 |
| etime | 后链路事件发生时间 |
| evalue | 后链路事件内容 |
| payout | Offer 收益，货币为美金 |

Postback 示例 ：
```bash
http://your.domain.com/postback?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&any_para1=somevalue1&any_para2=somevalue2&payout=1.5
```
