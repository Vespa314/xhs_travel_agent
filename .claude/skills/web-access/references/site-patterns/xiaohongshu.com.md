---
domain: xiaohongshu.com
aliases: [小红书, xhs]
updated: 2026-04-18
---

## 平台特征

- 搜索结果页 URL 格式: `https://www.xiaohongshu.com/search_result?keyword=KEYWORD&type=1`（type=1 为综合排序，type=51 为默认）
- 笔记卡片使用 `section.note-item` 选择器
- 笔记标题链接用 `a.title` 选择器，封面链接用 `a.cover` 或 `a[href*=search_result]`
- 搜索结果页 body.innerText 可能只返回页脚信息，需要用 DOM 选择器精确定位内容
- 笔记详情页内容可通过 `document.body.innerText` 提取，正文在页脚之前、导航栏之后
- 笔记链接格式: `https://www.xiaohongshu.com/search_result/NOTE_ID?xsec_token=TOKEN&xsec_source=`
- xsec_token 是访问必需参数，不能省略

## 有效模式

- 用 `/new` 打开搜索结果页后等待 3-4 秒加载完成
- 用 `section.note-item` 选择器获取笔记列表，遍历提取标题和链接
- 笔记详情页可直接通过 `/new` + 完整 URL（含 xsec_token）打开
- 笔记正文可通过 body.innerText 提取，包含完整的图文内容和评论

## 已知陷阱

- (2026-04-18) URL 中的 type 参数可能会被平台自动修改，搜索词也会变化，需要确认实际加载的页面内容
- (2026-04-18) body.innerText 在搜索列表页可能只返回页脚法律声明文本，必须用 DOM 选择器
- (2026-04-18) xsec_token 为必需参数，截断或省略会导致无法访问
- (2026-04-18) 用 `/new` + `/explore/{id}` 短链接打开笔记，页面不加载笔记内容而显示首页推荐流。必须使用搜索结果中的完整 xsec_token URL（`/search_result/{id}?xsec_token=xxx`）
- (2026-04-18) 笔记正文提取优先用 `document.querySelector("#detail-desc")?.innerText`，比 body.innerText 更精准
- (2026-04-18) 从搜索结果提取笔记链接时，每个笔记卡片有两个链接：`/explore/{id}`（短链接，无效）和 `/search_result/{id}?xsec_token=xxx`（完整链接，有效），应使用后者
