---
name: gilmos-generation
description: "Generate or display gilmOS sandbox images and videos with exact project asset tags and version references, preserve model choices and request IDs, and follow asynchronous results. Use for media production or previews, not just asset catalog editing."
---

# Generate and display gilmOS media

Use the connected user's project, permissions and credits. Resolve the relevant scene/shot and production references from the project context; use paginated `gilmos_read_resources` for missing records. Keep the requested number of outputs, model, aspect ratio, duration, audio option, seed and creative intent. A request for one image does not authorize more images, video variants or unrelated model experiments.

## Decide the operation and destination

| Request | Operation and saved destination |
| --- | --- |
| Display existing image/video/audio | `gilmos_get_media`; no generation |
| Free image/video, next-shot illustration, variant | `gilmos_generate_media`; global project sandbox |
| Image references for selected asset records | `gilmos_start_asset_image_workflow`; new asset versions |
| Native panorama / character reference sheet | `gilmos_run_sandbox_recipe`; its selected output |
| Technical breakdown / scene storyboard boards | `gilmos_start_scene_storyboard_workflow`; scene versions |

The MCP does not expose general audio/3D generation or direct save-to-shot-version for a free generation. Do not imply an unsupported destination. Recipe and asset/storyboard workflows have their own lifecycle; this skill's `get_job` loop applies to free generation.

## Resolve references before launching

1. Find the exact asset, including parent/variation. Read its selected image version with `gilmos_read_asset_versions` or `gilmos_read_resources`; do not replace an explicitly chosen version with a later one.
2. Include the canonical tag in `prompt`: for example, `@ROZ listens for a signal at the edge of @Forest.` The names are illustrative: use the actual project's tags. This source prompt must retain `@ROZ` even when the provider receives indexed image tags or native elements.
3. Optional `assetVersions` pins verified `{assetId,versionId}` pairs; `itemIds` attaches accessible sandbox media. Do not attach an unrelated selected item as a substitute for a named character. `generate_media` accepts project IDs/references, not external media URLs.
4. If the asset is absent, ambiguous or has no usable image, resolve that missing requirement before a paid call. A metadata path is not a visually inspected reference.

## Model and cost

Before choosing or submitting a model, call `gilmos_list_models`. It shares the enabled image/video registry with the UI and returns exact `id` values, display names, providers, limits and `supportedTools`. Filter with `mediaType`, `provider`, exact `model`, or `tool` (`generate_media`, `start_asset_image_workflow`, `start_scene_storyboard_workflow`). `includeDeveloperModels=true` also lists enabled reseller entries. Copy the exact `id` into `model`; labels and remembered aliases are not identifiers. Report an unavailable requested model instead of silently switching. Provider access, quotas and credits are checked when executing; discovery does not probe providers. Follow the offered schemas and server limits for model options.

Prices come from gilmOS. `gilmos_estimate_generation` works for reference-free requests; it rejects asset tags, `itemIds` and `assetVersions` because it cannot resolve their costs. Do not remove the user's references to manufacture a quote or present a base estimate as the full cost. When an exact reference-based estimate is required, explain the current limitation and use the existing gilmOS UI estimate if the user can access it.

## Submission, completion and retries

Read [examples.json](references/examples.json) for exact argument templates. Use a new UUID `requestId` for each distinct requested output. Preserve that UUID and all original arguments on a retry of the same generation; a changed UUID can charge and create a second output.

After `gilmos_generate_media`, inspect `assetReferences` (correct asset/version) and `providerPrompt` (provider representation). Preserve the returned `jobId` and item ID. If those references are wrong, inspect the pending job and request cancellation if appropriate; do not immediately launch another job.

Poll `gilmos_get_job` with paced checks appropriate to the task. `queued`/`running` means work in progress, not a finished image. On `failed` or `cancelled`, inspect the cause; do not silently launch more billable work. If the session cannot wait, report the actual job state and identifier for later retrieval. `gilmos_cancel_job` requires project-admin permission and cannot promise a refund for submitted provider work.

## Show the result

Completed free-generation jobs return native image content plus a temporary original-file link. Render the returned image content using the host's media display support. For existing media, or a missing/expired preview, call `gilmos_get_media` with the accessible item/version ID. It retrieves media without generating again.

`table` can be `sandbox_items`, `asset_versions`, `shot_versions` or `audios`; an asset ID is not an asset-version ID. For a shot still use `variant=keyframe`; the default media variant prefers its video. For video/audio use a supported preview/player; if unavailable, give the usable original link and state that display limitation. Signed links expire: refresh through `get_media`. Do not claim the user saw an image when only text or metadata was returned, and do not download remote media to bypass a host's display restriction.
