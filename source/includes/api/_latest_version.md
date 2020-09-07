# Mobile 版本

## Mobile 获得最新版本号

```shell
# iOS
curl -X GET https://<host>/api/mobile/ios/latestVersion

# Android
curl -X GET https://<host>/api/mobile/android/latestVersion
```

> 返回值 `data`：

```json
{
  "latestVersion": "<version>",
  "url": "https://<host>/<app type>/<app name>v<version>.<app extension>"
}
```

### HTTP 请求
iOS：
`GET /api/mobile/ios/latestVersion`

Android：
`GET /api/mobile/android/latestVersion`

### API 参数
无

### API 返回数据
  参数 | 含义
  --- | ---
  latestVersion | 最新版本，整数
  url | 下载地址
