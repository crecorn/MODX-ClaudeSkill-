# MODX LSB Starter Kit â€” Naming Conventions

> Comprehensive naming standards for Templates, Chunks, TVs, Snippets, Plugins, and ClientConfig settings.

---

## Pattern Format

```
{prefix}.{name}
{prefix}.{category}.{name}    (for nested contexts)
```

All names use **lowercase letters**, **periods as separators**, and **no underscores or hyphens** in element names (hyphens allowed in ClientConfig keys).

---

## 1. Templates

| Prefix | Purpose | Examples |
|--------|---------|----------|
| `tpl.` | Standard page templates | `tpl.Homepage`, `tpl.Contact`, `tpl.Interior` |
| `tpl.srv.` | Service-related templates | `tpl.srv.Detail`, `tpl.srv.Collection` |
| `tpl.blog.` | Blog templates | `tpl.blog.Article`, `tpl.blog.Collection` |
| `tpl.area.` | Service area/location templates | `tpl.area.Landing`, `tpl.area.Service` |
| `tpl.util.` | Utility templates (thank you, error) | `tpl.util.ThankYou`, `tpl.util.Error404` |

### Full Template List (LSB Starter Kit)

| Template | Purpose |
|----------|---------|
| `tpl.Homepage` | Homepage layout |
| `tpl.Interior` | Generic interior page |
| `tpl.Contact` | Contact page with form |
| `tpl.srv.Detail` | Individual service page |
| `tpl.srv.Collection` | Service listing/category page |
| `tpl.blog.Article` | Blog post |
| `tpl.blog.Collection` | Blog listing page |
| `tpl.area.Landing` | City/location landing page |
| `tpl.area.Service` | Service + City combo page |
| `tpl.util.ThankYou` | Form confirmation page |
| `tpl.util.Error404` | 404 error page |
| `tpl.Team` | Team/about page |
| `tpl.Pricing` | Pricing/packages page |

---

## 2. Chunks

| Prefix | Purpose | Placeholders | Examples |
|--------|---------|--------------|----------|
| `cmp.` | Standalone component (called directly from template) | `[[*field]]`, `[[++setting]]` | `cmp.Hero`, `cmp.CTA`, `cmp.Footer` |
| `tpl.` | Snippet template (used by pdoResources, getImageList) | `[[+field]]`, `[[+tv.field]]` | `tpl.ServiceCard`, `tpl.BlogCard` |
| `nav.tpl.` | pdoMenu template | `[[+link]]`, `[[+menutitle]]` | `nav.tpl.DesktopLink`, `nav.tpl.MobileRow` |
| `form.` | Form chunk | `[[!+fi.field]]` | `form.Contact`, `form.Quote` |
| `schema.` | JSON-LD schema chunk | `[[++setting]]`, `[[*field]]` | `schema.LocalBusiness`, `schema.Service` |
| `email.` | Email template | `[[+formfield]]` | `email.ContactNotify`, `email.QuoteConfirm` |
| `migx.tpl.` | MIGX output template | `[[+field]]` | `migx.tpl.FAQ`, `migx.tpl.Testimonial` |

### Chunk Naming Examples

```
cmp.Hero                  # Hero section component
cmp.Header                # Site header
cmp.Footer                # Site footer
cmp.CTA                   # Call-to-action block
cmp.TrustBar              # Trust badges/signals
cmp.ServiceGrid           # Services grid section

tpl.ServiceCard           # pdoResources template for service cards
tpl.BlogCard              # pdoResources template for blog cards
tpl.TeamMember            # pdoResources template for team members

nav.tpl.DesktopLink       # Desktop menu link
nav.tpl.DesktopParent     # Desktop menu parent with dropdown
nav.tpl.DesktopDropdown   # Desktop dropdown wrapper
nav.tpl.MobileLink        # Mobile menu link
nav.tpl.MobileParent      # Mobile accordion parent

schema.LocalBusiness      # Organization schema
schema.Service            # Service schema
schema.FAQ                # FAQ schema
schema.BreadcrumbList     # Breadcrumb schema

migx.tpl.FAQ              # MIGX FAQ item template
migx.tpl.Testimonial      # MIGX testimonial template
migx.tpl.TeamMember       # MIGX team member template
migx.tpl.Gallery          # MIGX gallery image template
```

