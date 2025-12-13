# MODX Extras Reference

Patterns and examples for core extras: pdoTools, MIGX, FormIt, SEO Suite, Image+, Collections, Tagger.

## Table of Contents
1. [pdoTools](#pdotools)
2. [MIGX](#migx)
3. [FormIt](#formit)
4. [SEO Suite](#seo-suite)
5. [Image+](#image)
6. [ClientConfig](#clientconfig)
7. [Collections](#collections)
8. [Tagger](#tagger)

---

## pdoTools

High-performance alternative to getResources, Wayfinder, and Breadcrumbs.

### pdoResources

#### Basic Listing
```
[[pdoResources?
  &parents=`5`
  &limit=`10`
  &tpl=`tplBlogCard`
]]
```

#### Full Parameters
```
[[pdoResources?
  &parents=`[[*id]]`
  &depth=`2`
  &limit=`12`
  &offset=`0`
  &sortby=`publishedon`
  &sortdir=`DESC`
  &tpl=`tplCard`
  &includeTVs=`heroImage,introText`
  &processTVs=`1`
  &tvPrefix=``
  &where=`{"published":1,"deleted":0}`
  &showHidden=`0`
  &hideContainers=`0`
  &context=`web`
  &idx=`1`
  &first=`1`
  &last=`0`
  &totalVar=`total`
  &tplWrapper=`@INLINE <div class="grid">[[+output]]</div>`
  &wrapIfEmpty=`0`
]]
```

#### Key Parameters
| Parameter | Purpose |
|-----------|---------|
| `&parents` | Parent ID(s), comma-separated |
| `&resources` | Specific resource IDs to include |
| `&depth` | How deep to search (0=unlimited) |
| `&limit` | Max results (0=unlimited) |
| `&offset` | Skip first N results |
| `&sortby` | Field to sort by |
| `&sortdir` | ASC or DESC |
| `&tpl` | Chunk for each item |
| `&includeTVs` | TV names to include |
| `&processTVs` | Process TV values (1/0) |
| `&tvPrefix` | Prefix for TV placeholders |
| `&where` | JSON filter conditions |
| `&showHidden` | Include hidemenu=1 |

#### Available Placeholders in tpl
```
[[+id]]           Resource ID
[[+pagetitle]]    Page title
[[+longtitle]]    Long title
[[+description]]  Description
[[+alias]]        URL slug
[[+content]]      Content
[[+introtext]]    Intro text
[[+menutitle]]    Menu title
[[+link]]         Generated URL
[[+idx]]          Loop index (starts at 1)
[[+first]]        1 if first item
[[+last]]         1 if last item
[[+total]]        Total count
[[+tvName]]       TV value (with tvPrefix=``)
```

#### Sample tpl Chunk (tplBlogCard)
```html
<article class="card [[+first:notempty=`card--featured`]]">
  [[+heroImage:notempty=`
    <img src="[[+heroImage]]" alt="[[+pagetitle]]" loading="lazy">
  `]]
  <div class="card__body">
    <h3><a href="[[+link]]">[[+pagetitle]]</a></h3>
    <p>[[+introtext:ellipsis=`150`]]</p>
    <time datetime="[[+publishedon:date=`%Y-%m-%d`]]">
      [[+publishedon:date=`%B %d, %Y`]]
    </time>
  </div>
</article>
```

### pdoMenu

#### Basic Navigation
```
[[pdoMenu?
  &parents=`0`
  &level=`2`
]]
```

#### Custom Templates
```
[[pdoMenu?
  &parents=`0`
  &level=`2`
  &tplOuter=`@INLINE <nav><ul class="nav">[[+wrapper]]</ul></nav>`
  &tpl=`@INLINE <li><a href="[[+link]]">[[+menutitle]]</a></li>`
  &tplParentRow=`@INLINE <li class="has-dropdown"><a href="[[+link]]">[[+menutitle]]</a>[[+wrapper]]</li>`
  &tplInner=`@INLINE <ul class="dropdown">[[+wrapper]]</ul>`
]]
```

#### Key Parameters
| Parameter | Purpose |
|-----------|---------|
| `&parents` | Starting point (0=top level) |
| `&level` | Depth of menu |
| `&tplOuter` | Wrapper template |
| `&tpl` | Each menu item |
| `&tplParentRow` | Items with children |
| `&tplInner` | Submenu wrapper |
| `&tplHere` | Current page item |
| `&tplParentRowHere` | Current parent item |

#### Critical: `[[+wrapper]]` Placement
The `[[+wrapper]]` placeholder outputs child menus. It MUST appear:
- In `&tplParentRow` - after the link, for dropdown menus
- In `&tplInner` - for the child items

### pdoCrumbs

```
[[pdoCrumbs?
  &showHome=`1`
  &showCurrent=`1`
  &tpl=`@INLINE <li><a href="[[+link]]">[[+menutitle]]</a></li>`
  &tplCurrent=`@INLINE <li aria-current="page">[[+menutitle]]</li>`
  &tplWrapper=`@INLINE <nav aria-label="Breadcrumb"><ol class="breadcrumb">[[+output]]</ol></nav>`
]]
```

### pdoPage (Pagination)

```
[[!pdoPage?
  &element=`pdoResources`
  &parents=`5`
  &limit=`12`
  &tpl=`tplCard`
  &pageVarKey=`page`
  &pageLimit=`5`
  &tplPageWrapper=`@INLINE <nav class="pagination">[[+first]][[+prev]][[+pages]][[+next]][[+last]]</nav>`
]]
```

---

## MIGX

Multi-Items Grid for eXtended functionality - repeatable custom fields.

### Creating MIGX TVs

1. Manager â†’ Extras â†’ MIGX
2. Create Configuration with:
   - Name: e.g., `faqs`
   - Form Tabs (JSON): Define fields
   - Grid Columns (JSON): Define display

#### Form Tabs JSON Example (FAQs)
```json
[{
  "caption": "FAQ Item",
  "fields": [{
    "field": "question",
    "caption": "Question",
    "inputTVtype": "text"
  },{
    "field": "answer", 
    "caption": "Answer",
    "inputTVtype": "richtext"
  }]
}]
```

#### Grid Columns JSON
```json
[{
  "header": "Question",
  "dataIndex": "question",
  "width": 200
},{
  "header": "Answer",
  "dataIndex": "answer",
  "width": 300,
  "renderer": "this.renderChunk"
}]
```

### Rendering MIGX Data

#### Using getImageList Snippet
```
[[getImageList?
  &tvname=`faqs`
  &tpl=`tplFaqItem`
]]
```

#### tpl Chunk (tplFaqItem)
```html
<div class="faq-item" itemscope itemprop="mainEntity" itemtype="https://schema.org/Question">
  <h3 itemprop="name">[[+question]]</h3>
  <div itemscope itemprop="acceptedAnswer" itemtype="https://schema.org/Answer">
    <div itemprop="text">[[+answer]]</div>
  </div>
</div>
```

### Common MIGX Configurations

#### Testimonials
Form Tabs:
```json
[{
  "fields": [
    {"field": "name", "caption": "Name", "inputTVtype": "text"},
    {"field": "title", "caption": "Title/Company", "inputTVtype": "text"},
    {"field": "quote", "caption": "Quote", "inputTVtype": "textarea"},
    {"field": "rating", "caption": "Rating (1-5)", "inputTVtype": "number"},
    {"field": "photo", "caption": "Photo", "inputTVtype": "image"}
  ]
}]
```

#### Features/Benefits
```json
[{
  "fields": [
    {"field": "icon", "caption": "Icon Class", "inputTVtype": "text"},
    {"field": "title", "caption": "Title", "inputTVtype": "text"},
    {"field": "description", "caption": "Description", "inputTVtype": "textarea"}
  ]
}]
```

#### Gallery
```json
[{
  "fields": [
    {"field": "image", "caption": "Image", "inputTVtype": "image"},
    {"field": "caption", "caption": "Caption", "inputTVtype": "text"},
    {"field": "alt", "caption": "Alt Text", "inputTVtype": "text"}
  ]
}]
```

---

## FormIt

Form processing with validation, hooks, and anti-spam.

### Basic Contact Form

```
[[!FormIt?
  &hooks=`spam,email,redirect`
  &emailTpl=`tplContactEmail`
  &emailTo=`[[++email-main]]`
  &emailSubject=`Website Contact: [[+subject]]`
  &redirectTo=`[[++contact_thanks_page]]`
  &validate=`name:required,
    email:required:email,
    phone:required,
    message:required:minLength=^10^`
  &validationErrorMessage=`Please correct the errors below.`
  &validationErrorBulkTpl=`@INLINE <li>[[+error]]</li>`
]]

<form method="post" action="[[~[[*id]]]]">
  <input type="hidden" name="nospam" value="">
  
  <label for="name">Name *</label>
  <input type="text" name="name" id="name" value="[[+fi.name]]">
  <span class="error">[[+fi.error.name]]</span>
  
  <label for="email">Email *</label>
  <input type="email" name="email" id="email" value="[[+fi.email]]">
  <span class="error">[[+fi.error.email]]</span>
  
  <label for="phone">Phone *</label>
  <input type="tel" name="phone" id="phone" value="[[+fi.phone]]">
  <span class="error">[[+fi.error.phone]]</span>
  
  <label for="message">Message *</label>
  <textarea name="message" id="message">[[+fi.message]]</textarea>
  <span class="error">[[+fi.error.message]]</span>
  
  <button type="submit">Send Message</button>
</form>
```

### Email Template (tplContactEmail)
```html
<p>New contact form submission:</p>
<table>
  <tr><td><strong>Name:</strong></td><td>[[+name]]</td></tr>
  <tr><td><strong>Email:</strong></td><td>[[+email]]</td></tr>
  <tr><td><strong>Phone:</strong></td><td>[[+phone]]</td></tr>
  <tr><td><strong>Message:</strong></td><td>[[+message]]</td></tr>
</table>
<p>Submitted: [[+date:date=`%Y-%m-%d %H:%M`]]</p>
```

### Validation Options
```
required              Field required
email                 Valid email
minLength=^N^         Min characters
maxLength=^N^         Max characters
minValue=^N^          Min numeric value
maxValue=^N^          Max numeric value
contains=^str^        Must contain string
regexp=^pattern^      Regex match
```

### Common Hooks
| Hook | Purpose |
|------|---------|
| `spam` | Honeypot spam check |
| `email` | Send email |
| `redirect` | Redirect after success |
| `recaptchav3` | Google reCAPTCHA v3 |
| `FormItSaveForm` | Save to database |
| `FormItAutoResponder` | Send confirmation |

### File Uploads
```
[[!FormIt?
  &hooks=`FormItSaveForm,email,redirect`
  &allowFiles=`1`
  &attachFilesToEmail=`1`
]]

<input type="file" name="attachment" accept=".pdf,.doc,.docx">
```

---

## SEO Suite

Meta fields, redirects, and sitemap management.

### Available Fields
- SEO Title (seoTitle)
- Meta Description (metaDescription)
- Canonical URL
- Robots (index/noindex, follow/nofollow)
- Open Graph fields
- Twitter Card fields

### In Templates
```html
<title>[[*seoTitle:default=`[[*pagetitle]] | [[++brand]]`]]</title>
<meta name="description" content="[[*metaDescription:default=`[[*description]]`]]">
```

### Redirect Management
When changing aliases, SEO Suite prompts to create 301 redirects automatically.

---

## Image+

Advanced image cropping TV with visual editor. Requires pThumb as cropping engine.

### Creating Image+ TVs

1. Create TV in Manager
2. Input Type: `imageplus`
3. Output Type: `default` (recommended for snippet processing)
4. Configure Input Options:
   - Target Width/Height: Minimum dimensions
   - Target Ratio: Force aspect ratio (e.g., 16:9)
   - Allow Alt Tag: Enable alt text field
   - Allow Caption: Enable caption field

**Important**: Always set Output Type to `default` when using the ImagePlus snippet to render images. Setting it to `Image+` can cause 500 errors due to double-processing.

### TV Input Options (JSON in MIGX)
```json
{
  "targetWidth": "1200",
  "targetHeight": "675",
  "targetRatio": "16:9",
  "thumbnailWidth": "200",
  "allowAltTag": "1",
  "allowCaption": "1",
  "allowCredits": "1"
}
```

### ImagePlus Snippet

#### Recommended Usage (using &value)
The most reliable approach is to pass the raw TV value directly using `&value`:
```
[[ImagePlus? &value=`[[*heroImage]]` &options=`w=800&h=450&zc=1`]]
```

This method explicitly passes the TV's JSON data to the snippet and avoids issues with TV output type configurations.

#### Alternative Usage (using &tvname)
```
[[ImagePlus? &tvname=`heroImage` &options=`w=800&h=450&zc=1`]]
```

**Note**: The `&tvname` approach can cause 500 errors in some configurations. If you encounter issues, switch to the `&value` method.

#### Snippet Properties
| Property | Description |
|----------|-------------|
| `&value` | Raw TV value - **recommended method** |
| `&tvname` | Name of Image+ TV (alternative, may cause issues) |
| `&docid` | Resource ID (required in pdoResources tpl when using &tvname) |
| `&options` | pThumb options: w, h, zc, q, etc. |
| `&type` | Output type: `url` (default), `tpl` |
| `&tpl` | Chunk name when type=tpl |

#### pThumb Options
```
w=800        Width in pixels
h=450        Height in pixels
zc=1         Zoom crop (1=crop to fit)
q=85         Quality (1-100)
aoe=1        Allow output enlargement
f=webp       Output format
```

### Usage Patterns

#### Direct in Template (Recommended)
```html
[[*heroImage:notempty=`
<img src="[[ImagePlus? &value=`[[*heroImage]]` &options=`w=1200&h=675&zc=1&q=85`]]" 
     alt="[[*pagetitle]]">
`]]
```

#### With Output Chunk
```
[[ImagePlus?
  &value=`[[*heroImage]]`
  &type=`tpl`
  &tpl=`tplResponsiveImage`
  &options=`w=800`
]]
```

tplResponsiveImage chunk:
```html
<picture>
  <source srcset="[[+source.src:pthumb=`w=1200`]] 1200w,
                  [[+source.src:pthumb=`w=800`]] 800w,
                  [[+source.src:pthumb=`w=400`]] 400w"
          sizes="(min-width: 800px) 800px, 100vw">
  <img src="[[+url]]" alt="[[+alt]]" loading="lazy">
</picture>
```

#### In pdoResources Template Chunk
When inside a pdoResources tpl chunk, use placeholder syntax (`[[+]]`) instead of resource field syntax (`[[*]]`):
```html
[[+heroImage:notempty=`
<img src="[[ImagePlus? &value=`[[+heroImage]]` &options=`w=400&h=225&zc=1`]]" 
     alt="[[+pagetitle]]"
     loading="lazy">
`]]
```

**Note**: The TV must be included via `&includeTVs` and `&processTVs=`1`` in the pdoResources call, with `&tvPrefix=``` to avoid prefix issues.

#### In MIGX
When Image+ is used inside MIGX, use `&value` with the MIGX placeholder:
```
[[ImagePlus?
  &value=`[[+image]]`
  &options=`w=600&q=85`
  &type=`tpl`
  &tpl=`tplGalleryImage`
]]
```
**Note**: MIGX TV output type must be `default`, not `imageplus`.

### Available Placeholders (in tpl chunks)
```
[[+url]]           Cropped image URL
[[+alt]]           Alt text
[[+caption]]       Caption text
[[+credits]]       Credits text
[[+source.src]]    Original source path (for pthumb modifier)
[[+source.width]]  Original width
[[+source.height]] Original height
[[+crop.width]]    Crop area width
[[+crop.height]]   Crop area height
[[+crop.x]]        Crop X position
[[+crop.y]]        Crop Y position
[[+crop.options]]  pThumb crop options string
```

### MIGX Integration

Form Tabs:
```json
[{
  "fields": [
    {"field": "image", "caption": "Image", "inputTVtype": "imageplus",
     "configs": {"targetRatio": "16:9", "allowAltTag": "1"}}
  ]
}]
```

Grid Columns:
```json
[{
  "header": "Image",
  "dataIndex": "image",
  "renderer": "ImagePlus.MIGX_Renderer"
}]
```

### Collections Integration
Set column renderer to `ImagePlus.MIGX_Renderer` to show thumbnails in grid.

### Common Aspect Ratios
| Use Case | Ratio | Example Size |
|----------|-------|--------------|
| Hero/Banner | 16:9 | 1920×1080 |
| Blog Featured | 16:9 | 1200×675 |
| Square/Avatar | 1:1 | 400×400 |
| Portrait | 3:4 | 600×800 |
| Landscape | 4:3 | 800×600 |
| Cinematic | 21:9 | 2560×1080 |

### Troubleshooting

| Issue | Solution |
|-------|----------|
| 500 error with ImagePlus | Switch from `&tvname` to `&value` method |
| 500 error persists | Verify TV Output Type is set to `default`, not `Image+` |
| Image not rendering | Check pThumb is installed |
| Crop not applied | Ensure Input Type is `imageplus` (not just `image`) |
| Wrong image in listing | In pdoResources tpl, use `[[+tvName]]` not `[[*tvName]]` |
| Blank output | Check TV is assigned to template, verify TV name spelling |

### Quick Reference

**In templates (direct resource access):**
```
[[ImagePlus? &value=`[[*featuredImage]]` &options=`w=800&h=450&zc=1`]]
```

**In pdoResources tpl chunks (placeholder context):**
```
[[ImagePlus? &value=`[[+featuredImage]]` &options=`w=400&h=225&zc=1`]]
```

**In MIGX tpl chunks:**
```
[[ImagePlus? &value=`[[+image]]` &options=`w=600&q=85`]]
```

## ClientConfig

Centralized site settings editable by non-developers.

### Accessing Settings
```
[[++settingKey]]
```

### Setting Groups (Standard LSB Setup)
1. **Company Information** - brand, legal-name, tagline, site-url
2. **Contact & NAP** - phone-main, email-main, address-*, geo-*, hours-json
3. **Social Links** - social-facebook, social-instagram, etc.
4. **Tracking & Analytics** - ga4-measurement-id, gtm-id, facebook-pixel-id
5. **Schema & SEO** - business-type, price-range, review ratings
6. **Service Areas** - base-city, service-cities, service-counties, radius-miles
7. **Branding** - logo-url, primary-color, secondary-color
8. **Site Structure & IDs** - services_root, blog_root, contact_page

### Conditional Usage
```
[[++gtm-id:notempty=`
  <!-- Google Tag Manager -->
  <script>...</script>
`]]

[[++social-facebook:notempty=`
  <a href="[[++social-facebook]]" aria-label="Facebook">
    <svg>...</svg>
  </a>
`]]
```

### JSON Settings (hours-json)
```
[[++hours-json]]
```
Outputs:
```json
[{"dayOfWeek":["Monday","Tuesday","Wednesday","Thursday","Friday"],"opens":"08:00","closes":"17:00"}]
```
Used in schema markup snippets to generate OpeningHoursSpecification.

---

## Collections

Grid-based management for child resources. Ideal for blogs, products, news, and any content type with many children.

### Overview

Collections adds a `CollectionContainer` resource class that:
- Hides direct children from the Resource Tree
- Displays children in a sortable, filterable grid view
- Supports drag-and-drop sorting
- Allows bulk actions on multiple resources
- Provides customizable column views

### Setting Up a Collection

1. Create or edit a Resource
2. Go to Settings tab → Resource Type
3. Change to `CollectionContainer`
4. Save - children now appear in grid under "Children" tab

### Collection Views (CMP)

Configure views at: **Extras → Collection Views**

| Setting | Purpose |
|---------|---------|
| Page size | Default items per page |
| Sort field | Default sort column |
| Sort dir | ASC or DESC |
| Allow bulk actions | Enable multi-select checkboxes |
| Allow drag & drop | Enable manual sorting |
| Default children's template | Auto-assign template to new children |

### Grid Columns

Add columns showing:
- Standard resource fields (`pagetitle`, `alias`, `publishedon`)
- Template Variables (prefix with `tv_`: `tv_heroImage`)
- Tagger groups (prefix with `tagger_`: `tagger_categories`)

### Collections + Image+ Integration

To show Image+ thumbnails in Collections grid:
1. Add column with TV name (e.g., `heroImage`)
2. Set Renderer to `ImagePlus.MIGX_Renderer`

### Selections (Curated Lists)

`SelectionContainer` creates curated resource lists without duplicating content:
- Add existing resources from anywhere in the tree
- Maintains separate menuindex for custom ordering
- Use `getSelections` snippet to output

```
[[getSelections?
  &container=`15`
  &tpl=`tplSelectionCard`
  &sortby=`menuindex`
]]
```

### Best Practices

- Use Collections for any container with 10+ children
- Configure default template for consistent child creation
- Add TV columns for quick content overview
- Enable bulk actions for efficient management

---

## Tagger

Robust tagging and taxonomy system for categorizing resources.

### Overview

Tagger provides:
- Tag Groups (categories, topics, types, etc.)
- Tags within each group
- Resource-to-tag assignments
- Snippets for displaying and filtering by tags

### Creating Tag Groups

**Extras → Tagger → Groups → Create Group**

| Setting | Purpose |
|---------|---------|
| Name | Display name |
| Alias | URL-friendly identifier |
| Field type | `tagger-combo-tag` (free-form) or `tagger-field-tags` (autocomplete) |
| Allow new tags | Let editors create tags on-the-fly |
| Remove unused | Auto-delete tags with no resources |
| Show for templates | Limit to specific templates |

### Assigning Tags to Resources

Tags appear in the Resource editor based on "Show for templates" setting. Editors can:
- Select from existing tags (dropdown)
- Type to search/filter
- Create new tags (if allowed)

### TaggerGetTags Snippet

Display tags for resources:

```
[[!TaggerGetTags?
  &resources=`[[*id]]`
  &groups=`categories`
  &rowTpl=`tpl.TagLink`
  &separator=`, `
]]
```

#### tpl.TagLink Chunk
```html
<a href="[[+uri]]" class="tag">[[+tag]]</a>
```

#### Available Placeholders
```
[[+id]]           Tag ID
[[+tag]]          Tag name
[[+alias]]        Tag alias (URL-friendly)
[[+group]]        Group ID
[[+group_name]]   Group name
[[+group_alias]]  Group alias
[[+uri]]          Generated URL to tag listing
[[+idx]]          Loop index
[[+active]]       1 if currently active tag
```

### TaggerGetResourcesWhere Snippet

Filter pdoResources/getResources by tags:

```
[[!pdoResources?
  &parents=`5`
  &where=`[[!TaggerGetResourcesWhere? &groups=`categories`]]`
  &tpl=`tpl.BlogCard`
]]
```

The snippet reads `$_GET` parameters to filter by selected tags.

### Tag Listing Page Pattern

**Step 1**: Create tag links pointing to listing page

```
[[!TaggerGetTags?
  &groups=`categories`
  &target=`10`
  &rowTpl=`tpl.TagLink`
]]
```

**Step 2**: On listing page (ID 10), filter by active tag

```
[[!pdoResources?
  &parents=`5`
  &where=`[[!TaggerGetResourcesWhere]]`
  &tpl=`tpl.BlogCard`
  &limit=`12`
]]
```

### TaggerGetCurrentTag Snippet

Display currently active tag(s):

```
[[!TaggerGetCurrentTag?
  &tagTpl=`@INLINE [[+label]]`
  &groupTpl=`@INLINE <li>[[+name]]: [[+tags]]</li>`
  &outTpl=`@INLINE <ul>[[+groups]]</ul>`
]]
```

### Common Configurations

#### Blog Categories
```
Group: Categories
Alias: categories
Field type: tagger-combo-tag
Allow new: No (admin-controlled)
Templates: Blog Post
```

#### Content Tags (Free-form)
```
Group: Tags
Alias: tags
Field type: tagger-field-tags
Allow new: Yes
Remove unused: Yes
Templates: Blog Post, Article
```

#### Product Types
```
Group: Product Type
Alias: product-type
Field type: tagger-combo-tag
Allow new: No
Templates: Product
```

### Collections + Tagger Integration

Show tags in Collections grid:
1. Add column with Name: `tagger_groupalias` (e.g., `tagger_categories`)
2. Tags display automatically

### Best Practices

- Use descriptive group aliases for clean URLs
- Limit "Allow new tags" to prevent tag sprawl
- Enable "Remove unused" for user-generated tags
- Create separate groups for different taxonomy types
- Use `&target` parameter to control tag link destinations
