# easyP
常用约饭表情包快速合成


## 添加新图

- 基底图放在 public/base/xxx
- 子图元放在 public/item/xxx
- 手动更新索引 src/assets/index_xxx.json
  - 索引可以乱序，图片命名不影响调用（但推荐遵循格式（3位数字））


## 添加新的分类
- assets/index_xxx.json添加json
- public添加对应图片文件夹
- HomeView.vue import json
- HomeView.vue添加html
  - 注意修改name和图片路径


## 更新记录

- 2023.10.23 增加canvas绘图
- 2023.8.1 大概是这一天创建的项目