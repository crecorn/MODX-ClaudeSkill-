---
name: modx-cms
description: Expert-level MODX Revolution CMS development, templating, and content management. Use this skill when building MODX websites, creating templates/chunks/snippets, configuring extras (pdoTools, MIGX, ClientConfig, SEO Suite, Image+, FormIt, Collections, Tagger), writing MODX tag syntax, implementing schema markup, or troubleshooting MODX issues. Triggers include "MODX", "pdoResources", "pdoMenu", "MIGX", "ClientConfig", "FormIt", "Image+", "ImagePlus", "Collections", "Tagger", "chunks", "snippets", "TVs", "template variables", or any MODX CMS development task.
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
[[- comment ]]            Comment (not processed or output)
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
6. **HTML comments don't stop processing**: `<!-- [[!Snippet]] -->` still executes! Use `[[-` or break brackets to truly disable

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
| Collections | Grid-based child resource management (blogs, products, listings) |
| Tagger | Tags and taxonomy system with groups and filtering |

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

### Templates → Chunks Pattern
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

## Base Href Configuration

The `<base href>` tag ensures relative URLs resolve correctly across all page depths.

### The Problem

Without `<base href>`, relative URLs resolve from the current page's path, breaking assets on nested pages:

| Page URL | Relative Path | Resolves To |
|----------|---------------|-------------|
| `/` | `assets/css/style.css` | `/assets/css/style.css` ✓ |
| `/blog/` | `assets/css/style.css` | `/blog/assets/css/style.css` ✗ |
| `/blog/article-name/` | `assets/css/style.css` | `/blog/article-name/assets/css/style.css` ✗ |

**Symptoms of missing base href:**
- Broken images/CSS/JS on nested pages
- Navigation links prepending parent path (e.g., `/blog/contact` instead of `/contact`)
- Logo not displaying on child pages
- Incorrect canonical URLs

### The Solution

Add this line inside `<head>` in your `cmp.Head` chunk, **before** any other asset references:

```html
<base href="[[++site_url]]">
```

This tells the browser to resolve all relative URLs from the site root, regardless of current page depth.

### Example cmp.Head Implementation

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<base href="[[++site_url]]">

<!-- Now these work on ALL pages -->
<link rel="stylesheet" href="assets/css/style.css">
<link rel="icon" href="assets/images/favicon.ico">

<title>[[*pagetitle]] | [[++co-brand]]</title>
<link rel="canonical" href="[[~[[*id]]? &scheme=`full`]]">
```

### Anchor Links Caveat

With `<base href>` set, same-page anchor links need the current page URL:

```html
<!-- WRONG: Goes to homepage#section -->
<a href="#section">Jump</a>

<!-- CORRECT: Stays on current page -->
<a href="[[~[[*id]]]]#section">Jump</a>
```

### Alternative: Root-Relative Paths

If you prefer not to use `<base href>`, ensure ALL asset paths start with `/`:

```html
<!-- Root-relative (works without base href) -->
<link rel="stylesheet" href="/assets/css/style.css">
<img src="/assets/uploads/brand/logo.svg" alt="Logo">

<!-- Relative (breaks on nested pages without base href) -->
<link rel="stylesheet" href="assets/css/style.css">
<img src="assets/uploads/brand/logo.svg" alt="Logo">
```

### Recommendation

Always include `<base href="[[++site_url]]">` in `cmp.Head`. It's a single line that prevents an entire class of URL resolution bugs across your site.

## Reference Files

For detailed information, read the appropriate reference:

- **[references/naming-conventions.md](references/naming-conventions.md)**: Element naming standards (templates, chunks, TVs, settings)
- **[references/syntax.md](references/syntax.md)**: Complete tag syntax, modifiers, caching rules
- **[references/extras.md](references/extras.md)**: pdoTools, MIGX, FormIt, Image+, Collections, Tagger patterns
- **[references/clientconfig.md](references/clientconfig.md)**: Settings structure and groups
- **[references/schema.md](references/schema.md)**: JSON-LD structured data implementation
- **[references/import-export.md](references/import-export.md)**: ResourceCreator/Exporter snippets, JSON templates
- **[references/va-guide.md](references/va-guide.md)**: VA content management procedures

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
| Changes not showing | Clear cache: Site → Clear Cache |
| Blank output | Check tag syntax, especially backticks and brackets |
| TV not rendering | Verify TV assigned to template, check name (single word) |
| pdoMenu empty | Check `&parents`, resource `hidemenu` setting |
| Image+ not working | Ensure pThumb installed, TV output type set to Image+ |
| Snippet error | Check Error Log: Manager → Reports → Error Log |
| "Commented" code still runs | HTML comments do not stop MODX processing; use `[[-` or remove brackets |
| Broken assets on nested pages | Add `<base href="[[++site_url]]">` to `cmp.Head` |
