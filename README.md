# Baize Providers

Baize 应用(`https://github.com/yourname/baize` 等)的厂商预设数据。

## 数据源

服务端从这个仓的 raw URL 拉取厂商列表:

```
https://raw.githubusercontent.com/songxuyao/baize-providers/main/profiles/providers.yaml
```

当 GitHub 不可达时,Baize 会回退到打包的 vendor-fallback.yaml(只有 OpenAI / Anthropic / Google / DeepSeek / Moonshot 5 项)。

## Schema

`profiles/providers.yaml` 字段说明(必填字段用 `*`):

| 字段 | 类型 | 说明 |
|---|---|---|
| `id` * | string | kebab-case 唯一 ID,作为数据库主键 |
| `name` * | string | 显示名 |
| `category` * | enum | `official` / `cn_official` / `aggregator` / `third_party` |
| `region` * | enum | `cn` / `us` / `eu` / `global` |
| `apiFormat` * | enum | `openai` / `anthropic` / `gemini` |
| `endpoint` * | URL | 厂商 API 根 URL(Baize 自动拼 `/v1/models` 拉模型列表) |
| `homepage` | URL | 厂商主页(UI 展示) |
| `logoUrl` | URL | Logo 图标 URL(可选,空则用首字母占位) |
| `modelsCatalogUrl` | URL | 模型列表专用端点(可选,空则用 `{endpoint}/v1/models`) |
| `notes` | string | 备注,UI 详情面板展示 |

## 添加新厂商

1. Fork 这个仓
2. 编辑 `profiles/providers.yaml`,**保持字段顺序** + YAML 缩进对齐
3. 提交 PR,标题写明厂商名
4. CI 会自动跑 schema 校验(`scripts/validate-yaml.sh` 如果存在)
5. 合并后 Baize 应用下次启动拉取最新列表

## 厂商数量

当前 `profiles/providers.yaml` 包含 ~22 个常用厂商。完整分类参考 `docs/baize-providers.md`。

## License

MIT — 任何人都可以提交补充,只要厂商 endpoint 真实可达。