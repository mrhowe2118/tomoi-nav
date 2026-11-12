<div align="center">
  <img src="assets/tomshengo.svg" alt="Tom 信奥" width="96">
  <h1>Tom 信奥导航</h1>
  <p>信息学竞赛学习资源导航 · 题库 / 学习 / 工具一站直达</p>
</div>

线上地址：**https://mrhowe2118.github.io/menav-oj/**

基于 [MeNav](https://github.com/rbetree/menav)（AGPL-3.0）构建的静态导航站，纯 YAML 配置，无需数据库和后端。

---

## 只改一个目录

所有个性化内容都在 `config/user/` 下。该目录存在时会**完全替换** `config/_default/`，因此这里放的是完整配置，不是补丁。

```
config/user/
├── site.yml            站点名、导航菜单、主题、社交链接
└── pages/
    ├── home.yml        首页（导航第一项）
    ├── oj.yml          题库
    ├── learn.yml       学习资料
    ├── tools.yml       工具
    └── content.yml     关于页（正文在 content/about.md）
```

## 怎么加一个网站

在对应页面的 `categories` 里，往 `sites` 下追加一项：

```yaml
      - name: 站点名称
        url: https://example.com
        icon: fas fa-code         # favicon 抓不到时的备用图标（Font Awesome 6）
        description: 一句话说明它解决什么问题
```

图标不需要手动找：`site.yml` 里 `icons.mode: favicon` 会自动按域名抓取，抓不到才回退到 `icon` 字段。

## 怎么加一个页面

1. 在 `config/user/pages/` 新建 `xxx.yml`，复制现有页面的结构
2. 在 `site.yml` 的 `navigation` 里追加一项，`id` 与文件名一致：

```yaml
  - name: 菜单名
    icon: fas fa-star
    id: xxx
```

导航顺序即菜单顺序，**第一项自动是首页**。

## 本地预览

需要 Node.js ≥ 22.23.4（CI 用 24）。

```bash
npm ci
npm run build                 # 生成 dist/
python -m http.server 8899 -d dist   # 然后打开 http://127.0.0.1:8899
```

改完配置重新 `npm run build` 即可，无需重启服务。

## 部署

推送到 `main` 分支后，GitHub Actions 自动构建并发布到 Pages，无需手动操作。

首次部署需要在仓库里做两件事（已完成则跳过）：

1. **Actions** 标签页 → 启用工作流
2. **Settings → Pages → Source** 选择 `GitHub Actions`

工作流见 `.github/workflows/deploy.yml`，默认每天 UTC 02:00 定时重建一次。

## 设计约定

| 项目 | 取值 | 原因 |
| :--- | :--- | :--- |
| 主题 | 暗色 | 长时间看屏，与教学课件风格统一 |
| 主色 | `#38d9a9` 青绿 | 与站点图标同源 |
| 字体 | 系统黑体栈（Noto Sans SC → 苹方 → 微软雅黑） | 零字体请求，国内首屏最快 |
| 代码字体 | JetBrains Mono → Cascadia → Consolas | 讲解代码时字重清晰 |
| 卡片布局 | 桌面 4 列 / 手机单列 | 手机上描述能完整显示 |

品牌覆盖层在 `assets/styles/brand.css`，只覆盖变量与少量排版，不改动上游组件结构。

## 相对上游的改动

- 替换全部站点配置为信奥教学资源
- 新增 `assets/tomshengo.svg` 站点图标与 `assets/styles/brand.css` 品牌层
- 页脚版权改为站点自身的 `site.title`
- 首页问候语、日期、待办默认项本地化为中文
- 搜索框提示文案本地化
- 关闭 RSS 聚合与 GitHub 项目墙（对教学场景无价值，减少构建耗时）
- 删除未使用的上游示例图片

## 许可

本项目沿用上游 AGPL-3.0。配置内容与站点设计归 Tom 信奥所有。
