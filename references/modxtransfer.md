# MODX Import/Export Reference

Unified ModxTransfer snippet for importing and exporting MODX elements and resources.

## Table of Contents
1. [ModxTransfer Snippet](#modxtransfer-snippet)
2. [Export Elements](#export-elements)
3. [Export Resources](#export-resources)
4. [Import Elements](#import-elements)
5. [Import Resources](#import-resources)
6. [JSON Formats](#json-formats)
7. [Usage Examples](#usage-examples)

---

## ModxTransfer Snippet

A unified snippet combining all import/export functionality for MODX elements and resources.

### Core Parameters

| Parameter | Values | Default | Description |
|-----------|--------|---------|-------------|
| `&action` | `export`, `import` | *required* | Operation to perform |
| `&type` | `elements`, `resources` | *required* | What to import/export |
| `&mode` | `preview`, `execute` | `preview` | Preview changes or execute (import only) |
| `&output` | `display`, `file`, `download` | `display` | Output method (export only) |
| `&file` | string | | Filename for export or path to import file |
| `&path` | string | `assets/exports/` | Directory for file output |
| `&pretty` | `0`, `1` | `1` | Pretty-print JSON output |

---

## Export Elements

Export MODX elements (categories, templates, chunks, TVs, snippets, plugins) to JSON.

### Element Export Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `&elementTypes` | `categories,templates,chunks,tvs,snippets,plugins` | Comma-separated element types |
| `&category` | | Filter by category name |

### Element Export Examples

#### Export All Elements (Display)
```
[[!ModxTransfer? &action=`export` &type=`elements`]]
```

#### Export Specific Element Types
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`elements` 
  &elementTypes=`chunks,tvs`
]]
```

#### Export Category to File
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`elements` 
  &category=`LSB - Core`
  &output=`file` 
  &file=`lsb-core-elements.json`
]]
```

#### Export and Download
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`elements` 
  &output=`download` 
  &file=`my-elements.json`
]]
```

---

## Export Resources

Export MODX resources to JSON with optional delete-after-export functionality.

### Resource Export Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `&parents` | | Comma-separated parent IDs |
| `&depth` | `10` | Depth to traverse |
| `&templates` | | Comma-separated template names to filter |
| `&ids` | | Specific resource IDs to export |
| `&excludeIds` | | Resource IDs to exclude |
| `&includeTVs` | `1` | Include TV values |
| `&includeContent` | `1` | Include content field |
| `&includeUnpublished` | `0` | Include unpublished resources |
| `&sortby` | `menuindex` | Sort field |
| `&sortdir` | `ASC` | Sort direction |
| `&limit` | `0` | Max resources (0=unlimited) |
| `&delete` | `0` | Delete after export: 0=no, 1=soft, 2=hard |
| `&confirmDelete` | | Must be `YES-DELETE` when using delete |

### Resource Export Examples

#### Export All Resources (Display)
```
[[!ModxTransfer? &action=`export` &type=`resources`]]
```

#### Export from Specific Parents
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources` 
  &parents=`5,10`
  &depth=`2`
]]
```

#### Export by Template
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources` 
  &templates=`Blog Post,Service Page`
  &output=`file`
  &file=`content-export.json`
]]
```

#### Export Specific IDs
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources` 
  &ids=`15,23,47,89`
]]
```

#### Export and Soft Delete (Move to Trash)
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources` 
  &parents=`25`
  &output=`file`
  &file=`archived-content.json`
  &delete=`1`
  &confirmDelete=`YES-DELETE`
]]
```

#### Export and Hard Delete (Permanent)
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources` 
  &parents=`30`
  &output=`file`
  &file=`removed-content.json`
  &delete=`2`
  &confirmDelete=`YES-DELETE`
]]
```

âš ï¸ **Warning**: Delete operations are permanent. Always verify export file before using `&delete`.

---

## Import Elements

Import MODX elements from JSON file.

### Element Import Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `&file` | *required* | Path to JSON file |
| `&mode` | `preview` | `preview` or `execute` |
| `&update` | `0` | Update existing elements (1=yes) |

### Element Import Examples

#### Preview Import
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`elements` 
  &file=`assets/exports/my-elements.json`
]]
```

