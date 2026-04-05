# 广告拦截 + 分流规则集

这是一个融合了广告拦截和国内外分流功能的代理规则集，基于以下规则融合而成：

## 规则来源

1. **广告拦截规则**：来自 `adblockclash.conf`，包含大量广告域名拦截
2. **国内外分流规则**：来自 `sr_cnip.conf`（Shadowrocket-ADBlock-Rules-Forever 项目）
3. **YouTube去广告规则**：针对YouTube广告的专门拦截规则

## 功能特性

- **广告拦截**：拦截各类广告域名和追踪器
- **国内外分流**：中国IP直连，其他走代理
- **YouTube去广告**：拦截YouTube视频广告和展示广告
- **常用服务优化**：针对Telegram、Google、Disney+等服务的代理规则
- **Google CN重定向**：自动将Google.cn重定向到Google.com

## 规则优先级

1. **广告域名** → 直接拦截（REJECT-DROP）
2. **特定代理域名** → 走代理（PROXY）
3. **中国IP** → 直连（DIRECT）
4. **其他所有** → 走代理（proxy）

## 鸣谢

- **原作者**：[Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever) 项目的 Moshel 和 Johnshall
- **广告规则来源**：adblockclash 规则集

## 使用方法

1. 下载 `adblockclash.conf` 文件
2. 在你的代理软件中导入该规则文件
3. 启用该规则集

## 规则结构

- **[Rule]** 部分：包含广告拦截、代理规则和分流规则
- **[URL Rewrite]** 部分：Google CN 重定向
- **[MITM]** 部分：Google 相关域名的 MITM 配置

## 版本信息

- 最后更新：2026-04-05
- 规则总数：约243,000行
- 包含：广告拦截 + 国内外分流 + YouTube去广告

## 注意事项

- 由于规则数量较多，可能会对设备性能有一定影响
- 部分网站可能需要手动调整规则以确保正常访问
- 定期更新规则以保持最佳效果