### Critical Rule: Placeholder Syntax by Chunk Type

| Chunk Type | Resource Fields | TVs | Settings | Snippet Data |
|------------|-----------------|-----|----------|--------------|
| `cmp.*` | `[[*pagetitle]]` | `[[*heroImage]]` | `[[++brand]]` | N/A |
| `tpl.*` | `[[+pagetitle]]` | `[[+tv.heroImage]]` | `[[++brand]]` | `[[+idx]]` |
| `nav.tpl.*` | N/A | N/A | `[[++brand]]` | `[[+link]]`, `[[+menutitle]]` |
| `migx.tpl.*` | N/A | N/A | `[[++brand]]` | `[[+field]]` |

---

## 3. Template Variables (TVs)

**Rule**: TV names must be **single camelCase words** (no underscores, no hyphens).

| Category | Prefix Pattern | Examples |
|----------|----------------|----------|
| Hero Section | `hero*` | `heroTitle`, `heroSubtitle`, `heroImage`, `heroBadge`, `heroStyle` |
| Featured/Media | `featured*` | `featuredImage`, `featuredVideo` |
| Service-specific | `service*` | `serviceIcon`, `servicePrice`, `serviceDuration` |
| Area/Location | `area*` | `areaCity`, `areaCounty`, `areaZips` |
| SEO | `seo*` | `seoTitle`, `seoDescription`, `seoCanonical` |
| MIGX Data | Descriptive | `faqs`, `testimonials`, `gallery`, `teamMembers` |
| Content | Descriptive | `introText`, `sidebarContent`, `ctaText` |

### TV Naming Examples

```
# Hero TVs
heroTitle                 # Override hero heading
heroSubtitle              # Hero subheading
heroImage                 # Hero background image (Image+)
heroBadge                 # Badge/kicker text
heroStyle                 # Hero variant selector

# Media TVs
featuredImage             # Main page image (Image+)
ogImage                   # Open Graph image override
galleryImages             # MIGX gallery data

# Service TVs
serviceIcon               # Icon name from sprite
servicePrice              # Starting price text
serviceDuration           # Typical duration

# Area TVs
areaCity                  # City name
areaCounty                # County name
areaZipCodes              # Comma-separated zips

# MIGX TVs
faqs                      # FAQ items JSON
testimonials              # Testimonial items JSON
benefits                  # Benefits list JSON
processSteps              # Process steps JSON
```

---

## 4. ClientConfig Settings

**Pattern**: `{prefix}-{descriptor}[-{modifier}]` (hyphens allowed)

| Prefix | Group | Examples |
|--------|-------|----------|
| `co-` | Company Info | `co-brand`, `co-legalname`, `co-tagline` |
| `nap-` | Contact/NAP | `nap-phone-main`, `nap-phone-tracking`, `nap-email-main` |
| `brand-` | Design Tokens | `brand-primary`, `brand-secondary`, `brand-logourl` |
| `social-` | Social Links | `social-facebook`, `social-instagram`, `social-youtube` |
| `track-` | Analytics | `track-ga4-id`, `track-gtm-id`, `track-fbpixel-id` |
| `seo-` | Schema/SEO | `seo-businesstype`, `seo-pricerange`, `seo-ratingvalue` |
| `geo-` | Service Areas | `geo-basecity`, `geo-radiusmiles`, `geo-servicecities` |
| `nav-` | Site Structure IDs | `nav-services-root`, `nav-blog-root`, `nav-contact-page` |
| `cta-` | CTA Text/URLs | `cta-primary-text`, `cta-schedule-url`, `cta-phone-text` |
| `ghl-` | GHL Integration | `ghl-location-id`, `ghl-calendar-url`, `ghl-form-url` |
| `trust-` | Trust Signals | `trust-license`, `trust-insured`, `trust-founding-year` |

