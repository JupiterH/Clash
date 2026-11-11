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

## 免责声明

以下所称「本仓库」包括本仓库及与之相关的仓库（如 [JupiterH/Surge](https://github.com/JupiterH/Surge)、[JupiterH/Clash](https://github.com/JupiterH/Clash)），「本仓库内容」指本仓库中分享的配置、规则、模块、脚本及文档等全部内容。

- 本仓库内容仅供个人学习与研究网络技术使用，不提供任何形式的担保。禁止用于商业用途，或任何违反所在国家、地区、组织的法律法规及相关规定的用途。
- 未经许可，禁止任何公众号、自媒体以任何形式转载、发布本仓库内容。
- 使用者应自行遵守所在国家或地区的法律法规，自行判断本仓库内容是否合法、适用。因使用本仓库内容产生的任何后果（包括但不限于数据丢失、隐私泄露、账号异常、设备故障）由使用者自行承担，作者不承担任何责任。
- 本仓库内容涉及的域名、接口等信息均可通过公开渠道或浏览器开发者工具获得，未经过逆向工程或网络攻击，不构成对计算机系统的入侵。
- 本仓库内容涉及的第三方软件、硬件及服务（如 Surge、Clash、各类代理服务）与本仓库没有任何直接或间接关系。本仓库仅客观描述其配置与使用方式，不代表支持或推荐使用任何第三方产品，使用第三方产品造成的一切后果由使用者自行承担。
- 本仓库中部分规则、模块和脚本来自或改编自第三方项目，版权归原作者所有，已尽量在文件内注明来源。如任何单位或个人认为本仓库内容涉嫌侵犯其权利，请及时联系作者并提供身份证明与权属证明，核实后会及时删除相关内容。
- 任何人基于本仓库内容所做的修改与再分发，均为其个人行为，与本仓库及作者无关，由此产生的后果亦与本仓库及作者无关。
- 规则与脚本可能随上游或目标服务的变化而失效，作者不保证其准确性、完整性、有效性和时效性。
- 请勿在中华人民共和国境内使用本仓库内容。
- 直接或间接使用本仓库内容的任何个人或组织，应在下载后 24 小时内完成学习与研究，并从存放或使用本仓库内容的任何硬件、软件及介质中完全删除。如有相关功能需求，请自行开发。
- 作者保留随时修改或补充本声明的权利。以任何方式查看、引用、下载、复制、修改或使用本仓库内容，即视为已阅读并同意本声明。
