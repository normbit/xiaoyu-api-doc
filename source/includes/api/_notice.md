# Notice 通知

## Notice List 获取通知列表

```shell
curl -X GET https://<host>/api/notice/list
     -d token=<api token>
     -d pageSize=<page size>
     -d page=<page>
```

> 返回 `data`：

```json
{
  "notices":
    [
      {
        "title": "通知标题",
        "time": 1463303896,
        "noticeId": 36,
        "isRead": 0,
        "content": "通知正文",
        "hasQuestion": 0,
        "hasReplied": 0
      },
      {
        "title": "通知标题",
        "time": 1463302964,
        "noticeId": 46,
        "isRead": 1,
        "content": "通知正文",
        "hasQuestion": 1,
        "hasReplied": 0
      },
      ...,
      {
        "title": "通知标题",
        "time": 1463302010,
        "noticeId": 56,
        "isRead": 1,
        "content": "通知正文",
        "hasQuestion": 1,
        "hasReplied": 0
      }
    ]
}
```

### HTTP 请求
`GET /api/notice/list`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
pageSize | 每页返回的通知数 | 30
page | 要返回的页数，从 1 开始 | 1

### API 返回数据
返回 `notices`，是长度不超过 `pageSize` 的列表，列表内元素如下：

参数 | 含义
--- | ---
title | 通知标题
time | 通知发出时间，用 UNIX 时间戳表示
noticeId | 通知 ID；在发送阅读回执和回复时，用来跟踪是哪个通知
isRead | 是否已读；1 表示已读，0 表示没有
content | 通知正文
hasQuestion | 是否附带问题
hasReplied | 问题是否已经回答

## Notice Get 获取通知详情
```shell
curl -X GET https://<host>/api/notice/get
     -d token=<api token>
     -d noticeId=<notice id>
```

> 返回 `data`：

```json
{
  "title": "通知标题",
  "time": 1463303896,
  "noticeId": 36,
  "isRead": 0,
  "content": "通知正文",
  "questions":
    [
      {
        "id": 1,
        "type": "open",
        "question": "开放问题 1",
        "minLen": 1,
        "maxLen": 50,
        "answer": "null"
      },
      {
        "id": 2,
        "type": "open",
        "question": "开放问题 2",
        "minLen": 1,
        "maxLen": 50,
        "answer": "回答"
      },
      {
        "id": 3,
        "type": "choice",
        "question": "选择题 1",
        "minChoice": 1,
        "maxChoice": 3,
        "choices":
          [
            {"id": 37, "choice": "选项 1"},
            {"id": 47,"choice": "选项 2"}
          ],
        "selection": []
      },
      {
        "id": 4,
        "type": "choice",
        "question": "选择题 2",
        "minChoice": 1,
        "maxChoice": 1,
        "choices":
          [
            {"id": 57, "choice": "选项 1"},
            {"id": 67, "choice": "选项 2"}
          ],
        "selection": [57]
      }
    ]
}
```

### HTTP 请求
`GET /api/notice/get`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
noticeId | 需要返回的通知 ID | 无

### API 返回数据
- `title` 通知标题
- `time` 通知发出时间，用 UNIX 时间戳表示
- `noticeId` 通知 ID；在发送阅读回执和回复时，用来跟踪是哪个通知
- `isRead` 是否已读；1 表示已读，0 表示没有
- `content` 通知正文
- `questions` 通知问题列表

通知问题包括两类：问答题和选择题，由列表元素中的 `type` 值决定。

- 问答题（open question）

  参数 | 含义
  --- | ----
  id | 问题 ID（`notice_question_id`，而非 `open_question_id`），发送回复时使用
  type | 类型，等于 `open`
  question | 问题描述
  minLen | 最少字符数限制
  maxLen | 最大字符数限制
  answer | 答案，未回复时为 `null`

- 选择题（choice question）

  参数 | 含义
  --- | ----
  id | 问题 ID（`notice_question_id`，而非 `choice_question_id`），发送回复时使用
  type | 类型，等于 `choice`
  question | 问题描述
  minChoice | 最小选择数
  maxChoice | 最大选择数
  choices | 可选项列表，每个元素包含 `id`（选项 ID）和 `choice`（选项描述）两个字段
  selection | 选中的选项列表，元素是选项 ID，长度介于 `minChoice` 和 `maxChoice` 之间

## Notice SetRead 发送阅读回执

```shell
curl -X POST https://<host>/api/notice/setRead
     -d token=<api token>
     -d noticeId=<notice id>
     -d setRead=1
```

> 返回 `data`：

```json
{
  "status": 1
}
```

### HTTP 请求
`POST /api/notice/setRead`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
noticeId | 通知 ID | 无
setRead | 要设置的阅读状态；目前不支持将已读设为未读，所以此参数只能是 1 | 1

### API 返回数据
参数 | 含义
--- | ---
status | 1 表示操作成功，0 表示失败

## Notice Reply 发送回复答案

```shell
curl -X POST https://<host>/api/notice/reply
     -d noticeId=<notice id>
     -d q1=开放问题1答案
     -d q2=开放问题2答案
     -d q3[]=37
     -d q3[]=47
     -d q4[]=57
     ...
     -d q<open question id>=<open question answer>
     -d q<choice question id>[]=<choice id>
```

> 返回 `data`：

```json
{
  "status": 1
}
```

### HTTP 请求
`POST /api/notice/reply`

### API 参数
参数 | 含义 | 样例
--- | ---- | -----
noticeId | 通知 ID | 无
q\<id\> | 开放问题答案 | `q1=开放问题1答案`，表示 ID 为 `1` 的开放问题问题的回答
q\<id\>[] | 选择题选中的选项 ID | `q3[]=37&q3[]=47`，表示 ID 为 `3` 的选择题选择 ID 为 `3` 和 `4` 的选项

### API 返回数据
参数 | 含义
--- | ---
status | 1 表示操作成功，0 表示失败
