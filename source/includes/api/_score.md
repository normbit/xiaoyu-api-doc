# Score 考试成绩

## Score Overview 成绩综览

```shell
curl -X GET https://<host>/api/information/score/overview
     -d token=<api token>
```

> 返回 `data`：

```json
{
  "recent":
    [
      {
        "subject": "语文",
        "name": "第二单元测验",
        "date": "2015-11-08",
        "mean": 90,
        "median": 93,
        "score": "87.5/100"
      },
      {
        "subject": "语文",
        "name": "小测验",
        "mean": 83,
        "median": 87,
        "date": "2015-10-14",
        "score": "90.5/100"
      },
      {
        "subject": "数学",
        "name": "单元测验",
        "mean": 95,
        "median": 91,
        "date": "2015-10-12",
        "score": "97/100"
      },
      ...
    ],
  "examGroups":
    [
      {
        "examGroupId": 123456,
        "name": "期中考试",
        "date": "2015-09-08",
        "score": "400/450"
      },
      ...,
      {
        "examGroupId": 12357,
        "name": "综合考试",
        "date": "2015-09-08",
        "score": "380/450"
      }
    ],
  "subjects":
    [
      {
        "subjectId": 123,
        "name": "语文",
        "exams": 3
      },
      ...,
      {
        "subjectId": 456,
        "name": "自然",
        "exams": 0
      }
    ]
}
```
按照考试日期，返回最近的考试成绩

### HTTP 请求
`GET /api/information/score/overview`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
token | api 令牌 | 无
recentCount | 返回最近成绩的数量 | 5

### API 返回数据
返回值分为三部分，每个部分都是一个列表:

- 最近的单项考试成绩：`recent`

  <a name="single-exam-json"></a>

  参数 | 含义
  --- | ---
  subject | 科目名称
  name | 考试名称
  date | 考试日期
  mean | 平均分
  median | 中位数
  score | 分数

- 本学期所有综合考试：`examGroups`

  参数 | 含义
  --- | ---
  examGroupId | 综合考试 ID，用于获取综合考试详情
  name | 综合考试名称
  date | 综合考试中最早的考试日期
  score | 分数和总分

- 本学期所有学科：`subjects`

  参数 | 含义
  --- | ---
  subjectId | 学科 ID，用于获取学科考试详情
  name | 学科名称
  exams | 考试数量

## Exam Group 综合考试详情

```shell
curl -X GET http://<host>/api/information/score/examGroupDetail
     -d token=<api token>
     -d examGroupId=<exam group id>
```

> 返回 `data`：

```json
{
  "name": "期中考试",
  "score": "285/300",
  "scores":
    [
      {
        "subject": "语文",
        "name": "语文期中考试",
        "date": "2015-11-03",
        "mean": 85,
        "median": 89,
        "score": "95/100"
      },
      {
        "subject": "数学",
        "name": "数学期中考试",
        "mean": 92,
        "median": 89.5,
        "date": "2015-11-03",
        "score": "95/100"
      },
      {
        "subject": "英语",
        "name": "英语期中考试",
        "mean": 88,
        "median": 90,
        "date": "2015-11-04",
        "score": "95/100"
      }
    ]
}
```

### HTTP 请求
`GET /api/information/score/examGroupDetail`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
examGroupId | 综合考试 ID  | 无

### API 返回数据
参数 | 含义
--- | ----
name | 综合考试名称
score | 总成绩和总分
scores | 综合考试中的每项考试，其中的每项考试详情[同前](#single-exam-json)


## Subject Exams 学科考试成绩

```shell
curl -X GET http://<host>/api/information/score/subjectDetail
     -d token=<api token>
     -d pageSize=5
     -d subjectId=4
```

> 返回 `data`：

```json
{
  "subject": "数学",
  "scores":
    [
      {
        "name": "期中考试",
        "date": "2015-11-03",
        "mean": 93,
        "median": 91.5,
        "score": "95/100"
      },
      {
        "name": "第二单元测试",
        "date": "2015-10-12",
        "mean": 95,
        "median": 95,
        "score": "97/100"
      }
    ]
}
```

### HTTP 请求
`GET /api/information/score/subjectDetail`

### API 参数
参数 | 含义 | 默认值
--- | ---- | -----
pageSize | 返回的考试数量 | 30
subjectId | 学科 ID | 无

### API 返回值
参数 | 含义
--- | ----
subject | 学科名称
scores | 学科考试列表，其中的每项考试详情[同前](#single-exam-json)，但不包括 `subject` 科目名称
