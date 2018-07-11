---
title: github api - github api中文说明
keywords: []
date: 2017/4/28 15:45
tags: []
categories:
---
github api的网址
```
https://api.github.com/
```
这里介绍两个api
```
#获取个人信息
https://api.github.com/users/{user};
```

| key | 含义 | value |
|:-----|:-----|:-----|
| login | 登录名称 | hisen-yuan |
| id | 数字编号 | 16789019 |
| avatar_url | 头像地址 | https://avatars1.githubusercontent.com/u/16789019?v=3 |
| name | 用户昵称 | hisenyuan |
| blog | 博客地址 | http://hisen.me |
| location | 地理位置 | China |
| bio | 个人说明| Java R & D |
| public_repos | 仓库个数 | 11 |
| created_at | 创建时间 | 2016-01-20 01:57:15Z |
| updated_at | 最后更新 | 2017-04-20 14:03:27Z |
```
#获取项目信息
https://api.github.com/users/{user}/repos
```

| key | 含义 | value |
|:-----|:-----|:-----|
| id | 项目编号 | 88646378 |
| name | 项目名称 | dubbo |
| html_url | 项目地址 | https://github.com/hisen-yuan/dubbo |
| created_at | 创建时间 | 2017-04-18T16:21:57Z |
| updated_at | 更新时间 | 2017-04-18T16:23:16Z |
| pushed_at | 提交时间 | 2017-04-19T02:33:33Z |
| size | 项目大小| 6514 |
| language | 编程语言 | Java |

github提供的所有api
<!--more-->
```
current_user_url: "https://api.github.com/user",
current_user_authorizations_html_url: "https://github.com/settings/connections/applications{/client_id}",
authorizations_url: "https://api.github.com/authorizations",
code_search_url: "https://api.github.com/search/code?q={query}{&page,per_page,sort,order}",
commit_search_url: "https://api.github.com/search/commits?q={query}{&page,per_page,sort,order}",
emails_url: "https://api.github.com/user/emails",
emojis_url: "https://api.github.com/emojis",
events_url: "https://api.github.com/events",
feeds_url: "https://api.github.com/feeds",
followers_url: "https://api.github.com/user/followers",
following_url: "https://api.github.com/user/following{/target}",
gists_url: "https://api.github.com/gists{/gist_id}",
hub_url: "https://api.github.com/hub",
issue_search_url: "https://api.github.com/search/issues?q={query}{&page,per_page,sort,order}",
issues_url: "https://api.github.com/issues",
keys_url: "https://api.github.com/user/keys",
notifications_url: "https://api.github.com/notifications",
organization_repositories_url: "https://api.github.com/orgs/{org}/repos{?type,page,per_page,sort}",
organization_url: "https://api.github.com/orgs/{org}",
public_gists_url: "https://api.github.com/gists/public",
rate_limit_url: "https://api.github.com/rate_limit",
repository_url: "https://api.github.com/repos/{owner}/{repo}",
repository_search_url: "https://api.github.com/search/repositories?q={query}{&page,per_page,sort,order}",
current_user_repositories_url: "https://api.github.com/user/repos{?type,page,per_page,sort}",
starred_url: "https://api.github.com/user/starred{/owner}{/repo}",
starred_gists_url: "https://api.github.com/gists/starred",
team_url: "https://api.github.com/teams",
user_url: "https://api.github.com/users/{user}",
user_organizations_url: "https://api.github.com/user/orgs",
user_repositories_url: "https://api.github.com/users/{user}/repos{?type,page,per_page,sort}",
user_search_url: "https://api.github.com/search/users?q={query}{&page,per_page,sort,order}"
```