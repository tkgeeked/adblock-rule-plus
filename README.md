# 广告拦截 + 分流规则集 (Shadowrocket)

这是一个适用于 **Shadowrocket** 的完整代理规则集，集成了广告拦截、国内外分流、YouTube去广告三大功能，支持**每日自动更新**，无需手动维护。

## 规则集列表

### AdBlock Plus 规则（`adblockplus.conf`）

- **🔄 自动更新**：GitHub Actions 每天北京时间 06:30 自动同步最新规则
- **🚫 广告拦截**：约 258,718 条广告域名拦截规则（含 YouTube 相关域名拦截）
- **🌍 国内外分流**：Telegram、Disney、Amazon、Apple 等国外服务走代理

**订阅 URL**：
```
https://raw.githubusercontent.com/tkgeeked/adblock-rule-plus/main/adblockplus.conf
```

## AdBlock Plus 规则结构

| 规则类型 | 数量 | 来源 |
|---------|------|------|
| 广告拦截 | ~258,718 条 | [217heidai/adblockfilters](https://github.com/217heidai/adblockfilters) |
| 国内外分流 | ~411 条 | Telegram、Disney、Amazon、Apple 等 |
| IP/CIDR 规则 | 17 条 | Telegram、Google Voice 等 |
| GEOIP 规则 | 1 条 | 中国IP直连 |

## YouTube 去广告说明

> ⚠️ **重要说明**：规则中包含 YouTube 相关广告域名拦截（如 `ads.youtube.com`、`doubleclick.net` 等），但 **YouTube APP 内的视频广告无法拦截**。
>
> **原因**：YouTube APP 的视频广告和正常视频内容从**同一个域名**（`googlevideo.com`）下载，DNS/域名级别的规则无法区分广告视频和正常视频。如果拦截整个 `googlevideo.com`，你将无法观看任何视频。
>
> **适用范围**：
> - ✅ **网页版有效**：配合浏览器广告拦截扩展（如 uBlock Origin）可拦截网页版广告
> - ❌ **APP 内无效**：YouTube 官方 APP 内的视频广告无法通过域名规则拦截
>
> **解决方案**：如需 APP 去广告，可考虑：
> - 第三方 YouTube 客户端（如 uYouEnhanced、Revanced、NewPipe 等）
> - YouTube Premium 订阅

## 使用方法

### 1. 在 Shadowrocket 中导入

1. 打开 **Shadowrocket**
2. 进入「配置」页面
3. 点击「+」添加配置
4. 选择「添加订阅」或「从 URL 导入」
5. 粘贴上面的 URL（选一个或两个都导入）
6. 下载并启用该规则集

### 2. 启用规则

- 在 Shadowrocket 的「路由」设置中，启用导入的规则
- 推荐使用「规则模式」

## 规则逻辑

### AdBlock Plus

1. **国内直连优先**（.cn 域名、微信、银联、网银、苹果国内 CDN 及常用 App 等） → 直连（DIRECT，优先匹配）
2. **广告域名** → 直接拦截（REJECT-DROP）
3. **国外服务**（Telegram、Disney、Amazon、部分 Apple 国外服务等） → 走代理（PROXY）
4. **中国 IP** → 直连（GEOIP,CN,DIRECT）
5. **其他所有** → 走代理（FINAL,PROXY）

## 自动更新

规则会**每天自动更新**，无需手动操作：

### AdBlock Plus
- **更新时间**：每天北京时间 06:30
- **更新来源**：[217heidai/adblockfilters](https://github.com/217heidai/adblockfilters)

如果你想**立即更新规则**：
1. 访问 [GitHub Actions 页面](https://github.com/tkgeeked/adblock-rule-plus/actions)
2. 选择对应的工作流：
   - `Auto Update AdBlock Rules` - 更新 AdBlock Plus 规则
3. 点击「Run workflow」手动触发
4. 等待几分钟，规则会自动同步并推送

## 规则格式

本规则文件采用 **Shadowrocket 格式**：

### AdBlock Plus

```ini
[General]
bypass-system = true
skip-proxy = 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, localhost, *.local, captive.apple.com, e.crashlytics.com
bypass-tun = 10.0.0.0/8,100.64.0.0/10,127.0.0.0/8,169.254.0.0/16,172.16.0.0/12,192.0.0.0/24,192.0.2.0/24,192.88.99.0/24,192.168.0.0/16,198.18.0.0/15,198.51.100.0/24,203.0.113.0/24,224.0.0.0/4,255.255.255.255/32
dns-server = 223.5.5.5, 119.29.29.29, system
update-url = https://raw.githubusercontent.com/tkgeeked/adblock-rule-plus/main/adblockplus.conf

[Rule]
# 国内直连优先匹配（微信、网银、国内加速服务等）
DOMAIN-SUFFIX,cn,DIRECT
DOMAIN-SUFFIX,unionpay.com,DIRECT
DOMAIN-SUFFIX,apple.com,DIRECT

# 广告拦截（含 YouTube 相关域名）
DOMAIN-SUFFIX,ads.youtube.com,REJECT-DROP
DOMAIN-SUFFIX,doubleclick.net,REJECT-DROP

# 国外服务代理
DOMAIN-SUFFIX,telegram.org,PROXY
IP-CIDR,91.108.56.0/22,PROXY

# GEOIP 兜底与 FINAL
GEOIP,CN,DIRECT
FINAL,PROXY

[URL Rewrite]
^https?://(www.)?(g|google)\.cn https://www.google.com 302

[MITM]
hostname = *.google.cn,*.googlevideo.com
```

## 版本信息

### AdBlock Plus
- **规则格式**：Shadowrocket
- **自动更新**：✅ 是（每日 06:30 北京时间）
- **规则总数**：约 259,130 条
- **文件大小**：约 11.45 MB

## 注意事项

- 本规则**专为 Shadowrocket 设计**，不适用于 Clash 等其他客户端
- 规则数量较多，可能对设备性能有一定影响
- 部分网站可能需要手动调整规则以确保正常访问
- 如果导入后无法访问某些网站，请检查规则是否加载成功
- **YouTube APP 内的视频广告无法通过域名规则拦截**，这是 YouTube 的技术限制
- 如果同时导入两个规则集，可能会有重复规则（不影响使用）

## 鸣谢

- **广告规则作者**：[217heidai/adblockfilters](https://github.com/217heidai/adblockfilters)
- **分流规则作者**：[Johnshall/Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever) 项目的 Moshel 和 Johnshall
- **自动更新**：GitHub Actions
- **仓库维护**：[tkgeeked](https://github.com/tkgeeked/adblock-rule-plus)
