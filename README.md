# Clash

本仓库收录 Clash（mihomo）的通用配置。分流规则不再单独维护，直接引用 [JupiterH/Surge](https://github.com/JupiterH/Surge) 仓库 `Rule/` 下的 `.list` 文件，两边始终保持一致。

## 目录结构

| 路径 | 内容 |
|---|---|
| `Universal.yaml` | 通用配置文件，与 Surge 仓库的 `Universal.conf` 一一对应，填入机场订阅即可使用，详见[通用配置](#通用配置) |

## 通用配置

`Universal.yaml` 由 Surge 版通用配置转换而来，适用于 mihomo（Clash.Meta）1.19 及以上版本的内核，可在 Clash Verge Rev、Mihomo Party、FlClash 等客户端中使用。配置中不含自建节点、个人直连规则等与个人环境相关的内容。

使用方法：

1. 将 `proxy-providers` 中「订阅」的 `url` 替换为机场订阅地址。各地区分组均从该订阅中按节点名称筛选，无需逐一修改。
2. 自定义规则写在 `rules` 开头的注释处，自建节点写在 `proxies` 中（配置内附有示例注释）。
3. TUN 模式默认关闭，建议在客户端界面中开启；客户端的设置会覆盖配置中的 `tun` 段。

策略组与 Surge 版相同：

| 策略组 | 默认 | 用途 |
|---|---|---|
| 🚀 节点选择 | ♻️ 自动选择 | 默认代理出口，同时用于 CDN 与测速 |
| 🤖 AI | 🇺🇸 美国 | OpenAI、Claude、Gemini 及 Apple Intelligence |
| 📺 流媒体 | 🚀 节点选择 | 各地区流媒体 |
| ✈️ Telegram | 🚀 节点选择 | Telegram 域名与 IP |
| 🍎 Apple / Ⓜ️ 微软 | 直连 | 系统服务与国内 CDN |
| 🅰️ Adobe | 直连 | 可切换为 REJECT 以阻止联网验证 |
| 💳 PayPal | 🇺🇸 美国 | PayPal、Venmo、Braintree |
| ⬇️ 下载 | 🚀 节点选择 | 软件更新与系统镜像等大文件，可指定低倍率节点 |
| 🌐 国外网站 | 🚀 节点选择 | Google、GitHub、Twitter 等常见国外服务 |
| 🐟 漏网之鱼 | 🚀 节点选择 | 未命中任何规则的流量 |

地区分组包括香港、台湾、日本、新加坡、美国。

与 Surge 版的差异：

- 地区分组使用 `url-test` 类型。官方 mihomo 内核不支持 Surge 的 `smart` 类型（仅 vernesong 的分支版本支持）。
- 兜底规则为 `MATCH`，Surge 中 `FINAL` 的 `dns-failed` 参数在 mihomo 中没有对应项。
- 引用自 JupiterH/Surge 的规则集（`Reject.list`、`adobe.list`、`paypal.list`）经 jsDelivr 加速，国内可直连下载。jsDelivr 对分支内容有缓存，规则集更新后可能延迟一段时间生效。

分流规则的顺序依照 [SukkaW/Surge](https://github.com/SukkaW/Surge) 的说明：所有 `domainset` 与 `non_ip` 规则均置于 `ip` 规则之前，以避免对需要代理的域名提前发起 DNS 解析。

## 引用规则集

在自己的 mihomo 配置中引用本仓库的规则集时，使用 `format: text`、`behavior: classical`：

```yaml
rule-providers:
  adobe:
    type: http
    behavior: classical
    format: text
    url: https://cdn.jsdelivr.net/gh/JupiterH/Surge@main/Rule/adobe.list
    path: ./ruleset/adobe.list
    interval: 86400

rules:
  - RULE-SET,adobe,Adobe
```

可引用的文件及用途见 [JupiterH/Surge 的规则集说明](https://github.com/JupiterH/Surge#规则集)。`rules` 中的策略名须替换为自己配置中的策略组名。

## 说明

- `.list` 中以 `#` 开头的注释行，mihomo 会自动忽略。
- `Reject.list` 使用了 `DOMAIN-WILDCARD` 规则类型，mihomo 1.19 及以上版本支持。
- `paypal.list` 中有一条 `USER-AGENT,PayPal*`，mihomo 不支持该规则类型，加载时会跳过并在日志中给出警告，不影响其他规则。
- 原来的 `Rules/*.yaml` 已删除，仍在引用旧地址的配置需要改为上述写法。
