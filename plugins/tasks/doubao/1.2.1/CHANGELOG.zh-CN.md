---
changelogVersion: 1
plugin: "doubao"
version: "1.2.1"
locale: "zh-CN"
---
# Changelog

## [1.2.1]

### Added

- 新增 `doubao-seedream-5-0-flash-260915`，可通过豆包原生图片接口和 OpenAI Images API 生成、编辑图片，支持 1K、1.5K、2K 输出，最多传入 10 张参考图。
- Seedream 5.0 Flash 支持图层拆分、透明背景编辑，以及 PNG 或 JPEG 输出。图层拆分要求恰好一张输入图；透明输出要求一张参考图并使用 PNG。该模型不支持组图生成。
- 通过 OpenAI Images API 使用 Agent Plan 模型名 `doubao-seedream-5-0-pro` 和兼容别名 `doubao-seedream-5.0-pro`。这些别名使用 `/api/plan/v3/images/generations`，带日期的方舟模型 ID 继续使用 `/api/v3/images/generations`。

### Fixed

- 对最小输出尺寸高于 1.5K 的 Seedream 模型，即使响应未返回图片尺寸，也按实际返回的图片数量报告 `images_above_1_5k` 用量，不再沿用请求时的图片数量。
- 不支持参考视频的 Seedance 模型会报告 `video_input: "none"`，使已有的用量计费规则继续匹配无参考视频条件。
- 在任务插件注册表中声明两个 Agent Plan 模型名，使生图请求由插件处理，不再回退到不相关的渠道适配器。

### Migration

- 为要提供给用户的 Agent Plan 模型名（`doubao-seedream-5-0-pro` 或 `doubao-seedream-5.0-pro`）配置 Agent Plan 资费。使用分层任务计费时，表达式应兼容 `u("images_up_to_1_5k")`、`u("images_above_1_5k")`、`u("input_images")` 和 `u("layer_decomposition")`。
- 如果生产环境已安装旧的 `doubao@1.2.1` 覆盖插件，请先删除旧覆盖版本，再上传并激活此源码。new-api 会将插件键和版本视为不可变，不能直接用不同源码覆盖同一版本。
- 使用 Agent Plan 时，将渠道 API 基础 URL 保持为 `https://ark.cn-beijing.volces.com`，填写专用 Agent Plan API Key，并选择对应的模型名。插件会为这些别名选择 `/api/plan/v3/images/generations`，不要在基础 URL 后追加 `/v3`。如果渠道只用于 Agent Plan 生图，也可以将基础 URL 设置为以 `/api/plan` 结尾。
- 安装前，将 new-api 升级至支持在模型用量 profile 收窄后保留已保存表达式的版本。详见[模型用量 profile](../../../../docs/plugin-api/v1.md#usage-profiles-by-model)。
