# OpenClash Private Config

用于个人 OpenClash / Mihomo 的多机场订阅聚合与分类分流配置。

## 方案结构

- **多个机场订阅**：作为独立 `proxy-providers` 接入，节点统一进入策略组。
- **节点模式**：提供自动容错、手动选择和自动测速最快三个入口。
- **流量分流**：引用 MetaCubeX 维护的 Mihomo 规则集，覆盖广告、国内直连、OpenAI、Telegram、YouTube、Netflix、GitHub、Microsoft 等。
- **敏感信息隔离**：仓库只保存模板；真实订阅地址写入本地副本，不提交 GitHub。

## 快速使用

1. 下载 `config/openclash-template.yaml`。
2. 复制为本地文件，例如 `openclash-private.yaml`。
3. 将 `SUBSCRIPTION_URL_A/B/C` 替换为各机场的 Clash/Mihomo 订阅地址。
4. 如果机场数量不同，可复制或删除对应的 `proxy-providers`。
5. 在 OpenClash 的配置管理中上传并启用该文件。
6. 先执行配置检查，再启动 OpenClash。

> 不建议把真实机场订阅 URL 提交到 GitHub。即使仓库是私有的，订阅 URL 通常仍等同于访问密钥。


## 节点模式

- `自动选择`：按节点顺序使用第一个健康节点；当前节点失效时自动切换。
- `手动选择`：显示三家机场的全部节点，由你手动指定。
- `自动最快`：每 5 分钟测速一次，自动使用当前延迟最低的节点。

## 自定义“杂项”分流

主配置的 `rule-providers.misc.payload` 是你的私人规则区。默认的 `example.invalid` 不会匹配真实网站，可以替换或追加，例如：

```yaml
payload:
  - DOMAIN-SUFFIX,example.com
  - DOMAIN,api.example.net
  - IP-CIDR,203.0.113.0/24,no-resolve
```

这些规则会进入 `杂项` 策略组。自定义规则位于国内直连规则之前，因此适合添加必须访问外网的网站。

## 机场订阅兼容性

机场订阅最好直接返回 Clash/Mihomo 的节点列表。如果某家返回完整配置、格式不兼容或需要统一重命名，可先通过自托管 [Sub-Store](https://github.com/sub-store-org/Sub-Store) 转换为 Mihomo/Clash.Meta 输出，再把生成的地址填入本模板。

## 上游项目

详见 [UPSTREAMS.md](UPSTREAMS.md)。本仓库只保存自编配置和上游链接，不复制上游完整规则库。

## 文件说明

- `config/openclash-template.yaml`：OpenClash 主配置模板
- `config/subscriptions.example.yaml`：增加/删除机场的示例片段
- `UPSTREAMS.md`：上游来源与维护说明
- `.gitignore`：阻止本地密钥及私人配置被误提交
