# easyP
常用表情包快速合成


## 添加新图

- 基底图放在 public/base/xxx
- 子图元放在 public/item/xxx
- 手动更新 src/assets/index_xxx.json


## 添加新的分类
- assets/index_xxx.json添加json
- public添加对应图片文件夹
- HomeView.vue import json
- HomeView.vue添加html
  - 注意修改name和图片路径