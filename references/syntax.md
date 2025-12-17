# MODX Tag Syntax Reference

Complete syntax patterns for MODX Revolution templating.

## Table of Contents
1. [Tag Types](#tag-types)
2. [Output Modifiers](#output-modifiers)
3. [Caching Rules](#caching-rules)
4. [Common Field Names](#common-field-names)
5. [Conditional Patterns](#conditional-patterns)
6. [Binding Notation](#binding-notation)

## Tag Types

### Resource Fields `[[*field]]`
Access current resource properties:
```
[[*id]]           Resource ID
[[*pagetitle]]    Page title
[[*longtitle]]    Long title (H1)
[[*description]]  Meta description
[[*alias]]        URL slug
[[*content]]      Main content
[[*parent]]       Parent resource ID
[[*template]]     Template ID
[[*menuindex]]    Sort order
[[*menutitle]]    Navigation title
[[*introtext]]    Summary/intro text
[[*createdon]]    Creation date
[[*publishedon]]  Publish date
[[*editedon]]     Last edit date
[[*createdby]]    Creator user ID
[[*editedby]]     Editor user ID
```

### Template Variables `[[*tvName]]`
Custom fields assigned to templates:
```
[[*heroImage]]      Image TV
[[*seoTitle]]       Text TV
[[*faqs]]           MIGX TV (JSON)
```

**Critical**: TV names must be single words (no hyphens/underscores in MODX 3.x best practice).

### Snippets `[[SnippetName]]`
PHP code returning output:
```
[[pdoResources]]           Cached
[[!pdoResources]]          Uncached
[[!FormIt]]                Forms always uncached
[[pdoResources? &limit=`5`]]   With parameters
```

### Chunks `[[$ChunkName]]`
Reusable HTML/markup:
```
[[$header]]                      Simple chunk
[[$card? &title=`Hello`]]        With placeholder
[[$wrapper? &content=`[[*content]]`]]  Nested
```

### Placeholders `[[+name]]`
Values set by snippets/chunks:
```
[[+pagetitle]]     From pdoResources tpl
[[+idx]]           Loop index
[[+total]]         Total count
[[+wrapper]]       Child content (pdoMenu)
[[+link]]          Generated URL
[[+output]]        Snippet output placeholder
```

### System Settings `[[++key]]`
```
[[++site_name]]         Site name
[[++site_url]]          Base URL
[[++emailsender]]       Default from email
[[++cultureKey]]        Language code
```

### ClientConfig Settings `[[++key]]`
```
[[++brand]]             Company name
[[++phone-main]]        Primary phone
[[++email-main]]        Contact email
[[++address-street]]    Street address
[[++primary-color]]     Brand color
```

### Link Tags `[[~id]]`
Generate URLs to resources:
```
[[~5]]                  Link to resource 5
[[~[[*parent]]]]        Link to parent
[[~[[++site_start]]]]   Link to homepage
[[~5? &scheme=`full`]]  Full URL with domain
```

## Output Modifiers

Chain modifiers left-to-right:
```
[[*field:modifier1:modifier2=`param`:modifier3]]
```

### String Modifiers
```
:striptags           Remove HTML
:stripTags           Same as above
:htmlent             HTML entities
:htmlspecialchars    Encode special chars
:nl2br               Newlines to <br>
:limit=`60`          Truncate to length
:ellipsis=`60`       Truncate with ...
:wordwrap=`80`       Wrap at width
:wordwrapcut=`80`    Hard wrap
:uppercase           UPPERCASE
:lowercase           lowercase
:ucfirst             Capitalize first
:ucwords             Capitalize Words
:reverse             esreveR
:length              String length
:replace=`a==b`      Replace a with b
:notags              Remove MODX tags
:esc                 Escape for JS
:strip               Remove whitespace
:stripString=`str`   Remove specific string
:trim                Trim whitespace
```

### Conditional Modifiers
```
:default=`value`     Fallback if empty
:notempty=`output`   Output if not empty
:empty=`output`      Output if empty (alias: isempty)
:if=`value`:then=`a`:else=`b`   Conditional
:isnot=`value`:then=`a`:else=`b`
:is=`value`:then=`a`:else=`b`
:eq=`value`          Equals
:ne=`value`          Not equals
:gt=`5`              Greater than
:gte=`5`             Greater or equal
:lt=`5`              Less than
:lte=`5`             Less or equal
```

### Date Modifiers
```
:strtotime           Parse to timestamp
:date=`%Y-%m-%d`     Format date
:date=`%B %d, %Y`    "January 15, 2025"
:date=`%l`           Day name
:fuzzydate           Relative time
:ago                 "2 days ago"
```

### Math Modifiers
```
:add=`5`             Add
:subtract=`3`        Subtract
:multiply=`2`        Multiply
:divide=`4`          Divide
:modulus=`2`         Remainder
:incr                Increment by 1
:decr                Decrement by 1
```

### Special Modifiers
```
:urlencode           URL encode
:urldecode           URL decode
:toJSON              Encode as JSON
:fromJSON            Decode JSON
:md5                 MD5 hash
:cdata               Wrap in CDATA
:cat=`string`        Concatenate
:lcase               lowercase
:ucase               UPPERCASE
:cssToHead           Add CSS to head
:jsToHead            Add JS to head
:jsToBottom          Add JS to bottom
```

## Caching Rules

### When to Use Uncached `[[!tag]]`

**Always uncached:**
- `[[!FormIt]]` - Form processing
- `[[!Login]]` - Authentication
- Session-based content
- User-specific output

**Usually cached:**
- `[[pdoResources]]` - Listings
- `[[pdoMenu]]` - Navigation
- `[[$chunks]]` - All chunks
- `[[++settings]]` - All settings

### Caching Decision Matrix

| Content Type | Cached | Reason |
|-------------|--------|--------|
| Static HTML | âœ“ | Never changes |
| Nav menus | âœ“ | Same for all users |
| Blog listings | âœ“ | Clear cache on publish |
| Forms | âœ— | Session tokens |
| User greetings | âœ— | Per-user |
| Live data | âœ— | Real-time |
| Search results | âœ— | Per-query |

## Common Field Names

### Standard Resource Fields
```
id, pagetitle, longtitle, description, alias
content, introtext, menutitle
template, parent, menuindex
published, hidemenu, deleted, isfolder
createdby, createdon, editedby, editedon
publishedon, publishedby
uri, uri_override
class_key, context_key
content_type, content_dispo
cacheable, searchable, richtext
```

### Common TV Naming (Single Words)
```
heroImage, heroVideo, featuredImage
introText, seoTitle, metaDescription  
serviceIcon, categoryColor
faqs, testimonials, features, gallery
ctaText, ctaLink, ctaStyle
```

## Conditional Patterns

### Show if Not Empty
```
[[*heroImage:notempty=`
  <div class="hero">
    <img src="[[*heroImage]]" alt="[[*pagetitle]]">
  </div>
`]]
```

### Show if Empty (Fallback)
```
[[*seoTitle:default=`[[*pagetitle]] | [[++brand]]`]]
```

### Conditional with Else
```
[[*template:is=`5`:then=`
  [[$layout.service]]
`:else=`
  [[$layout.default]]
`]]
```

### Multiple Conditions (Nested)
```
[[*published:is=`1`:then=`
  [[*hidemenu:is=`0`:then=`
    <li><a href="[[~[[*id]]]]">[[*menutitle]]</a></li>
  `]]
`]]
```

## Binding Notation

For TV default values and input options:

```
@INHERIT           Inherit from parent
@FILE path         Load from file
@CHUNK ChunkName   Load chunk content
@INLINE content    Inline content
@SELECT query      Database query
@EVAL php          Execute PHP
@DIRECTORY path    List directory
@RESOURCE id       Resource field value
```

### Examples
```
TV Default: @INHERIT
TV Input:   @SELECT `pagetitle`, `id` FROM `modx_site_content` WHERE `parent` = 5
File-based: @FILE assets/chunks/footer.html
```

## Snippet Parameters

### Parameter Syntax
```
[[Snippet?
  &stringParam=`value`
  &numericParam=`10`
  &booleanParam=`1`
  &nestedParam=`[[*id]]`
  &jsonParam=`{"key":"value"}`
]]
```

### Property Sets
Named parameter groups attached to elements:
```
[[Snippet@PropertySetName]]
[[Snippet@PropertySetName? &override=`value`]]
```

## Error Prevention

### Common Mistakes
```
âŒ [[*hero-image]]        Hyphens in TV names
âœ“  [[*heroImage]]         Single word TV name

âŒ [[$chunk]][[*field]]   Missing space/newline
âœ“  [[$chunk]]
   [[*field]]

âŒ [[snippet? &p=`[[*f]]`  Missing closing brackets
âœ“  [[snippet? &p=`[[*f]]`]]

âŒ [[++setting]            Missing second bracket
âœ“  [[++setting]]

âŒ [[*content:limit=60]]   Missing backticks
âœ“  [[*content:limit=`60`]]
```

### Debugging Tips
1. Check Error Log: Manager â†’ Reports â†’ Error Log
2. Enable `log_level` = 4 for verbose logging
3. Test tags individually before nesting
4. Clear cache between changes
5. Use browser dev tools for output inspection
