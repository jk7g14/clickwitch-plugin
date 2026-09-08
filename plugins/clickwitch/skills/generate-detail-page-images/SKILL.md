---
name: generate-detail-page-images
description: Generate missing ClickWitch detail-page section images with host image tools or the user's ChatGPT UI, then save them to existing web sections and verify the final merged page. Use to create or resume a ClickWitch run.
---

# Generate, Save and Assemble Detail Page Images

ClickWitch prepares prompt packs and reference bundles, never server-rendered images. Use the connected user's host-native image generator, or control their signed-in ChatGPT UI with available browser/computer tools. The goal is a saved, assembled detail page in the original workspace.

## Select available host capabilities

Discover the installed MCP, image-generation, browser and computer-use tools. Read the applicable installed host skills before using their tools; their browser selection, attachment, download, upload and permission restrictions remain in force. This plugin does not provide a browser runtime or make a tool available merely by naming it. Do not bypass an installed browser skill with raw HTTP, shell browser automation, hidden app state or undocumented APIs.

Prefer the user's chosen rendering surface; otherwise use native image generation when available, then ChatGPT UI. Claude Code need not have a native image generator: it can use the user's ChatGPT UI if browser/computer control and file transfer are available. Do not switch to a paid image API or install software to manufacture a missing capability. Ask only for the specific missing capability or user action; continue independent authorized work. Existing generation/upload authorization carries across sections, subject to host permission requirements; do not add per-section approval prompts.

## Preserve the run and inputs

- Retain the exact origin, `workspace_id` and `run_id` from the user's result URL. Check access using `clickwitch_get_run`. For an existing-work request without a run URL/ID, obtain it before creating a pack; MCP has no run-search catalog.
- `clickwitch_get_started` accepts the known workspace ID and shows at most 12 inventory entries per list. An omitted item is not absent. Use the existing workspace's web selection for products/assets outside that slice; do not invent pagination or silently recreate them.
- Chat attachments are not yet ClickWitch assets. Save local product/brand files through the correct workspace's existing web editor when needed. Do not require public hosting of private files or pass local paths/base64 to URL-only MCP inputs.
- Preserve product evidence, package/logo appearance, factual claims, language, selected advertising-model identity and its role/crop restrictions. Reference layouts are visual guidance, not evidence for claims or permission to replace the selected model. Do not add invented endorsements, metrics or product features.
- `clickwitch_create_pack` consumes production credits. Honor its estimate and explicit confirmation contract; never set `use_production_credit: true` without the approval required for that operation. A resume does not need another pack. `clickwitch_prepare_section_image` is read-only and costs zero ClickWitch credits.

## Read readiness and saved state

1. Call `clickwitch_get_started` in the intended workspace. For a known run, read `clickwitch_get_run` with `response_format: "concise"`. Only create a new pack when the user's request needs one and inputs/credit approval are ready.
2. Wait for actual `ready` status with bounded retries (at most six status checks in this invocation). Report the current state and resumable URL if still pending. Read the ready run with `response_format: "detailed"` for exact section IDs and order. If truncated, request its omitted section IDs with `section_ids`; never infer IDs from titles/templates.
3. Open the exact MCP result-page link on the connected origin, normally `/dashboard/workspace/results/<run_id>?workspaceId=<workspace_id>`. In **섹션별 생성**, inspect saved thumbnails/versions and custom-image sections. Record which generated sections are already saved and skip them unless the user asked to regenerate. MCP readiness alone is not evidence of saved images. If web inspection is unavailable, use verified saved-state information or ask for it before regenerating an uncertain section.
4. Maintain a section-to-file record: `run_id`, `workspace_id`, `section_id`, order/title, actual output file/link, and state (`not_started`, `generated`, `saved`, `failed`). Add final merge link/status separately. Save `clickwitch-handoff.json` when supported; otherwise retain a compact mapping in the response. Never invent local paths or download links.

## Render one missing section

Skip custom-image sections; they are existing user content. Use the connected server's actual tool inventory to select a complete execution kit:

