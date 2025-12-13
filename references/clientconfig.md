# ClientConfig Settings Reference

ClientConfig structure for MODX websites. This is an example organization - adapt groups and settings to your project needs.

## Settings Groups

### Group 1: Company Information
| Key | Label | Type | Notes |
|-----|-------|------|-------|
| `brand` | Brand Name | text | Public trading name |
| `legal-name` | Legal Business Name | text | Registered entity |
| `tagline` | Tagline | text | Short slogan |
| `site-url` | Website URL | text | Absolute URL with protocol |

### Group 2: Contact & NAP
| Key | Label | Type | Notes |
|-----|-------|------|-------|
| `phone-main` | Primary Phone | text | Main business line |
| `phone-tracking` | Call Tracking Number | text | If using tracking |
| `email-main` | Public Email | text | Contact email |
| `address-street` | Street Address | text | |
| `address-city` | City | text | |
| `address-region` | Region/State | text | |
| `address-postal` | Postal/ZIP | text | |
| `country` | Country Code | text | 2-letter (US, CA, UK) |
| `geo-lat` | Latitude | text | For maps/schema |
| `geo-lng` | Longitude | text | For maps/schema |
| `hours-json` | Business Hours | textarea | JSON format |

#### hours-json Format
```json
[
  {
    "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
    "opens": "08:00",
    "closes": "17:00"
  },
  {
    "dayOfWeek": ["Saturday"],
    "opens": "09:00",
    "closes": "14:00"
  }
]
```

### Group 3: Social Links
| Key | Label | Type |
|-----|-------|------|
| `social-facebook` | Facebook URL | text |
| `social-instagram` | Instagram URL | text |
| `social-linkedin` | LinkedIn URL | text |
| `social-youtube` | YouTube URL | text |
| `social-tiktok` | TikTok URL | text |
| `social-yelp` | Yelp URL | text |
| `social-gbp` | Google Business Profile URL | text |

### Group 4: Tracking & Analytics
| Key | Label | Type | Format |
|-----|-------|------|--------|
| `ga4-measurement-id` | GA4 Measurement ID | text | G-XXXXXXXXXX |
| `gtm-id` | Google Tag Manager ID | text | GTM-XXXXXXX |
| `gtag-id` | Google Ads Conversion ID | text | AW-XXXXXXXXX |
| `facebook-pixel-id` | Facebook Pixel ID | text | |
| `clarity-id` | Microsoft Clarity ID | text | |

### Group 5: Schema & SEO
| Key | Label | Type | Notes |
|-----|-------|------|-------|
| `business-type` | Schema.org Type | text | LocalBusiness subtype |
| `price-range` | Price Range | text | $, $$, $$$ |
| `service-schema-enabled` | Enable Service Schema | boolean | |
| `faq-schema-enabled` | Enable FAQ Schema | boolean | |
| `review-rating-value` | Aggregate Rating | text | e.g., 4.9 |
| `review-rating-count` | Review Count | text | e.g., 215 |

#### Common business-type Values
- Plumber
- Electrician
- HVACBusiness
- RoofingContractor
- GeneralContractor
- LocksmithService
- MovingCompany
- PestControlService
- HomeAndConstructionBusiness

### Group 6: Service Areas
| Key | Label | Type |
|-----|-------|------|
| `base-city` | Primary City | text |
| `service-cities` | Service Cities | textarea |
| `service-counties` | Service Counties | textarea |
| `radius-miles` | Service Radius (miles) | text |

Format for cities/counties: Comma-separated list
```
Anytown, Otherville, Newplace, Springfield
```

### Group 7: Media & Branding (Legacy)
| Key | Label | Type |
|-----|-------|------|
| `logo-url` | Logo URL | text |
| `logo-svg` | Inline Logo SVG | textarea |
| `primary-color` | Primary Brand Color | text |
| `secondary-color` | Secondary Brand Color | text |