### ClientConfig Examples

```
# Company Info
co-brand                  # "Acme Plumbing"
co-legalname              # "Acme Plumbing LLC"
co-tagline                # "Your Trusted Local Plumber"

# Contact/NAP
nap-phone-main            # (555) 123-4567
nap-phone-tracking        # Call tracking number
nap-email-main            # info@acmeplumbing.com
nap-address-street        # 123 Main Street
nap-address-city          # Anytown
nap-address-region        # OH
nap-address-postal        # 44444

# Brand Design Tokens
brand-primary             # #0A84FF
brand-secondary           # #111827
brand-logourl             # /assets/logo.svg

# Site Structure
nav-services-root         # Resource ID: 5
nav-blog-root             # Resource ID: 10
nav-contact-page          # Resource ID: 3

# CTAs
cta-primary-text          # "Get Free Quote"
cta-secondary-text        # "Call Now"
cta-schedule-url          # https://app.gohighlevel.com/...
```

---

## 5. Snippets

| Prefix | Purpose | Examples |
|--------|---------|----------|
| `lsb.` | Custom LSB snippets | `lsb.ServiceSchema`, `lsb.AreaBreadcrumb` |
| `util.` | Utility snippets | `util.FormatPhone`, `util.TruncateText` |
| `hook.` | FormIt hooks | `hook.CreateLead`, `hook.NotifySlack` |

### Snippet Examples

```
lsb.ServiceSchema         # Generate service JSON-LD
lsb.FAQSchema             # Generate FAQ JSON-LD  
lsb.AreaServices          # List services for an area
lsb.RelatedPosts          # Related blog posts

util.FormatPhone          # Format phone for tel: links
util.SanitizeJSON         # Clean JSON for output

hook.CreateGHLContact     # FormIt hook: create GHL contact
hook.SendToWebhook        # FormIt hook: webhook notification
```

---

## 6. Plugins

| Prefix | Purpose | Examples |
|--------|---------|----------|
| `lsb.` | LSB-specific functionality | `lsb.AutoCanonical`, `lsb.InjectSchema` |

---

## 7. Categories

Use for organizing elements in MODX Manager:

```
LSB - Core               # Core components (header, footer, head)
LSB - Hero               # Hero section elements
LSB - Services           # Service-related elements
LSB - Blog               # Blog elements
LSB - Forms              # Form chunks and hooks
LSB - Schema             # JSON-LD schema chunks
LSB - Navigation         # Menu templates
LSB - Areas              # Service area elements
LSB - MIGX               # MIGX output templates
```

---

## Quick Reference Card

| Element | Pattern | Example |
|---------|---------|---------|
| Template | `tpl.{type}.{name}` | `tpl.srv.Detail` |
| Component Chunk | `cmp.{Name}` | `cmp.Hero` |
| Snippet Template | `tpl.{Name}` | `tpl.ServiceCard` |
| Nav Template | `nav.tpl.{Name}` | `nav.tpl.DesktopLink` |
| Schema Chunk | `schema.{Name}` | `schema.LocalBusiness` |
| MIGX Template | `migx.tpl.{Name}` | `migx.tpl.FAQ` |
| Form Chunk | `form.{Name}` | `form.Contact` |
| Email Chunk | `email.{Name}` | `email.ContactNotify` |
| TV | `{category}{Name}` | `heroImage`, `serviceIcon` |
| ClientConfig | `{prefix}-{name}` | `nap-phone-main` |
| Snippet | `lsb.{Name}` | `lsb.ServiceSchema` |

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-11 | Initial naming conventions established |