#### Execute Import (Create Only)
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`elements` 
  &file=`assets/exports/my-elements.json`
  &mode=`execute`
]]
```

#### Execute Import with Updates
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`elements` 
  &file=`assets/exports/my-elements.json`
  &mode=`execute`
  &update=`1`
]]
```

### Import Process
1. **Categories** are created first (with parent relationships)
2. **Templates** are created/updated
3. **Chunks** are created/updated
4. **TVs** are created/updated with template assignments
5. **Snippets** are created/updated with properties
6. **Plugins** are created/updated with event registrations

---

## Import Resources

Import MODX resources from JSON file.

### Resource Import Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `&file` | *required* | Path to JSON file |
| `&parentId` | *required* | Resource ID to nest imports under |
| `&mode` | `preview` | `preview` or `execute` |
| `&defaultTemplate` | | Template ID for imported resources |

### Resource Import Examples

#### Preview Resource Import
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`resources` 
  &file=`assets/exports/blog-posts.json`
  &parentId=`5`
]]
```

#### Execute Resource Import
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`resources` 
  &file=`assets/exports/blog-posts.json`
  &parentId=`5`
  &mode=`execute`
]]
```

#### Import with Default Template
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`resources` 
  &file=`assets/exports/pages.json`
  &parentId=`0`
  &defaultTemplate=`4`
  &mode=`execute`
]]
```

### Import Behavior
- All resources are imported as **unpublished** (for review)
- Hierarchy is preserved using `original_id` and `parent` mapping
- TVs are imported including MIGX JSON data
- Template names are resolved to IDs automatically

---

## JSON Formats

### Elements JSON Structure

```json
{
  "meta": {
    "project": "Project Name",
    "description": "MODX Elements Export",
    "created": "2025-01-15T10:30:00Z",
    "exported_types": ["categories", "templates", "chunks", "tvs"],
    "category_filter": "LSB - Core"
  },
  "categories": [
    {
      "name": "LSB - Core",
      "parent": "LSB"
    }
  ],
  "templates": [
    {
      "name": "tpl.Homepage",
      "description": "Homepage template",
      "category": "LSB - Core",
      "content": "[[$head]]\n[[$header]]\n..."
    }
  ],
  "chunks": [
    {
      "name": "cmp.Header",
      "description": "Site header component",
      "category": "LSB - Core",
      "content": "<header>...</header>"
    }
  ],
  "tvs": [
    {
      "name": "heroImage",
      "caption": "Hero Image",
      "description": "Page hero background",
      "type": "image",
      "category": "LSB - Media",
      "templates": ["tpl.Homepage", "tpl.Interior"],
      "default": "",
      "elements": "",
      "display": "default",
      "input_properties": {},
      "output_properties": {}
    }
  ],
  "snippets": [
    {
      "name": "lsb.ServiceSchema",
      "description": "Generate service JSON-LD",
      "category": "LSB - Schema",
      "content": "<?php\n...",
      "properties": []
    }
  ],
  "plugins": [
    {
      "name": "lsb.AutoCanonical",
      "description": "Auto-generate canonical URLs",
      "category": "LSB - SEO",
      "content": "<?php\n...",
      "disabled": false,
      "events": ["OnLoadWebDocument"]
    }
  ]
}
```

### Resources JSON Structure

```json
{
  "meta": {
    "project": "Site Name",
    "description": "MODX Resources Export",
    "created": "2025-01-15T10:30:00Z",
    "count": 25,
    "filters": {
      "parents": "5,10",
      "templates": "Blog Post",
      "includeUnpublished": false,
      "includeTVs": true
    },
    "deleted_after_export": false
  },
  "resources": [
    {
      "original_id": 15,
      "pagetitle": "Page Title",
      "longtitle": "Extended Page Title for H1",
      "alias": "page-slug",
      "description": "Meta description under 160 characters.",
      "introtext": "Summary text for listings.",
      "menutitle": "Nav Title",
      "content": "<p>Main content HTML...</p>",
      "template": "Blog Post",
      "parent": 5,
      "parent_path": "blog",
      "published": 1,
      "hidemenu": 0,
      "menuindex": 0,
      "searchable": 1,
      "cacheable": 1,
      "richtext": 1,
      "isfolder": 0,
      "class_key": "modDocument",
      "pub_date": "2025-01-20T08:00:00-05:00",
      "tvs": {
        "heroImage": "/assets/images/hero.jpg",
        "seoTitle": "Custom SEO Title",
        "faqs": [
          {"question": "Question 1?", "answer": "Answer 1"},
          {"question": "Question 2?", "answer": "Answer 2"}
        ]
      }
    }
  ]
}
```

### Resource Fields Reference

#### Required
| Field | Type | Description |
|-------|------|-------------|
| `pagetitle` | string | Page title, under 60 chars |

#### Standard Fields
| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `longtitle` | string | | Extended title for H1 |
| `alias` | string | auto | URL slug (lowercase, hyphens) |
| `description` | string | | Meta description, under 160 chars |
| `introtext` | string | | Summary for listings |
| `menutitle` | string | pagetitle | Navigation title |
| `content` | string | | Main body HTML |
| `template` | string | | Template name (exact match) |
| `parent` | int | | Parent resource ID |
| `parent_path` | string | | Parent path (informational) |
| `published` | int | 0 | 0=draft, 1=published |
| `hidemenu` | int | 0 | 0=show, 1=hide from nav |
| `menuindex` | int | 0 | Sort order (lower=first) |
| `searchable` | int | 1 | Include in search |
| `cacheable` | int | 1 | Enable caching |
| `richtext` | int | 1 | Use rich text editor |
| `isfolder` | int | 0 | 1=container with children |
| `class_key` | string | modDocument | Resource class |
| `pub_date` | string | | Scheduled publish (ISO 8601) |
| `unpub_date` | string | | Scheduled unpublish (ISO 8601) |

#### Template Variables
| Field | Type | Description |
|-------|------|-------------|
| `tvs` | object | Key-value pairs matching TV names |

---

## Usage Examples

### Site Migration Workflow

#### 1. Export Everything from Source Site
```
[[!ModxTransfer? 
  &action=`export` 
  &type=`elements`
  &output=`file`
  &file=`site-elements.json`
]]

