# MODX CMS Skill for Claude AI

A comprehensive Claude AI skill for MODX Revolution CMS development. This skill provides Claude with expert-level knowledge of MODX templating, extras configuration, and best practices for building professional websites.

## 🎯 What This Skill Does

When installed, Claude gains deep expertise in:

- **MODX Tag Syntax** - Resource fields, placeholders, snippets, chunks, TVs, and output modifiers
- **Core Extras** - pdoTools, MIGX, ClientConfig, SEO Suite, Image+, FormIt, Collections, Tagger
- **Naming Conventions** - Consistent element naming patterns for templates, chunks, TVs, and settings
- **Schema Markup** - JSON-LD structured data for LocalBusiness, Service, FAQ, and more
- **Import/Export** - ModxTransfer patterns for bulk content management
- **Performance** - Caching strategies and optimization techniques

## 📦 Installation

### For Claude.ai Projects

1. Create a new Project in Claude.ai
2. Upload all `.md` files from this repository to the Project's knowledge base
3. Start chatting with MODX expertise!

### For Claude Code

**User-level installation (available in all projects):**
```bash
git clone https://github.com/YOUR_USERNAME/modx-cms-skill.git ~/.claude/skills/modx-cms
```

**Project-level installation (for a specific repo):**
```bash
cd your-project
mkdir -p .claude/skills
git clone https://github.com/YOUR_USERNAME/modx-cms-skill.git .claude/skills/modx-cms
```

### Manual Installation

1. Download or clone this repository
2. Copy the contents to your Claude skills directory:
   - User skills: `~/.claude/skills/modx-cms/`
   - Project skills: `your-repo/.claude/skills/modx-cms/`

## 📁 File Structure

```
modx-cms-skill/
├── SKILL.md                 # Main skill file (quick reference + triggers)
├── README.md                # This file
├── LICENSE                  # MIT License
├── CHANGELOG.md             # Version history
└── references/
    ├── naming-conventions.md   # Element naming standards
    ├── syntax.md               # Complete tag syntax reference
    ├── extras.md               # pdoTools, MIGX, FormIt, Image+, Collections, Tagger
    ├── clientconfig.md         # Settings structure and groups
    ├── schema.md               # JSON-LD structured data patterns
    └── modxtransfer.md         # Import/export documentation
```

## 🚀 Usage Examples

Once installed, Claude will automatically use this skill when you ask about MODX. Try prompts like:

```
"Create a pdoResources call to list blog posts with hero images"

"Set up a MIGX TV for FAQ items with question and answer fields"

"Write the ClientConfig settings for a plumbing company"

"Create a Collections view for managing blog posts"

"Set up Tagger groups for blog categories and tags"

"Generate LocalBusiness schema markup using ClientConfig values"
```

## 📋 Quick Reference

### Tag Syntax
```
[[SnippetName]]           Cached snippet
[[!SnippetName]]          Uncached snippet
[[$ChunkName]]            Chunk
[[*fieldname]]            Resource field
[[++settingKey]]          System/ClientConfig setting
[[~resourceId]]           Link to resource
[[+placeholder]]          Placeholder from snippet
[[- comment ]]            Comment (not processed)
```

### Naming Conventions
| Element | Pattern | Example |
|---------|---------|---------|
| Template | `tpl.{type}.{name}` | `tpl.srv.Detail` |
| Component Chunk | `cmp.{Name}` | `cmp.Hero` |
| Snippet Template | `tpl.{Name}` | `tpl.ServiceCard` |
| Nav Template | `nav.tpl.{Name}` | `nav.tpl.DesktopLink` |
| Schema Chunk | `schema.{Name}` | `schema.LocalBusiness` |
| MIGX Template | `migx.tpl.{Name}` | `migx.tpl.FAQ` |
| TV | `{category}{Name}` | `heroImage` |
| ClientConfig | `{prefix}-{name}` | `nap-phone-main` |

### Core Extras
| Extra | Purpose |
|-------|---------|
| pdoTools | High-performance listings (pdoResources, pdoMenu, pdoCrumbs) |
| MIGX | Repeatable content fields (FAQs, testimonials, galleries) |
| ClientConfig | Site-wide settings (contact, social, branding) |
| SEO Suite | Meta titles, descriptions, redirects |
| Image+ | Visual image cropping with aspect ratio control |
| FormIt | Form processing with validation and hooks |
| Collections | Grid-based child resource management |
| Tagger | Tags and taxonomy system |

## ⚠️ Important Notes

### HTML Comments Don't Stop Processing!
```html
<!-- [[!Snippet]] -->   ← STILL EXECUTES! Don't do this.
[[- [[!Snippet]] ]]     ← Correct: Use MODX comment syntax
```

### TV Names Must Be Single Words
```
❌ hero-image, hero_image
✅ heroImage
```

### Placeholder Types by Context
| Context | Resource Fields | Snippet Data |
|---------|-----------------|--------------|
| Template/Component Chunk | `[[*pagetitle]]` | N/A |
| Snippet tpl Chunk | `[[+pagetitle]]` | `[[+idx]]` |

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

### Areas for Contribution
- Additional extras documentation (Babel, FRED, ContentBlocks, etc.)
- More schema markup patterns
- Workflow examples
- Bug fixes and improvements

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🔗 Resources

- [MODX Documentation](https://docs.modx.com/)
- [MODX Community Forums](https://community.modx.com/)
- [MODX Extras](https://extras.modx.com/)
- [Claude AI Skills Documentation](https://docs.anthropic.com/)

## 📝 Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

---

**Made with ❤️ for the MODX Community**
