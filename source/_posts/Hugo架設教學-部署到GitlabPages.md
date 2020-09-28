---
title: Hugo 架設教學 - 部署到 Gitlab Pages
date: 2020-03-23 10:00:00
tags:
categories:
- Hugo 筆記
---
這篇文章會教你如何把 Hugo 部署到 Gitlab Pages 上。
<!--more-->
Gitlab Pages 的基本建立就不說了。

# 首先
Hugo 的靜態檔案會產生至 public 資料夾中，所以基本上不用做太多設定，把整個網站的資料夾加入 git 即可，不過基於某種原因，我的 Repoitory 沒有包含到整個站點資料夾，僅有必要部分。
![](https://b80xhg.bn.files.1drv.com/y4mpZrNasWdYh4_fInzxP-LcKGyTqHkwlu3JRiz4brkz0_-F-KK3cigYle1zSAfilswFdJnQ__e_XRLngDcu6YNtzVbBypAOKWx1liflZWWl7PDkEnN0uWwwZxfu6_SqrRVHRuLUN_sxpTwRmrJ84e1XiUURZ-izt8cx01dojdoDO6FPhzYFFtFKjU0bQvstO4nL76ySUVoQiZ67zBP1gZRoQ)

Gitlab Pages 還有個好處，它可以把整個 Project 設為 Private，但 Pages 設為 Public，這樣一來大家只能看到靜態內容，不用擔心整個 Project 曝光。

# .gitlab-ci.yml 設定
唯一需要設定的大概就是 CI/CD ，如下修改即可直接上線。

```yml
# This file is a template, and might need editing before it works on your project.
# Full project: https://gitlab.com/pages/plain-html
pages:
  stage: deploy
  script:
    - echo 'Nothing to do...'
  artifacts:
    paths:
    - public
  only:
  - master
```

# 塔啦！
通過！上線！
![](https://b82wqq.bn.files.1drv.com/y4ms6t6AEF9BtQkMl1V4VsvPhoX34U3wkDnLve0dBeD0KoHo00-RnW28W1awQdCM2_k24tXUnt7M_hWepdESCoOkBWGISLDTSZWxVg8I6mc1RsBV3qnkqoGA8urZ8s_Y0WKJxEv9c6mstgs0Xn7SKaOdeRQo5Hxhs6gdjam9ZaqkOBZjIhyRE9uarkhgFIIwR4j2yS3zjpFjkfjbtJ4zX0_ig)