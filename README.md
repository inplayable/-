# Iplayable网盟平台下游对接文档
## 1. 概要
本API文档包括线上调用Iplayable网盟平台Offer List的API方法，响应实例以及跟踪回掉参数的配置信息

## 1. 查询 offer

请求地址：
```bash
http://callback.flatmobi.com/offer/getoffer
```

每次请求 offer 时，可以配置以下参数：

| 参数名称 | 参数说明 | 示例 | 是否必须 |
| -------- | -------- | ---- | -------- |
| app_id | 渠道 ID，分配给渠道的唯一标识 | 50 | 是 |
| app_key | 请求 KEY，验证请求的合法性（32位长度字符串） | 098f6bcd4621d373cade4e832627b4f6 | 是 |
| countries | Offer 国家，两位大写字符国家简写，多个国家用逗号隔开 | ID,TH,HK | 否 |
| platform | Offer 操作系统，多系统使用逗号隔开 | android,ios | 否 |
| price_type | Offer 出价类型，大写，多值使用逗号隔开 | CPM,CPC,CPI,CPE | 否 |

查询 offer API（测试的 API，接入的正式 API 需要申请 app_id 和 app_key） `http://callback.flatmobi.com/offer/getoffer?app_id=1000&app_key=098f6bcd4621d373cade4e832627b4f6&countries=ID&paltform=android`

## 2. 查询 offer 响应内容

查询结果示例：

响应内容为 json 字符串，status=access 表示请求成功，error 表示请求失败，total 表示 offer 总数。offers 为 offer 列表 offer 列表说明如下：

| 数据名称 | 参数说明 |
| -------- | -------- |
| id | Offer 唯一标识 |
| app_name | Offer 名称 |
| app_icon | Offer 图标 |
| app_pkg | 包名 |
| price | Offer 单价 |
| pricing_models | 计费事件cpc cpi等 |
| click_url | Offer 跳转链接 |
| daily_cap | Offer 每日预算限制，激活数不能超过这个值 |
| preview_url | 预览链接，点击进入 google play 查看 offer |
| countries | 国家 |
| platform | 平台， android 或者 ios，多个平台以逗号分隔 |
| isincent | 激励类型，1=激励 offer、0=非激励 offer |
| des | offer 描述信息 |
| material | Offer 素材图片，包含多个尺寸的图片 |
| category | Offer 分类 |
| kpitype | Offer 转化条件、KPI |
| min_os_version | 最低版本号，版本号低于此值是无效流量 |
| BlockChannel | 禁止投放的subid |

## 3. Click_url 参数说明

发送get请求，请求地址：`http://callback.flatmobi.com/aff/click`

| 数据名称 | 参数说明 | 可选性 |
| -------- | -------- | ------ |
| app_id | 渠道 id | 必传 |
| offer_id | offerid | 必传 |
| gaid | 谷歌广告 ID | android系统必传 |
| idfa | IOS idfa | ios系统必传 |
| 其他任何参数1 |  |  |
| 其他任何参数2 |  |  |
| 其他任何参数n |  |  |

示例： `http://callback.flatmobi.com/aff/click?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&any_para1=somevalue1&any_para2=somevalue2`

## 4. Postback 参数说明

Offer 激活之后，可以向 postback 地址发送一次请求，postback 时除了会带上第3部分点击里的所有参数外还会会传入以下参数：

| 数据名称 | 参数说明 |
| -------- | -------- |
| payout | Offer 收益，货币为美金 |

Postback 示例 ： `http://your.domain.com/postback?app_id=1000&offer_id=22&gaid=4716d154-7232-11ee-800e-e9037848532a&any_para1=somevalue1&any_para2=somevalue2&payout=1.5`