[[!ModxTransfer? 
  &action=`export` 
  &type=`resources`
  &output=`file`
  &file=`site-resources.json`
]]
```

#### 2. Import to Target Site
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`elements`
  &file=`assets/exports/site-elements.json`
  &mode=`execute`
]]

[[!ModxTransfer? 
  &action=`import` 
  &type=`resources`
  &file=`assets/exports/site-resources.json`
  &parentId=`0`
  &mode=`execute`
]]
```

### Archive and Remove Old Content

```
[[!ModxTransfer? 
  &action=`export` 
  &type=`resources`
  &parents=`50`
  &templates=`Blog Post`
  &output=`file`
  &file=`archived-blog-2024.json`
  &delete=`1`
  &confirmDelete=`YES-DELETE`
]]
```

### Export Specific Category for Reuse

```
[[!ModxTransfer? 
  &action=`export` 
  &type=`elements`
  &category=`LSB - Forms`
  &elementTypes=`chunks,snippets`
  &output=`file`
  &file=`form-components.json`
]]
```

### Bulk Import Service Pages with FAQs

```json
{
  "meta": {
    "project": "Service Pages Import",
    "created": "2025-01-15T10:00:00Z"
  },
  "resources": [
    {
      "pagetitle": "Plumbing Services",
      "longtitle": "Professional Plumbing Services",
      "alias": "plumbing",
      "template": "tpl.srv.Detail",
      "description": "Expert plumbing services for residential and commercial properties.",
      "content": "<p>We offer comprehensive plumbing solutions...</p>",
      "tvs": {
        "heroImage": "/assets/images/services/plumbing.jpg",
        "serviceIcon": "wrench",
        "faqs": [
          {"question": "Do you offer emergency services?", "answer": "Yes, we provide 24/7 emergency plumbing."},
          {"question": "What areas do you serve?", "answer": "We serve the greater metro area."}
        ]
      }
    },
    {
      "pagetitle": "Drain Cleaning",
      "longtitle": "Professional Drain Cleaning Services",
      "alias": "drain-cleaning",
      "template": "tpl.srv.Detail",
      "description": "Fast and effective drain cleaning solutions.",
      "content": "<p>Clogged drains are no match for our team...</p>",
      "tvs": {
        "heroImage": "/assets/images/services/drain-cleaning.jpg",
        "serviceIcon": "filter"
      }
    }
  ]
}
```

