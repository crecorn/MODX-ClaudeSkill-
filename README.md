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

### For Claude.ai Skill
1. Download SKILL.MD and references folder
2. Create a zip file with SKILL.MD and references folder
3. Go to your settings at Claude.ai | Capabilities
4. Under Skills +add
5. Name modx-cms
6. Create a project or start a new chat and reference modx-cms

I use Opus 4.5 with extended thinking.

## 📁 Zip Folder File Structure

```
modx-cms-skill/
├── SKILL.md                 # Main skill file (quick reference + triggers)
└── references/
    ├── naming-conventions.md   # Element naming standards
    ├── syntax.md               # Complete tag syntax reference
    ├── extras.md               # pdoTools, MIGX, FormIt, Image+, Collections, Tagger
    ├── clientconfig.md         # Settings structure and groups
    ├── schema.md               # JSON-LD structured data patterns
    └── modxtransfer.md         # Import/export documentation
```

### For Claude.ai Projects

1. Create a new Project in Claude.ai
2. Upload SKILL.MD and all `.md` files from the references folder to the Project's Files
3. Start chatting with MODX expertise!

### For Claude Code

Claude wrote these instructions. I have not used it in Claude Code. 

** User-level installation (available in all projects):**
```bash
git clone https://github.com/YOUR_USERNAME/modx-cms-skill.git ~/.claude/skills/modx-cms
```

** Project-level installation (for a specific repo):**
```bash
cd your-project
mkdir -p .claude/skills
git clone https://github.com/YOUR_USERNAME/modx-cms-skill.git .claude/skills/modx-cms
```

## 🚀 Usage Examples

Once installed, Claude will automatically use this skill when you ask about MODX. Try prompts like:

```
"Use Modx-cms skill to create a head, header with nav with a dropdown, and a footer chunk."

"Create templates for the homepage, interior, contact, blog container, blog article, and utility pages."

"Research X business." "Create the ClientConfig settings XML file using researched information."

"Generate chunks of all the Local Business schema markup using ClientConfig values."

"Create the JSON file of all chunks and template for ModxTransfer import." 

```

ModxTransfer is now a standalone extra. See my repositories. I also submitted it to MODX Extras. 

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
