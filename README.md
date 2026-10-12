# Baize Providers

Baize 的远端 provider preset 目录。服务端从此仓库读取新预设；应用自带的 `providers.jsonc` bundle 仍提供离线 preset 和模型能力表。

## 数据源

服务端从以下 raw URL 拉取 provider presets：

```text
https://raw.githubusercontent.com/songxuyao/baize-providers/main/profiles/providers.jsonc
```

远端文件使用 JSONC，包含 provider 的 `preset` 字段。模型能力数据由 Baize 应用本地 bundle 管理，不放在此仓库。

## Schema

`profiles/providers.jsonc` 的根节点为 `providers` 对象；provider ID 是对象键，preset 字段为 camelCase。`models` 可以省略。

```jsonc
{
  "providers": {
    "deepseek": {
      "preset": {
        "name": "DeepSeek",
        "category": "cn_official",
        "region": "cn",
        "apiFormat": "openai",
        "endpoint": "https://api.deepseek.com"
      }
    }
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| provider ID（对象键）* | string | kebab-case 唯一 ID，作为数据库主键 |
| `preset.name` * | string | 显示名 |
| `preset.category` * | enum | `official` / `cn_official` / `aggregator` / `third_party` / `custom` |
| `preset.region` * | enum | `cn` / `us` / `eu` / `global` |
| `preset.apiFormat` * | enum | `openai` / `anthropic` / `gemini` |
| `preset.endpoint` * | URL | 厂商 API 根 URL |
| `preset.homepage` | URL | 厂商主页 |
| `preset.logoUrl` | URL | Logo 图标 URL |
| `preset.modelsCatalogUrl` | URL | 模型列表专用端点 |
| `preset.notes` | string | 备注 |
| `preset.type` | enum | `official` / `relay` / `local` / `custom` / `aggregator` |
| `preset.modelIds` | string[] | 可选模型 ID 列表 |

必填字段用 `*` 标记。JSONC 注释可用于说明数据；属性值仍须符合 JSON 类型。

## 添加新厂商

1. Fork 此仓库。
2. 在 `profiles/providers.jsonc` 的 `providers` 对象中新增 provider ID 和 `preset`。
3. 保持 provider ID 唯一，填写必填字段并验证 JSONC。
4. 提交 PR，说明 endpoint 与厂商资料来源。
5. 合并后，Baize 会在后续目录刷新时读取该 preset。

## License

MIT — 欢迎提交 endpoint 可用且资料准确的 provider preset。
