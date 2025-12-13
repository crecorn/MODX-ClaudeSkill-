# Changelog

All notable changes to the MODX CMS Skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-12-13

### Added
- **Collections extra documentation** - Complete guide for grid-based resource management
  - CollectionContainer setup and configuration
  - Collection Views CMP settings
  - Grid columns with TV and Tagger integration
  - Selections (curated lists) with getSelections snippet
  - Image+ thumbnail integration
- **Tagger extra documentation** - Full taxonomy/tagging system reference
  - Tag Groups configuration
  - TaggerGetTags snippet with all placeholders
  - TaggerGetResourcesWhere for filtering
  - Tag listing page patterns
  - Common configurations (categories, tags, product types)
- **Commenting Code section** - Critical documentation on MODX comment syntax
  - `[[-` comment tag usage
  - Warning that HTML comments do NOT stop MODX processing
  - Three methods to properly disable MODX tags
- Updated skill triggers to include "Collections" and "Tagger" keywords
- Added troubleshooting entry for "commented code still runs" issue

### Changed
- Core Extras Stack now includes Collections and Tagger
- extras.md Table of Contents updated with new sections
- SKILL.md description updated with new trigger words

## [1.0.0] - 2025-11

### Added
- Initial release of MODX CMS Skill
- **SKILL.md** - Main skill file with quick reference and triggers
- **naming-conventions.md** - Element naming standards for LSB sites
  - Template naming (tpl.*, tpl.srv.*, tpl.blog.*, etc.)
  - Chunk naming (cmp.*, tpl.*, nav.tpl.*, schema.*, migx.tpl.*)
  - TV naming (camelCase single words)
  - ClientConfig prefixes (co-, nap-, brand-, social-, etc.)
  - Snippet and Plugin naming
- **syntax.md** - Complete MODX tag syntax reference
  - All tag types with examples
  - Output modifiers (string, conditional, date, math)
  - Caching rules and decision matrix
  - Binding notation
  - Error prevention tips
- **extras.md** - Core extras documentation
  - pdoTools (pdoResources, pdoMenu, pdoCrumbs, pdoPage)
  - MIGX configurations and getImageList usage
  - FormIt with validation and hooks
  - SEO Suite fields
  - Image+ with ImagePlus snippet patterns
  - ClientConfig structure
- **clientconfig.md** - Settings structure reference
  - All setting groups with keys and types
  - Usage examples in templates
  - CSS custom properties pattern
- **schema.md** - JSON-LD structured data patterns
  - LocalBusiness/Organization schema
  - Service schema
  - FAQ schema
  - Breadcrumb schema
  - Article/BlogPosting schema
  - Product schema
- **modxtransfer.md** - Import/export documentation
  - ModxTransfer snippet reference
  - Element export/import
  - Resource export/import with delete options
  - JSON format specifications
  - Migration workflows

---

## Future Plans

- [ ] Additional extras: Babel, FRED, ContentBlocks, Login
- [ ] E-commerce patterns (Commerce, SimpleCart)
- [ ] Multi-context configurations
- [ ] Advanced caching strategies
- [ ] Security best practices
- [ ] Deployment workflows
