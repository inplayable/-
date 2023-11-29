# 系统下游对接文档v3.1
## 1. 查询 offer

请求地址： `http://callback.flatmobi.com/offer/getoffer`

每次请求 offer 时，可以配置以下参数：

| 参数名称 | 参数说明 | 示例 | 是否必须 |
| -------- | -------- | ---- | -------- |
| app_id | 渠道 ID，分配给渠道的唯一标识 | 50 | 是 |
| app_key | 请求 KEY，验证请求的合法性（32位长度字符串） | 098f6bcd4621d373cade4e832627b4f6 | 是 |
| countries | Offer 国家，两位大写字符国家简写，多个国家用逗号隔开 | ID,TH,HK | 否 |
| platform | Offer 操作系统，多系统使用逗号隔开 | android,ios | 否 |
| price_type | Offer 出价类型，大写，多值使用逗号隔开 | CPM,CPC,CPI,CPE |否|

查询 offer API（测试的 API，接入的正式 API 需要申请 app_id 和 app_key） `http://callback.flatmobi.com/offer/getoffer?app_id=1000&app_key=098f6bcd4621d373cade4e832627b4f6&countries=ID&paltform=android`

## 2. 查询 offer 响应内容

查询结果示例：