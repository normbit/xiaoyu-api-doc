Slate Documentation for Mobile API
===

[![publish](https://github.com/normbit/xiaoyu-api-doc/workflows/publish/badge.svg)](https://github.com/normbit/xiaoyu-api-doc/actions)

### Authors
- [Liang Ran](https://github.com/ranliang)
- [LiRen Tu](https://github.com/tuliren)

### Reviewers
- [ZhaoQing Wang](https://bitbucket.org/wangzhaoqing8778/)

### Procedures
- A pull request is submitted for each new or change in API.
- The pull request is reviewed, discussed and approved by at least one of the engineers.
- The pull request is merged by one of the authors.

### Deploy
- Update api doc: `bundle exec middleman build --clean` ([reference](https://github.com/tripit/slate/wiki/Deploying-Slate)) or `bin/deploy_local.sh`
- Restart `nginx`: `sudo service nginx restart`
