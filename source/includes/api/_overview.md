# API 概述

所有的请求最后一个参数都应该是token，除了登陆并获取token的一个API。

```shell
curl -X GET https://<host>/api/<api_url>
```

> 每一个返回的JSON都应该符合如下格式：

```json
{
  "status": 200,
  "message": "AOK",
  "token": "null",
  "data": { <API 数据> }
}
```

### API 返回参数
参数 | 含义 | 默认值
---- | ---- | ----
status | 返回状态，使用 `HTTP` 标准 [status code](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81) | 200 表示一切正常，401 表示 token 错误
message | 对返回状态的解释 | 一切正常时，返回 AOK
token | 新的 token | `null` 表示不需要更新 token
data | API 数据 | 无

如果没有特殊情况，除 `data` 外的 API 返回参数大体相同，在之后的 API 描述予以省略。