### Group 810: Branding (Extended)
| Key | Label | Type | Notes |
|-----|-------|------|-------|
| `logo-url-light` | Logo URL (Light) | text | Light theme |
| `logo-url-dark` | Logo URL (Dark) | text | Dark theme |
| `logo-svg-light` | Logo SVG (Light) | textarea | Inline SVG |
| `logo-svg-dark` | Logo SVG (Dark) | textarea | Inline SVG |
| `favicon-ico-url` | Favicon .ico URL | text | |
| `favicon-svg-url` | Favicon .svg URL | text | Vector |
| `mask-icon-url` | Safari Pinned Tab | text | |
| `icon-180-url` | Apple Touch Icon | text | 180Ã—180 |
| `icon-512-url` | PWA Icon | text | 512Ã—512 |
| `og-image-url` | Open Graph Image | text | Default social image |
| `tagline-short` | Tagline (Short) | text | Badges/kickers |
| `tagline-long` | Tagline (Long) | textarea | Marketing sentence |
| `primary-contrast` | Primary Contrast | text | Text on primary |
| `accent-color-1` | Accent Color 1 | text | Gradients |
| `accent-color-2` | Accent Color 2 | text | Gradients |
| `bg-color` | Background Color | text | Page background |
| `surface-color` | Surface Color | text | Cards/panels |
| `ink-color` | Text Color | text | Primary text |
| `ink-2-color` | Secondary Text | text | Muted text |
| `gradient-from` | Gradient From | text | Start color |
| `gradient-to` | Gradient To | text | End color |
| `radius-base` | Base Radius (px) | number | Corner radius |
| `container-max` | Container Max Width | text | e.g., 1100px |
| `font-sans` | Sans-serif Stack | text | UI text fonts |
| `font-serif` | Serif Stack | text | Heading fonts |
| `theme-dark-enabled` | Enable Dark Mode | boolean | |
| `css-root-extra` | Extra CSS Variables | textarea | Custom :root vars |

### Group 820: Site Structure & IDs
| Key | Label | Type | Notes |
|-----|-------|------|-------|
| `services_root` | Services Container | number | Parent ID |
| `service_categories_root` | Service Categories | number | Parent ID |
| `highlights_root` | Business Highlights | number | Parent ID |
| `blog_root` | Blog Container | number | Parent ID |
| `locations_root` | Locations Container | number | Parent ID |
| `faqs_root` | FAQs Container | number | Parent ID |
| `testimonials_root` | Testimonials Container | number | Parent ID |
| `forms_root` | Forms Container | number | Parent ID |
| `contact_page` | Contact Page | number | Resource ID |
| `schedule_thanks_page` | Schedule Thank You | number | Resource ID |
| `contact_thanks_page` | Contact Thank You | number | Resource ID |
| `privacy_page` | Privacy Policy | number | Resource ID |
| `terms_page` | Terms & Conditions | number | Resource ID |
| `primary_menu_root` | Primary Menu Root | number | For pdoMenu |
| `footer_menu_root` | Footer Menu Root | number | For pdoMenu |
| `menu_context` | Menu Context | context | Empty = current |

### Group 830: Contact & Actions
| Key | Label | Type |
|-----|-------|------|
| `schedule-url` | Schedule URL | text |
| `chat-url` | Chat URL | text |
| `chat-provider-name` | Chat Provider | text |
| `schedule-provider-name` | Schedule Provider | text |

---

## Usage in Templates

### Basic Settings
```html
<a href="tel:[[++phone-main]]">[[++phone-main]]</a>
<a href="mailto:[[++email-main]]">[[++email-main]]</a>
```

### Address Block
```html
<address>
  [[++address-street]]<br>
  [[++address-city]], [[++address-region]] [[++address-postal]]
</address>
```

### Conditional Social Links
```html
[[++social-facebook:notempty=`
  <a href="[[++social-facebook]]" target="_blank" rel="noopener">Facebook</a>
`]]
[[++social-instagram:notempty=`
  <a href="[[++social-instagram]]" target="_blank" rel="noopener">Instagram</a>
`]]
```

### CSS Custom Properties
```html
<style>
:root {
  --color-primary: [[++primary-color]];
  --color-secondary: [[++secondary-color]];
  --color-bg: [[++bg-color:default=`#ffffff`]];
  --color-surface: [[++surface-color:default=`#f8f9fa`]];
  --color-ink: [[++ink-color:default=`#1a1a1a`]];
  --radius: [[++radius-base:default=`18`]]px;
  --container-max: [[++container-max:default=`1100px`]];
  --font-sans: [[++font-sans:default=`system-ui, sans-serif`]];
}
</style>
```

### Tracking Scripts (Conditional)
```html
[[++gtm-id:notempty=`
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){...})(window,document,'script','dataLayer','[[++gtm-id]]');</script>
`]]
```

---

## Import/Export

ClientConfig settings can be exported/imported via XML:
- Manager â†’ Extras â†’ ClientConfig â†’ Import/Export
- Settings and Groups exported separately
- Use for deploying consistent configurations across sites
