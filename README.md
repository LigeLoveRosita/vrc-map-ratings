# vrc-map-ratings

[VRC 地图图鉴](https://github.com/LigeLoveRosita/main-vrc) 的 **推荐 + 评分** 数据仓库。

站点直接读取本仓库的 JSON。**改这里的内容，刷新网页即刻生效，不需要重新部署站点。**

## 文件

| 文件 | 内容 |
| --- | --- |
| `data/maps.json` | 地图清单（名称、作者、平台、标签、简介、链接、缩略图） |
| `data/ratings.json` | 用户评分（`ratings`）+ 收藏记录（`collections`） |

## 三种提交方式

1. **在网页上提交** — 「我的」页 → 同步到仓库，会生成一个预填好 JSON 的 Issue
2. **直接提 PR** — 按下面格式改 JSON 后提交 Pull Request
3. **开 Issue** — 打上 `submission` 标签，维护者会合并

## 新增一张地图

在 `data/maps.json` 的 `maps` 数组里追加：

```json
{
  "id": "my-world",
  "name": "地图名称",
  "author": "作者",
  "platform": ["PC", "Quest"],
  "capacity": 0,
  "tags": ["社交", "风景"],
  "thumbnail": "",
  "description": "一句话介绍",
  "url": "https://vrchat.com/home/world/wrld_xxx",
  "publishedAt": "2026-01-01"
}
```

### 字段约束

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `id` | 是 | **必须唯一**。建议小写英文加连字符。会出现在分享链接里（`#/map/the-great-pug`），**建好后不要改** |
| `name` | 是 | 显示名称 |
| `author` | 是 | 地图作者 |
| `platform` | 是 | 只能填 `PC` / `Quest` / `Android` / `iOS` |
| `capacity` | 否 | 人数上限，`0` 表示不限 |
| `tags` | 是 | 标签数组，1–4 个比较合适；重复使用已有标签能提高聚合效果 |
| `thumbnail` | 否 | 图片直链。留空则显示渐变色占位块 |
| `description` | 是 | 建议 60 字以内，卡片上只显示两行 |
| `url` | 是 | VRChat 世界链接 |
| `publishedAt` | 否 | `YYYY-MM-DD`，用于「新图榜」排序 |

## 新增一条评分

在 `data/ratings.json` 的 `ratings` 数组里追加：

```json
{ "mapId": "my-world", "user": "你的昵称", "stars": 5, "at": "2026-09-15T12:00:00Z" }
```

- `stars` 为 1–5 的整数
- 同一个 `mapId` + `user` 只保留最新一条，重复提交等于改分
- **不需要手工维护平均分**，站点会实时聚合

## 新增一条收藏

```json
{ "user": "你的昵称", "mapId": "my-world", "at": "2026-09-15T12:00:00Z" }
```

## 分数怎么算

- **详情页平均分** = 该地图全部评分的算术平均
- **榜单页高分榜** 额外做贝叶斯平滑（先验 4.0 分，权重 3），避免只有一两个人打满分的地图挤掉公认的好图

## 许可

数据以 CC0 释出，随便用。
