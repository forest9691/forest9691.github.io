# 数据库层文档

本项目使用 JSON 文件临时替代真实 MySQL 数据库，便于快速开发和原型验证。

## 目录结构

```
src/database/
├── setting.json           # 系统配置信息
├── user.json              # 用户配置信息
├── category.json          # 分类定义文件
├── website.json           # 网站数据示例文件
├── websiteList/           # 各分类网站数据目录
│   ├── video.json         # 视频分类网站数据
│   ├── news.json          # 新闻分类网站数据
│   ├── military.json      # 军事分类网站数据
│   ├── sports.json        # 体育分类网站数据
│   ├── games.json         # 游戏分类网站数据
│   ├── shopping.json      # 购物分类网站数据
│   ├── mall.json          # 商城分类网站数据
│   ├── auto.json          # 汽车分类网站数据
│   ├── finance.json       # 财经分类网站数据
│   ├── bank.json          # 银行分类网站数据
│   ├── travel.json        # 旅游分类网站数据
│   ├── tech.json          # 科技分类网站数据
│   ├── email.json         # 邮箱分类网站数据
│   ├── health.json        # 健康分类网站数据
│   ├── literature.json    # 文学分类网站数据
│   ├── music.json         # 音乐分类网站数据
│   ├── life.json          # 生活分类网站数据
│   ├── community.json     # 社区分类网站数据
│   ├── flashgame.json     # 闪游分类网站数据
│   ├── recruitment.json   # 招聘分类网站数据
│   ├── dating.json        # 交友分类网站数据
│   ├── mobile.json        # 手机分类网站数据
│   ├── food.json          # 美食分类网站数据
│   └── realestate.json    # 房产分类网站数据
└── Readme.md              # 本文件
```

## 网址分类数据格式【严格必须遵守】

`categories`中的`id`初始化(顶级分类 id)为`9691001`,二级是`9691001001`，三级是`9691001001001`,依次类推

```json
{
  "version": "1.0.0",
  "lastUpdated": "2024-05-24T10:00:00Z",
  "categories": [
    {
      "id": "96910001",
      "categoryCode": "video",
      "categoryName": "视频",
      "description": "优秀视频",
      "icon": "categoryCode-96910001",
      "parentId": null,
      "childCount": 10,
      "visitCount": 0,
      "children": [
        {
          "id": "96910001001",
          "categoryCode": "entertainment",
          "categoryName": "娱乐视频",
          "description": "娱乐类视频",
          "icon": "categoryCode-96910001001",
          "parentId": "96910001",
          "childCount": 10,
          "visitCount": 0,
          "children": [
            {
              "id": "96910001001001",
              "categoryCode": "movies",
              "categoryName": "电影",
              "description": "电影视频",
              "icon": "categoryCode-96910001001001",
              "parentId": "96910001001",
              "childCount": 0,
              "visitCount": 0
            }
          ]
        }
      ]
    }
  ]
}
```

### 字段说明

| 字段           | 类型        | 说明                                                                                  |
| -------------- | ----------- | ------------------------------------------------------------------------------------- |
| `id`           | string      | 分类唯一标识符，层级递增规则：一级`96910001`、二级`96910001001`、三级`96910001001001` |
| `categoryCode` | string      | 分类英文名，用于 URL、API 路由等场景（如：video、entertainment、movies）              |
| `categoryName` | string      | 分类中文名称，用于界面展示                                                            |
| `description`  | string      | 分类描述                                                                              |
| `icon`         | string      | 分类图标标识                                                                          |
| `parentId`     | string/null | 父级分类 ID，顶级分类为 null                                                          |
| `childCount`   | number      | 直接子分类数量                                                                        |
| `visitCount`   | number      | 访问次数统计                                                                          |
| `path`         | string      | 关联的网站数据文件路径（如：database/websiteList/video.json）                         |
| `children`     | array       | 子分类数组                                                                            |

## 网站数据格式【严格必须遵守】

### 文件命名规则

网站数据按分类 `categoryCode` 分文件存储，命名格式为 `{categoryCode}.json`：

- `video` 分类 → `video.json`
- `entertainment` 分类 → `entertainment.json`
- `movies` 分类 → `movies.json`

### 文件路径关联

每个分类在 `category.json` 中通过 `path` 字段关联到对应的网站数据文件，使用项目路径别名 `@/database/`：

```json
{
  "id": "96910001",
  "categoryCode": "video",
  "categoryName": "视频",
  "path": "database/websiteList/video.json",
  "...": "..."
}
```

### ID 初始化规则

`websites`中的`id`初始化规则：顶级`9691000100100100001`，根据所属分类层级依次扩展

```json
{
  "version": "1.0.0",
  "lastUpdated": "2024-05-24T10:00:00Z",
  "websites": [
    {
      "id": "9691000100100100001",
      "description": "优秀视频",
      "icon": "categoryCode-9691000100100100001",
      "parentId": "96910001001001",
      "categoryName": "腾讯视频",
      "categoryCode": "video",
      "visitCount": 0
    }
  ]
}
```

### 字段说明

| 字段                   | 类型   | 说明                                              |
| ---------------------- | ------ | ------------------------------------------------- |
| `id`                   | string | 网站唯一标识符，关联到所属分类层级                |
| `categoryName`         | string | 网站名称，用于界面展示                            |
| `description`          | string | 网站描述                                          |
| `icon`                 | string | 网站图标标识                                      |
| `parentId`             | string | 所属分类 ID（关联 categories 的 id）              |
| `categorycategoryCode` | string | 所属分类的 categoryCode（冗余字段，便于快速查询） |
| `visitCount`           | number | 网站访问次数统计                                  |
