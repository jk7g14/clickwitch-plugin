# ClickWitch Plugin Submission Tests

Run these before distributing a new Codex/Claude Code release or a public directory submission. These scenarios require host integration checks beyond manifest and archive validation.

## Positive Tests

1. Existing ready run, Korean: user asks to generate all images for a `ko` run. The plugin calls `clickwitch_get_run`, prepares each generated section in order, and invokes host-native image generation once per section.
2. Existing ready run, Japanese: user asks for a `jp` run. The prepared prompt keeps Japanese shopper-facing copy rules and does not convert locale code to `ja`.
3. Existing ready run, US English: user asks for an `en` run. The plugin keeps product and brand facts from ClickWitch and renders one section at a time.
4. New pack with enough credits: user asks to create a new detail-page pack from existing product/brand ids. The plugin gets the credit estimate, waits for explicit approval, creates the pack, polls until ready, then generates images section by section.
5. ChatGPT computer-use fallback: native image generation is unavailable but browser/computer tools and file transfer are available. The plugin follows the installed host browser skill, uploads all references in order to a fresh ChatGPT conversation, sends the exact section prompt, downloads the completed output, and saves it to the existing ClickWitch section. If these capabilities are also missing, it returns the exact pending step and manual kit.
6. OAuth return: connect while signed out using each configured social provider; after sign-in return to the original consent request, preserving state/challenge/redirect parameters. A failed sign-in retry retains that target.
7. Exact section inventory: start with a concise ready response that has no section IDs. Fetch detailed output, follow omitted-ID entries with `section_ids`, and prepare every generated section exactly once without using template IDs as section IDs. Skip custom images.
8. Complete roundtrip: retain actual host output files and section mapping; open the returned workspace/run link; upload each image to its existing generated section; choose versions/order; merge and open the final file. Record evidence at each stage. A ready pack or host-generated image is not a passing final-output test.
9. Partial failure: fail one host generation or upload, preserve successful outputs, retry that section only, and do not recreate/charge for the pack. Returning to an already-open editor refreshes server-saved uploads without losing local selected versions.
10. Local-only product photo or logo: attach a file in chat without a supported image URL. The plugin guides upload through the correct workspace's existing product/brand editor, then reuses that saved asset. It does not demand public hosting or invent a URL.
11. Continue from a result URL: provide an existing workspace/run result link. The plugin preserves that origin and exact run/workspace IDs, checks access and resumes the run without creating another pack.
12. Truncated inventory: the desired workspace/product/brand is outside the 12 displayed entries. The plugin treats the list as incomplete, guides selection in the existing web workspace, and reuses the selected item instead of creating a replacement or inventing pagination.

13. Saved-section resume: two generated sections already have persisted thumbnails and one custom section exists. Inspect the existing editor, skip the saved generated sections, preserve the custom section and generate only missing sections. An unsaved successful local output is uploaded without regeneration.
14. Stale merge: a final image exists but selected versions/order changed afterward. Rebuild and inspect the current merged image instead of marking the old final complete.
15. Prompt extraction: the first text block includes metadata and a footer around the fenced execution prompt. Render structuredContent.prompt or only that fenced prompt verbatim, without surrounding transport prose.
16. Filtered new pack: user asks to rebuild previously chosen products in Korean, Japanese and English. The plugin uses `clickwitch_list_assets` to reuse the existing workspace, products, brand, concept/category and a ready advertising model, applies `workspace_id` and `query` broadly, applies `category_top` to category/concept selection, applies `modelMode` to concept/model eligibility, then creates one pack per requested language without dropping any product_roles.
17. HERO template fidelity: a prepared HOOK/HERO section is missing the expected layout/template reference image while product and model references are present. The plugin records the missing visual-layout reference and does not claim the prompt will follow the original HERO template unless the reference is restored or the user explicitly accepts a text-only prompt.
18. Rendering-surface comparison: user requests an A/B comparison across ChatGPT image settings. The plugin keeps ClickWitch pack inputs fixed, labels the host rendering surface separately, stores outputs in separate manifests, and does not expand the matrix beyond the requested variants.

## Negative Tests

1. Free plan: MCP returns paid-plan coaching. The plugin does not leak workspace or run data and does not attempt image generation.
2. Foreign or missing run: user provides an inaccessible `run_id`. The plugin treats it as missing and does not reveal whether the run exists.
3. Bad reference set: `clickwitch_prepare_section_image` refuses an unreadable, unsupported, or oversize image. The plugin does not generate from partial references.
4. Missing upload capability: the plugin states that web upload/assembly is pending. It never claims an MCP upload succeeded, fabricates a download path, or calls add-custom-section to replace a generated slot.
5. Continue without a run reference: the user asks to continue an existing page but no run ID or result URL is available. The plugin requests that reference before creating a pack.

These are test definitions, not executed submission results. Record client/version, source commit, locale, input/run, actual artifacts, outcome and unresolved steps for each execution.

## Policy Checks

- ClickWitch server never renders images.
- Image usage depends on the selected host/service account and its limits; provider allowances are not interchangeable.
- Host rendering settings such as Latest, 6 Pro, Medium effort or image-model quality do not replace ClickWitch category, concept, product-role or advertising-model filters.
- The plugin does not start checkout or sell a digital ClickWitch subscription inside ChatGPT.
- `clickwitch_create_pack` requires explicit production-credit confirmation before a chargeable run.

## Public GitHub Installation Checks

Use disposable host configuration directories for installation tests; preserve the reviewer's actual plugins, accounts and settings. Record the published source commit and actual CLI versions. A local source replay alone is not proof of remote GitHub installation.

1. Fresh Codex: add `jk7g14/clickwitch-plugin`, install `clickwitch@clickwitch`, list the installed version, and verify pickup in a new task.
2. Fresh Claude Code: add the same repository, install `clickwitch@clickwitch` in user scope, reload plugins or restart, and verify the namespaced skill and MCP registration.
3. Repeat the copied installation request in an already installed host. The agent reuses the matching source and installed plugin without creating duplicates, changing scope or overwriting unrelated settings.
4. Marketplace collision: a different source already uses `clickwitch`. The guide identifies the collision and preserves that source. Existing `clickwitch-local`, personal plugins and standalone MCP configurations are not silently removed or duplicated.
5. Missing or incompatible CLI: report the unavailable installation step accurately; do not claim download-only or MCP-only setup installed the skill.
6. Session/OAuth boundary: distinguish installed, activated and connected states. Test an authenticated read-only quota response separately from missing login and Free-plan coaching. No pack creation, image rendering, asset upload or merge occurs during installation.
7. Public export: both root marketplace catalogs resolve `./plugins/clickwitch`; all manifest/skill assets remain inside the plugin. Root README and INSTALL work without private monorepo files. Confirm that the export contains only allowlisted public plugin and distribution files, with no credentials, customer references or local evidence.

These are release test requirements, not executed outcomes. Remote installation and account connection must be reported with their own evidence and gaps.
