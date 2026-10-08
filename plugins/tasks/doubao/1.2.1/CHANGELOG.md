---
changelogVersion: 1
plugin: "doubao"
version: "1.2.1"
locale: "en"
translations:
  zh-CN: "CHANGELOG.zh-CN.md"
---
# Changelog

## [1.2.1]

### Added

- Generate and edit images with `doubao-seedream-5-0-flash-260915` through the native Doubao image route and the OpenAI Images API. Choose 1K, 1.5K, or 2K output and supply up to 10 reference images.
- Use Seedream 5.0 Flash for layer decomposition and transparent-background editing, with PNG or JPEG output. Layer decomposition requires exactly one input image; transparent output requires one reference image and PNG. Group generation is not supported by this model.
- Use the Agent Plan model names `doubao-seedream-5-0-pro` and `doubao-seedream-5.0-pro` through the OpenAI Images API. These aliases use `/api/plan/v3/images/generations`; dated Ark model IDs continue to use `/api/v3/images/generations`.

### Fixed

- Count actual returned images for Seedream models whose minimum output is above 1.5K, even when the response omits image sizes. Usage is reported in `images_above_1_5k` instead of retaining the requested count.
- Report `video_input: "none"` for Seedance models without reference-video support so existing usage-based prices continue to match their no-video condition.
- Claim both Agent Plan model names in the task plugin registry so image requests are handled by the plugin instead of falling through to an unrelated channel adaptor.

### Migration

- Configure the Agent Plan tariff for the model name you expose (`doubao-seedream-5-0-pro` or `doubao-seedream-5.0-pro`). For tiered task billing, use an expression compatible with `u("images_up_to_1_5k")`, `u("images_above_1_5k")`, `u("input_images")`, and `u("layer_decomposition")`.
- If an older `doubao@1.2.1` override is installed, remove that override before uploading this source again, then activate the replacement. new-api keeps a plugin key and version immutable, so a different source cannot overwrite the same version.
- For Agent Plan, keep the channel Base URL at `https://ark.cn-beijing.volces.com`, use a dedicated Agent Plan API key, and choose the matching model name. The plugin selects `/api/plan/v3/images/generations` for these aliases; do not append `/v3` to the Base URL. A Base URL ending in `/api/plan` is available when a channel is dedicated to Agent Plan image requests.
- Before installing, upgrade new-api to a build that supports preserving stored expressions when a model's usage profile narrows. See [usage profiles by model](../../../../docs/plugin-api/v1.md#usage-profiles-by-model).
