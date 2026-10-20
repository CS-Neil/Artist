---
title: 日常电影
description: 用镜头定格日常的动容瞬间，以电影的方式记录一生。
resources:
  - src: cover.jpg
    params:
      cover: true # 首页封面，也用于社交分享预览图
menus:
  main:
    name: 主页
    weight: -1
cascade:
  build:
    # 只发布压缩后的图片，不发布原图（同时去掉下载按钮和照片元数据）。
    # 想让访客下载原图，把这一行改成 true。
    publishResources: false
---
