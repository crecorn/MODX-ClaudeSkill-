# Schema Markup Reference

JSON-LD structured data patterns for MODX websites.

## Table of Contents
1. [Organization/LocalBusiness Schema](#organizationlocalbusiness-schema)
2. [Service Schema](#service-schema)
3. [FAQ Schema](#faq-schema)
4. [Product Schema](#product-schema)
5. [Aggregate Rating](#aggregate-rating)
6. [Breadcrumb Schema](#breadcrumb-schema)
7. [Article/BlogPosting Schema](#articleblogposting-schema)

---

## Organization/LocalBusiness Schema

### Base Snippet Pattern

Create a snippet `schemaLocalBusiness` that outputs:

```php
<?php
/**
 * schemaLocalBusiness - Outputs LocalBusiness JSON-LD
 * Uses ClientConfig settings for business data
 */

$modx->getService('clientconfig', 'ClientConfig', $modx->getOption('clientconfig.core_path', null, MODX_CORE_PATH . 'components/clientconfig/') . 'model/clientconfig/');

$cc = function($key, $default = '') use ($modx) {
    return $modx->getOption($key, null, $default);
};

$schema = [
    '@context' => 'https://schema.org',
    '@type' => $cc('business-type', 'LocalBusiness'),
    '@id' => $cc('site-url') . '#organization',
    'name' => $cc('brand'),
    'legalName' => $cc('legal-name', $cc('brand')),
    'url' => $cc('site-url'),
    'telephone' => $cc('phone-main'),
    'email' => $cc('email-main'),
    'priceRange' => $cc('price-range', '$$'),
    'image' => $cc('logo-url'),
    'logo' => [
        '@type' => 'ImageObject',
        'url' => $cc('logo-url')
    ],
    'address' => [
        '@type' => 'PostalAddress',
        'streetAddress' => $cc('address-street'),
        'addressLocality' => $cc('address-city'),
        'addressRegion' => $cc('address-region'),
        'postalCode' => $cc('address-postal'),
        'addressCountry' => $cc('country', 'US')
    ],
    'sameAs' => array_filter([
        $cc('social-facebook'),
        $cc('social-instagram'),
        $cc('social-linkedin'),
        $cc('social-youtube'),
        $cc('social-yelp'),
        $cc('social-gbp')
    ])
];

// Add coordinates if available
if ($cc('geo-lat') && $cc('geo-lng')) {
    $schema['geo'] = [
        '@type' => 'GeoCoordinates',
        'latitude' => $cc('geo-lat'),
        'longitude' => $cc('geo-lng')
    ];
}

// Add hours from JSON setting
$hours = json_decode($cc('hours-json'), true);
if ($hours && is_array($hours)) {
    $schema['openingHoursSpecification'] = $hours;
}

// Add aggregate rating if available
if ($cc('review-rating-value') && $cc('review-rating-count')) {
    $schema['aggregateRating'] = [
        '@type' => 'AggregateRating',
        'ratingValue' => $cc('review-rating-value'),
        'reviewCount' => $cc('review-rating-count'),
        'bestRating' => '5',
        'worstRating' => '1'
    ];
}

return '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT) . '</script>';
```

### In Template (cached)
```
[[schemaLocalBusiness]]
```

---

## Service Schema

For service pages, add Service schema alongside LocalBusiness.

### Snippet Pattern (schemaService)

```php
<?php
/**
 * schemaService - Outputs Service JSON-LD for current resource
 * Call from Service Page template
 */

$cc = function($key, $default = '') use ($modx) {
    return $modx->getOption($key, null, $default);
};

$resource = $modx->resource;

$schema = [
    '@context' => 'https://schema.org',
    '@type' => 'Service',
    'name' => $resource->get('pagetitle'),
    'description' => $resource->get('description'),
    'url' => $modx->makeUrl($resource->get('id'), '', '', 'full'),
    'provider' => [
        '@type' => $cc('business-type', 'LocalBusiness'),
        '@id' => $cc('site-url') . '#organization',
        'name' => $cc('brand')
    ],
    'areaServed' => [
        '@type' => 'City',
        'name' => $cc('base-city')
    ]
];

// Add service image if available
$heroImage = $resource->getTVValue('heroImage');
if ($heroImage) {
    $schema['image'] = $cc('site-url') . ltrim($heroImage, '/');
}

return '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT) . '</script>';
```

### Multiple Service Areas

```php
// For multiple cities
$cities = array_map('trim', explode(',', $cc('service-cities')));
$schema['areaServed'] = array_map(function($city) {
    return ['@type' => 'City', 'name' => $city];
}, $cities);
```

---

## FAQ Schema

For FAQ sections - can be embedded on any page with FAQs.

### Snippet Pattern (schemaFAQ)

```php
<?php
/**
 * schemaFAQ - Outputs FAQPage JSON-LD
 * @property string $tvname - Name of MIGX TV containing FAQs
 */

$tvname = $modx->getOption('tvname', $scriptProperties, 'faqs');
$resource = $modx->resource;

$faqs = json_decode($resource->getTVValue($tvname), true);
if (!$faqs || !is_array($faqs)) return '';

$items = [];
foreach ($faqs as $faq) {
    $items[] = [
        '@type' => 'Question',
        'name' => $faq['question'],
        'acceptedAnswer' => [
            '@type' => 'Answer',
            'text' => strip_tags($faq['answer'])
        ]
    ];
}

$schema = [
    '@context' => 'https://schema.org',
    '@type' => 'FAQPage',
    'mainEntity' => $items
];

return '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT) . '</script>';
```

### In Template
```
[[++faq-schema-enabled:is=`1`:then=`[[schemaFAQ? &tvname=`faqs`]]`]]
```

---

## Aggregate Rating

Standalone rating schema if not embedded in LocalBusiness.

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "[[++brand]]",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "[[++review-rating-value]]",
    "reviewCount": "[[++review-rating-count]]",
    "bestRating": "5",
    "worstRating": "1"
  }
}
```

---

## Service Area

GeoCircle for radius-based service areas.

```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "areaServed": {
    "@type": "GeoCircle",
    "geoMidpoint": {
      "@type": "GeoCoordinates",
      "latitude": "[[++geo-lat]]",
      "longitude": "[[++geo-lng]]"
    },
    "geoRadius": "[[++radius-miles]] mi"
  }
}
```

---

## Breadcrumb Schema

Complements pdoCrumbs visual output.

### Snippet Pattern (schemaBreadcrumb)

```php
<?php
/**
 * schemaBreadcrumb - Outputs BreadcrumbList JSON-LD
 */

$resource = $modx->resource;
$parents = $modx->getParentIds($resource->get('id'));
$parents = array_reverse($parents);

$items = [];
$position = 1;

// Add home
$items[] = [
    '@type' => 'ListItem',
    'position' => $position++,
    'name' => 'Home',
    'item' => $modx->makeUrl($modx->getOption('site_start'), '', '', 'full')
];

// Add parents
foreach ($parents as $parentId) {
    if ($parentId == 0) continue;
    $parent = $modx->getObject('modResource', $parentId);
    if ($parent && $parent->get('published') && !$parent->get('deleted')) {
        $items[] = [
            '@type' => 'ListItem',
            'position' => $position++,
            'name' => $parent->get('menutitle') ?: $parent->get('pagetitle'),
            'item' => $modx->makeUrl($parentId, '', '', 'full')
        ];
    }
}

// Add current page
$items[] = [
    '@type' => 'ListItem',
    'position' => $position,
    'name' => $resource->get('menutitle') ?: $resource->get('pagetitle'),
    'item' => $modx->makeUrl($resource->get('id'), '', '', 'full')
];

$schema = [
    '@context' => 'https://schema.org',
    '@type' => 'BreadcrumbList',
    'itemListElement' => $items
];

return '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT) . '</script>';
```

---

## Complete Head Schema Block

Combine all schemas in template `<head>`:

```html
<!-- Organization Schema (all pages) -->
[[schemaLocalBusiness]]

<!-- Breadcrumb Schema (all pages except home) -->
[[*id:ne=`[[++site_start]]`:then=`[[schemaBreadcrumb]]`]]

<!-- Service Schema (service pages only) -->
[[*template:is=`4`:then=`[[schemaService]]`]]

<!-- FAQ Schema (pages with FAQs) -->
[[*faqs:notempty=`[[++faq-schema-enabled:is=`1`:then=`[[schemaFAQ]]`]]`]]
```

---

## Testing Schema

1. Google Rich Results Test: https://search.google.com/test/rich-results
2. Schema Markup Validator: https://validator.schema.org/
3. View page source â†’ search for `application/ld+json`

### Common Issues
- Missing required fields (name, address for LocalBusiness)
- Invalid JSON syntax
- URLs not absolute (must include domain)
- Image URLs not accessible
- Duplicate @id values across schemas

---

## Product Schema

For e-commerce and product pages.

### Basic Product
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[[*pagetitle]]",
  "description": "[[*description]]",
  "image": "[[*productImage]]",
  "sku": "[[*sku]]",
  "brand": {
    "@type": "Brand",
    "name": "[[++brand]]"
  },
  "offers": {
    "@type": "Offer",
    "url": "[[~[[*id]]? &scheme=`full`]]",
    "priceCurrency": "USD",
    "price": "[[*price]]",
    "availability": "https://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "[[++brand]]"
    }
  }
}
```

### Product with Reviews
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "[[*pagetitle]]",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "[[*ratingValue]]",
    "reviewCount": "[[*reviewCount]]"
  },
  "review": [{
    "@type": "Review",
    "author": {"@type": "Person", "name": "Reviewer Name"},
    "reviewRating": {"@type": "Rating", "ratingValue": "5"},
    "reviewBody": "Review text here"
  }]
}
```

---

## Article/BlogPosting Schema

For blog posts and articles.

### BlogPosting Snippet Pattern
```php
<?php
/**
 * schemaBlogPosting - Outputs BlogPosting JSON-LD
 */

$resource = $modx->resource;
$cc = function($key, $default = '') use ($modx) {
    return $modx->getOption($key, null, $default);
};

$schema = [
    '@context' => 'https://schema.org',
    '@type' => 'BlogPosting',
    'headline' => $resource->get('pagetitle'),
    'description' => $resource->get('description'),
    'url' => $modx->makeUrl($resource->get('id'), '', '', 'full'),
    'datePublished' => date('c', strtotime($resource->get('publishedon'))),
    'dateModified' => date('c', strtotime($resource->get('editedon'))),
    'author' => [
        '@type' => 'Organization',
        'name' => $cc('brand'),
        'url' => $cc('site-url')
    ],
    'publisher' => [
        '@type' => 'Organization',
        'name' => $cc('brand'),
        'logo' => [
            '@type' => 'ImageObject',
            'url' => $cc('logo-url')
        ]
    ],
    'mainEntityOfPage' => [
        '@type' => 'WebPage',
        '@id' => $modx->makeUrl($resource->get('id'), '', '', 'full')
    ]
];

// Add featured image if available
$image = $resource->getTVValue('heroImage');
if ($image) {
    $schema['image'] = $cc('site-url') . ltrim($image, '/');
}

return '<script type="application/ld+json">' . json_encode($schema, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT) . '</script>';
```

---

## Common Schema Types by Site Type

| Site Type | Primary Schema | Additional |
|-----------|----------------|------------|
| Local Service | LocalBusiness | Service, FAQ, AggregateRating |
| E-commerce | Product, Organization | Offer, AggregateRating, Review |
| Blog/News | BlogPosting, Article | Organization, BreadcrumbList |
| Directory | ItemList, LocalBusiness | AggregateRating |
| Lead Gen | Organization, Service | FAQ, ContactPoint |
| SaaS | SoftwareApplication | Organization, FAQ, Offer |
