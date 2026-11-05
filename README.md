# Clash

Clash（mihomo）规则不再单独维护，直接引用 [JupiterH/Surge](https://github.com/JupiterH/Surge) 仓库 `Rule/` 下的 `.list` 文件，两边始终保持一致。

## rule-providers

在 mihomo 配置里这样写（`format: text`，`behavior: classical`）：

```yaml
rule-providers:
  Direct:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/JupiterH/Surge/main/Rule/Direct.list
    path: ./ruleset/Direct.list
    interval: 86400
  us:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/JupiterH/Surge/main/Rule/us.list
    path: ./ruleset/us.list
    interval: 86400
  adobe:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/JupiterH/Surge/main/Rule/adobe.list
    path: ./ruleset/adobe.list
    interval: 86400
  paypal:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/JupiterH/Surge/main/Rule/paypal.list
    path: ./ruleset/paypal.list
    interval: 86400

rules:
  - RULE-SET,Direct,DIRECT
  - RULE-SET,us,美国节点
  - RULE-SET,adobe,Adobe
  - RULE-SET,paypal,美国节点
```

`rules` 里的策略名换成自己配置中的策略组名。

## 说明

- `.list` 中以 `#` 开头的注释行 mihomo 会自动忽略。
- `paypal.list` 里有一条 `USER-AGENT,PayPal*`，mihomo 不支持这种规则，加载时会跳过并在日志里给一条警告，不影响其他规则。
- 原来的 `Rules/*.yaml` 已删除；还在引用旧地址的配置需要换成上面的写法。
