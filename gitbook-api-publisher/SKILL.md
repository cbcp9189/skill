---
name: gitbook-api-publisher
description: Generate and publish the EnjoyIot SaaS API reference or approved documentation pages to GitBook. Use when the user asks to inspect, synchronize, draft, publish, or add an operationId/page to the GitBook publication allowlist; do not use for ordinary API implementation work.
---

# GitBook API Publisher

Publish only the API operations and documentation pages explicitly listed in `doc/gitbook-api-publish.yaml`. The manifest is the publication contract: a controller, URL prefix, or `@Operation` annotation alone never authorizes publication.

Read [the manifest guide](references/manifest.md) before changing the manifest. If the API repository does not yet contain `doc/gitbook-api-publish.yaml`, initialize it from `references/publish-manifest.template.yaml` before any GitBook mutation. Read [the GitBook target profile](references/ebelong-gitbook.md) before interacting with GitBook.

## Modes

Choose the least-mutating mode that meets the request.

- **Inspect**: Generate or obtain the current OpenAPI specs and report additions, changes, removals, missing explicit operation IDs, and allowed versus excluded operations. Do not change GitBook.
- **Draft**: Update only allowed operations/pages in a GitBook change request. Return the change-request number, changed pages, skipped items, and compatibility risks. Do not merge.
- **Publish**: Merge only the change request the user explicitly identifies (for example, “发布草稿 6”). Verify the published revision and return the public link.
- **Allowlist update**: Add or modify manifest entries only when the user explicitly asks to publish/approve the specified operation or page. Keep the edit narrow and report the resulting diff before drafting.

Never infer a request to publish from “同步”“生成” or “更新”; those create or update a draft only. Never merge a draft merely because it has no validation errors.

## API selection and stability

1. Locate the operation in the generated OpenAPI specification by its exact `operationId`. Do not identify an operation by a Chinese summary, controller name, or URL alone.
2. Require a non-empty explicit `operationId` in source OpenAPI annotations for every newly allowlisted API. If it is generated implicitly, report the issue and stop before adding it to the manifest. Suggest a stable semantic ID such as `saas.space.create`.
3. Include only operations whose manifest entry has `enabled: true` and whose source specification and target group match the request.
4. Exclude every `/openapi/device/**` operation regardless of other source metadata.
5. Never delete a published page or remove an allowlisted operation as part of an ordinary sync. Report it as a removal candidate and wait for an explicit removal instruction.

## Page generation

- Use GitBook OpenAPI-operation blocks for interfaces, referencing the configured GitBook OpenAPI spec, method, and path.
- Use Chinese titles and the configured `通用接口` / `应用接口` navigation groups.
- Treat auto-generated API structure, parameters, responses, and samples as generated content.
- Preserve the visible GitBook hint whose first bold line is `人工维护区块 · <key>`. It is the protected manual area. Update neither its body nor its placement during normal sync.
- For normal documentation pages, use the manifest `pageKey` as the stable identity and preserve their matching protected manual block.

## Compatibility report

Before drafting, compare the candidate OpenAPI contract against the latest published revision. Flag: removed paths/operations, removed response fields, response type changes, required request fields newly added, and enum values removed. Still create a draft only when the request asked for a draft; never merge when any such risk is present without an explicit follow-up confirmation.

## Completion evidence

For a draft, report its number/ID, changed page titles, skipped operation IDs, compatibility findings, and the preview link if available. For a publication, report the published revision, the stable public URL, and a revision-pinned URL. Do not claim publication based only on a successful change-request update.
