# Baize Providers

Baize 的远端提供商目录。服务端从此仓库读取新增预设；应用自带的 `providers.jsonc` bundle 仍提供离线目录和模型能力表。

## 数据源

```text
https://raw.githubusercontent.com/songxuyao/baize-providers/main/profiles/providers.jsonc
```

`profiles/providers.jsonc` 是 JSONC 格式的 v2 目录。根节点 `_meta.schemaVersion` 必须为 `2`；`providers` 记录目录主体，`variants` 记录可配置的接入方式。旧版 `providers` 对象键对应新版 **Variant ID**，迁移时不得更名。Provider ID 与 Variant ID 是不同命名空间，同名也不代表相同实体。

```jsonc
{
  "_meta": { "schemaVersion": 2 },
  "providers": {
    "bailian": {
      "name": "Bailian",
      "catalogGroup": "cloud",
      "homepage": "https://bailian.console.aliyun.com"
    }
  },
  "variants": {
    "bailian": {
      "ownerProviderId": "bailian",
      "preset": {
        "name": "Bailian",
        "kind": "official",
        "authMode": "api_key",
        "region": "cn",
        "apiFormat": "anthropic",
        "endpoint": "https://dashscope.aliyuncs.com/apps/anthropic"
      }
    },
    "bailian-for-coding": {
      "ownerProviderId": "bailian",
      "preset": {
        "name": "Bailian For Coding",
        "kind": "official",
        "authMode": "api_key",
        "region": "cn",
        "apiFormat": "anthropic",
        "endpoint": "https://coding.dashscope.aliyuncs.com/apps/anthropic"
      }
    }
  }
}
```

| 字段 | 说明 |
| --- | --- |
| `_meta.schemaVersion` | 必须为 `2` |
| `providers.<id>.name` | 提供商显示名 |
| `providers.<id>.catalogGroup` | `cloud` / `local` / `custom` |
| `providers.<id>.homepage` / `logoUrl` | 可选的官网与图标 URL |
| `variants.<id>.ownerProviderId` | 必须引用现有 Provider；接入项 ID 独立且稳定 |
| `variants.<id>.placements` | 可选的其他展示 Provider ID 数组；不得包含归属方或重复值 |
| `variants.<id>.preset.name` | 接入方式显示名 |
| `variants.<id>.preset.kind` | `official` / `relay` / `aggregator` / `local` / `custom` / `unknown` |
| `variants.<id>.preset.authMode` | `api_key` / `local_endpoint` / `local_endpoint_key` / `chatgpt_oauth` |
| `variants.<id>.preset.region` | `cn` / `us` / `eu` / `global` |
| `variants.<id>.preset.apiFormat` | `openai` / `anthropic` / `gemini` / `openai_responses`；`openai` 仍表示 Chat Completions |
| `variants.<id>.preset.endpoint` | API 根 URL |
| `variants.<id>.preset.homepage` / `logoUrl` / `modelsCatalogUrl` / `notes` / `modelIds` | 可选的原有预设资料 |

当前文件保留旧版预设的 `category`、`type` 字段供兼容读取；`kind` 与 `authMode` 是新版接入信息。远端目录只发布预设与展示关系，不发布模型能力、密钥或授权配置。模型的 `contextWindow`、`reasoningSupport` 和 `fastModeByEndpoint` 仍由 Baize 本地 bundle 管理。未经确认的关联不靠名称、端点或模型 ID 猜测归属。

## 添加接入方式

1. 在 `providers` 添加新主体，或选择已存在的明确归属 Provider。
2. 在 `variants` 添加唯一且稳定的接入项 ID；填写 `ownerProviderId` 和完整 `preset`。
3. 如需从其他 Provider 页面发现该接入项，显式添加 `placements`，并确认所有引用存在。
4. 保持原有 Variant ID 不变，验证 JSONC 与 Baize 的 v2 解析器，再提交 PR 说明端点与资料来源。

## License

MIT — 欢迎提交端点可用且资料准确的提供商预设。