- **Prepared kit:** when `clickwitch_prepare_section_image` is available, call it with the real run/section IDs. Use **structuredContent.prompt** when available; otherwise extract only the fenced **실행 프롬프트** from the first text block, not its metadata, host instructions or footer. Pair that exact prompt with **all ordered MCP image blocks immediately after the text block**, only for this section.
- **Legacy detailed kit:** when preparation is unavailable, use `clickwitch_get_run` with `response_format: "detailed"` and the exact section ID. Use only its fenced **프롬프트** body and the explicitly ordered attachment manifest. Resolve every listed attachment to its actual file through supported host download/attachment facilities or the matching web reference control. Match the returned filename/role and section; a filename alone, thumbnail, inferred template URL, or a different section's image is insufficient. If the entire manifest is resolved and the prompt is complete, continue through the same native/ChatGPT and save/merge steps. Missing the newer tool alone is not a reason to block a complete legacy kit.

For either format, request omitted section IDs when text is truncated, remove surrounding Markdown fences only if the host requires plain text, and inspect attachment roles/order. Product and selected-model references must not be dropped or replaced, even if a legacy manifest labels them optional. Do not attach a preceding generated section unless MCP explicitly returns it. If a required file is unresolved or preparation refuses, continue independent read-only/saved-state work and record `blocked_incomplete_reference_kit` with the exact missing filename/role and resumable run/section. Do not render a partial kit, invent URLs, or claim that plugin installation upgraded the server.

- **Native image tools:** follow the installed image skill's reference mechanism. Transfer the complete returned set as supported image references or real local files without changing order. If the host cannot address every returned image, resolve that limitation before rendering; do not substitute thumbnails or guess paths.
- **ChatGPT through browser/computer use:** open a fresh conversation per section (or a genuinely empty composer with no inherited references). Obtain the returned image blocks as actual files using supported host attachment/file facilities. Upload in the returned order, waiting for each attachment to settle; verify the attachment count/sequence before submission. Insert the unchanged prompt and send once. Wait for a completed image, distinguish it from progress/errors/limits, and download the actual generated output with supported browser/computer controls. Never scrape credentials or use private ChatGPT APIs. If downloads or attachment transfer are unavailable, stop this section at its precise pending step and supply the exact kit.

Keep the output associated with its section, e.g. `01-HOOK-<section_id>.png` with a filesystem-safe name. A visible image is only `generated`. If generation is uncertain after a timeout, inspect the existing conversation/result before retrying to avoid duplicate renders. Continue to saving before starting another section. Retry a transient failed save using the already-generated file, not another image generation.

## Save through the existing ClickWitch UI

The current MCP has no generated-section upload or final-merge tool. Use available browser/computer controls to finish through the web editor. Do not call undocumented endpoints or use `clickwitch_edit_run` / `add_section` / `custom_image` to simulate uploading an existing generated section.

1. Return to the exact original workspace/run. Match the existing section using its ID when the supported DOM exposes it, plus its current title/order; resolve ambiguous matches before uploading.
2. Under **섹션별 생성**, use that section's **이미지 선택** file selector and choose the real generated file. Selection submits automatically. Wait for saving to settle; use **결과 저장** only when the current UI still needs submission, never immediately after auto-save.
3. Verify **업로드됨**, the correct thumbnail and intended version. Refresh the editor through supported UI and confirm that thumbnail/version persists before marking `saved`. A local preview, upload spinner, model response or handoff file is insufficient.
4. Repeat for missing generated sections. Preserve custom sections and successful uploads. If interrupted, resume from verified saved state; reuse generated-but-unsaved files before rendering anything again.

## Assemble and verify the final page

When all required sections have images, review selected versions and existing section order in **섹션별 생성**. Preserve the user's order unless an edit was requested. Open **최종 정리**, choose **세로 병합 이미지 만들기**, wait for completion, then use **최종 이미지 열기** or **최종 이미지 다운로드**. Verify the actual final image opens and contains the intended sections in order, including custom sections. If an existing final already matches current selections/order, reuse it; a final made before subsequent edits is stale and must be rebuilt.

Only report the detail page complete after saved sections and the current merged final are verified. Report generated, failed and pending-save/merge sections separately when blocked, with the exact result URL and available files. Continue all supported authorized UI work before handing off; a generic instruction for the user to upload is a fallback only when the necessary host capability or access is unavailable.

## Boundaries

- Korean shopper copy uses `ko`, US `en`, Japan `jp` (not `ja`). Keep the pack's language and exact prompt.
- Free-plan coaching responses are not operational pack data. Missing/foreign runs remain missing; do not reveal existence hints or bypass entitlements.
- Image generation limits belong to the chosen host/service; do not promise shared provider allowances or free rendering.
- Do not start ClickWitch subscription checkout, expose tokens or publish private references. Respect tool-provided account/credit errors and the user's scope.
