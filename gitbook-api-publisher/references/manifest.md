# 发布清单

`doc/gitbook-api-publish.yaml` is checked into the repository and is the single source of truth for what may appear in the public GitBook site.

## API entry

Each API is selected by exactly one OpenAPI `operationId`; URLs and controller classes are descriptive metadata, not selectors.

```yaml
operations:
  - operationId: saas.space.create
    enabled: true
    title: 创建空间
    section: 通用接口
    group: 空间管理
    source:
      spec: saas-v1-space-management
      method: post
      path: /api/v1/spaces
    gitbook:
      pageId: null # Populate after the first draft creates the page.
      manualBlockKey: api:saas.space.create
```

`operationId`, `section`, `group`, `source.spec`, `source.method`, and `source.path` are required. `pageId` is optional only before the initial draft; persist it after GitBook returns it.

Use stable semantic IDs. Do not use a generated method name, a Chinese title, or a mutable path segment as an ID. A renamed path retains the same `operationId`; a semantically new operation receives a new one.

## Documentation-page entry

Normal pages use `pageKey` instead of `operationId`, but remain explicitly allowlisted.

```yaml
pages:
  - pageKey: api-overview
    enabled: true
    title: SaaS API 概述
    section: 通用接口
    sourceFile: doc/saas-api-gitbook.md
    gitbook:
      pageId: null
      manualBlockKey: page:api-overview
```

## Change procedure

1. Add an explicit `operationId` to the source API annotation.
2. Add one enabled entry to the manifest, with the desired Chinese navigation group.
3. Request a draft. Review the compatibility report and GitBook preview.
4. Only after approval, explicitly ask to publish the returned draft number.

To stop future changes from being published, set `enabled: false`; this does not delete the existing GitBook page. Deletion or unpublishing must be a separate explicit request.