Import the services:
```
[[!ModxTransfer? 
  &action=`import` 
  &type=`resources`
  &file=`assets/exports/services.json`
  &parentId=`5`
  &mode=`execute`
]]
```

---

## Content HTML Guidelines

```html
<p>Opening paragraph - no heading needed.</p>

<h2>Main Section Title</h2>
<p>Section content here. Keep paragraphs to 3-4 sentences.</p>

<h3>Subsection Title</h3>
<p>More detailed content.</p>

<ul>
  <li>Use lists for scannable content</li>
  <li>Keep list items concise</li>
</ul>

<h2>Call to Action</h2>
<p>Contact us today for a free consultation.</p>
```

**Rules:**
- Never use `<h1>` - template handles this via pagetitle/longtitle
- Use `<h2>` for main sections
- Use `<h3>` for subsections
- Keep paragraphs short (3-4 sentences)
- End service/product pages with CTA

---

## Validation Checklist

### Before Importing Elements
- [ ] Valid JSON syntax
- [ ] Category names match for assignment
- [ ] Template names in TV assignments exist
- [ ] Plugin event names are valid MODX events

### Before Importing Resources
- [ ] Valid JSON syntax
- [ ] All resources have `pagetitle`
- [ ] Template names match exactly
- [ ] Parent ID exists in target site
- [ ] TV names match (single words, case-sensitive)
- [ ] Dates in ISO 8601 format
- [ ] Aliases lowercase with hyphens only
- [ ] MIGX TVs are arrays of objects

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "File not found" | Use relative path from MODX root or absolute path |
| "Invalid JSON" | Validate JSON at jsonlint.com |
| "Parent not found" | Verify `&parentId` exists |
| Elements skipped | Existing elements skip by default; use `&update=`1`` |
| TVs not importing | Ensure TV names match exactly (case-sensitive) |
| Resources orphaned | Process in hierarchy order; parents must exist first |
| Delete not working | Must include `&confirmDelete=`YES-DELETE`` |

---

## File Naming Convention

```
{client}-{type}-{date}.json

Examples:
acme-elements-2025-01-15.json
clientname-resources-full.json
mysite-blog-archive-2024.json
lsb-starter-kit-chunks.json
```

---

## Quick Reference Card

### Export Elements
```
[[!ModxTransfer? &action=`export` &type=`elements`]]
[[!ModxTransfer? &action=`export` &type=`elements` &output=`file` &file=`export.json`]]
[[!ModxTransfer? &action=`export` &type=`elements` &category=`MyCategory`]]
```

### Export Resources
```
[[!ModxTransfer? &action=`export` &type=`resources`]]
[[!ModxTransfer? &action=`export` &type=`resources` &parents=`5` &output=`file` &file=`export.json`]]
[[!ModxTransfer? &action=`export` &type=`resources` &templates=`Blog Post`]]
```

### Import Elements
```
[[!ModxTransfer? &action=`import` &type=`elements` &file=`export.json`]]
[[!ModxTransfer? &action=`import` &type=`elements` &file=`export.json` &mode=`execute`]]
[[!ModxTransfer? &action=`import` &type=`elements` &file=`export.json` &mode=`execute` &update=`1`]]
```

### Import Resources
```
[[!ModxTransfer? &action=`import` &type=`resources` &file=`export.json` &parentId=`5`]]
[[!ModxTransfer? &action=`import` &type=`resources` &file=`export.json` &parentId=`5` &mode=`execute`]]
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2025-12 | Unified ModxTransfer snippet replacing separate snippets |
| 1.0 | 2025-11 | Initial ResourceCreator/ResourceExporter snippets |
