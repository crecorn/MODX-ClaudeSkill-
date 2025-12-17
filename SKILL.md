---
name: modx-cms
description: Expert-level MODX Revolution CMS development, templating, and content management. Use this skill when building MODX websites, creating templates/chunks/snippets, configuring extras (pdoTools, MIGX, ClientConfig, SEO Suite, Image+, FormIt), writing MODX tag syntax, implementing schema markup, or troubleshooting MODX issues. Triggers include "MODX", "pdoResources", "pdoMenu", "MIGX", "ClientConfig", "FormIt", "Image+", "ImagePlus", "chunks", "snippets", "TVs", "template variables", or any MODX CMS development task.
---

# MODX CMS Development

Expert guidance for MODX Revolution websites.

## Tag Syntax Quick Reference

```
[[SnippetName]]           Cached snippet
[[!SnippetName]]          Uncached snippet (per-request)
[[$ChunkName]]            Chunk (HTML)
[[*fieldname]]            Resource field (pagetitle, content, alias, etc.)
[[++settingKey]]          System/Context setting
[[~resourceId]]           Link to resource
[[+placeholder]]          Placeholder from snippet/chunk
```

### Output Modifiers
```
[[*pagetitle:striptags:limit=`60`]]
[[*field:default=`fallback value`]]
[[+date:strtotime:date=`%B %d, %Y`]]
[[*content:nl2br]]
```

### Parameters
```
[[SnippetName? &param=`value` &other=`[[*field]]`]]
[[$ChunkName? &title=`[[*pagetitle]]` &class=`card`]]
```

## Critical Syntax Rules

1. **Resource fields use asterisk**: `[[*pagetitle]]`, `[[*content]]`, `[[*alias]]`
2. **Placeholders use plus**: `[[+title]]`, `[[+idx]]`, `[[+wrapper]]`
3. **TV names must be single words**: `heroImage` not `hero_image` or `hero-image`
4. **Uncached (`!`) only when dynamic**: session data, user input, time-based
5. **Nested tags resolve inner-first**: `[[Snippet? &id=`[[*parent]]`]]`

## Core Extras Stack

| Extra | Purpose |
|-------|---------|
| pdoTools | High-performance listings (pdoResources, pdoMenu, pdoCrumbs) |
| MIGX | Repeatable content fields (FAQs, testimonials, galleries) |
| ClientConfig | Site-wide settings (contact, social, branding) |
| SEO Suite | Meta titles, descriptions, redirects |
| Image+ | Visual image cropping with aspect ratio control |
| FormIt | Form processing with validation and hooks |
| modAI | AI-powered content generation |

## Common Patterns

### pdoResources Listing
```
[[pdoResources?
  &parents=`[[*id]]`
  &limit=`12`
  &tpl=`tplCard`
  &includeTVs=`heroImage,introText`
  &processTVs=`1`
  &tvPrefix=``
  &sortby=`menuindex`
  &sortdir=`ASC`
]]
```

### pdoMenu Navigation
```
[[pdoMenu?
  &parents=`0`
  &level=`2`
  &tplOuter=`@INLINE <ul class="nav">[[+wrapper]]</ul>`
  &tpl=`@INLINE <li><a href="[[+link]]">[[+menutitle]]</a>[[+wrapper]]</li>`
  &tplParentRow=`@INLINE <li class="has-children"><a href="[[+link]]">[[+menutitle]]</a>[[+wrapper]]</li>`
  &tplInner=`@INLINE <ul class="dropdown">[[+wrapper]]</ul>`
]]
```

### Image+ Snippet
```
[[ImagePlus? &tvname=`heroImage` &options=`w=800&h=450&zc=1`]]
```

With template chunk:
```
[[ImagePlus? &tvname=`heroImage` &type=`tpl` &tpl=`tplImage` &options=`w=800`]]
```

In pdoResources tpl (requires `&docid`):
```
[[ImagePlus? &tvname=`heroImage` &docid=`[[+id]]` &options=`w=400&h=225&zc=1`]]
```

### ClientConfig Usage
```
[[++brand]]                    Company name
[[++phone-main]]               Primary phone
[[++email-main]]               Contact email
[[++social-facebook]]          Facebook URL
[[++primary-color]]            Brand color hex
```

### Conditional Output
```
[[*heroImage:notempty=`<img src="[[*heroImage]]" alt="[[*pagetitle]]">`]]
[[++gtm-id:notempty=`<!-- GTM Code -->`]]
```

## File Organization

### Templates Chunks Pattern
Keep templates minimal; decompose into cached chunks:

```
<!-- Template -->
[[$head]]
[[$header]]
<main>
  [[$hero]]
  [[$section.content]]
  [[$cta]]
</main>
[[$footer]]
[[$scripts]]
```

## Reference Files

For detailed information, read the appropriate reference:

- **[references/naming-conventions.md](references/naming-conventions.md)**: Element naming standards (templates, chunks, TVs, settings)
- **[references/syntax.md](references/syntax.md)**: Complete tag syntax, modifiers, caching rules
- **[references/extras.md](references/extras.md)**: pdoTools, MIGX, FormIt, Image+ patterns
- **[references/clientconfig.md](references/clientconfig.md)**: Settings structure and groups
- **[references/schema.md](references/schema.md)**: JSON-LD structured data implementation
- **[references/ModxTransfer.md](references/ModxTransfer.md)**: ModxTransfer snippet for importing/exporting elements and resources

## Caching Strategy

| Element | Default | When Uncached |
|---------|---------|---------------|
| Chunks | Cached | Never (use snippet wrapper) |
| Snippets | Cached | User-specific, form data, real-time |
| TVs | Cached | Rarely |
| Settings | Cached | Never |

## Performance Tips

1. Use `pdoTools` over `getResources` for listings
2. Cache snippets by default; benchmark before uncaching
3. Limit `&includeTVs` to only needed TVs
4. Use `&tvPrefix=` `` to avoid placeholder prefix issues
5. Index database columns used in `&where` clauses

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Changes not showing | Clear cache: Site Clear Cache |
| Blank output | Check tag syntax, especially backticks and brackets |
| TV not rendering | Verify TV assigned to template, check name (single word) |
| pdoMenu empty | Check `&parents`, resource `hidemenu` setting |
| Image+ not working | Ensure pThumb installed, TV output type set to Image+ |
| Snippet error | Check Error Log: Manager â†’ Reports â†’ Error Log |
