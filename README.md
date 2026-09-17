[README v0.0.1.md](https://github.com/user-attachments/files/32347934/README.v0.0.1.md)
# UNISEFE-AUTOPOST
AUTOMATIC POST - WP PLUGIN
# UNISEFE Autopost

**Version:** 0.0.1  
**Platform:** WordPress  
**Type:** Campaign-based content automation  
**Architecture:** Single-file plugin  
**Sources:** Prompt / Sitemap / RSS-Atom  
**AI Providers:** Mock / OpenRouter / OpenAI / Together AI  
**JavaScript:** None  
**AJAX:** None  
**License:** MIT  

---

UNISEFE Autopost is a native WordPress campaign engine for scheduled content generation and publishing.

A campaign defines:

```text
SOURCE
+
AI PROVIDER
+
INSTRUCTIONS
+
WORDPRESS DESTINATION
+
FREQUENCY
```

Each campaign owns its schedule.

There is no global "publish every 5 minutes" setting.

---

## Campaign Model

```text
UNISEFE Autopost

Campaign A
├── Source: Sitemap
├── Provider: OpenRouter
├── Frequency: 2 hours
├── Post Type: Post
└── Status: Active

Campaign B
├── Source: Prompt / Titles
├── Provider: Mock
├── Frequency: 30 minutes
├── Post Type: Post
└── Status: Active

Campaign C
├── Source: RSS / Atom
├── Provider: OpenAI
├── Frequency: 1 day
├── Post Type: Custom Post Type
└── Status: Paused
```

---

## Per-Campaign Scheduling

UNISEFE Autopost does not use one fixed recurring cron event.

Each active campaign receives its own native WordPress single event.

```text
Campaign saved
      │
      ▼
next_run calculated
      │
      ▼
wp_schedule_single_event()
      │
      ▼
campaign executes
      │
      ▼
last_run updated
      │
      ▼
next_run calculated
      │
      ▼
next single event scheduled
```

Changing the campaign frequency clears the old event and creates the new one.

Pausing a campaign removes its pending event.

Resuming it creates a new event.

---

## Sources

### Prompt / Title List

One item per line.

```text
How steel structures resist wind
Bolted versus welded connections
Design principles for industrial steel halls
```

One item is consumed per campaign execution.

### Sitemap

The campaign reads:

```text
sitemap.xml
```

or a sitemap index.

Already processed URLs are remembered per campaign.

### RSS / Atom

The campaign reads the latest feed items and skips items already processed.

---

## AI Providers

### Mock / Test

```text
Cost: 0
API key: not required
External request: no
```

Mock/Test executes the campaign pipeline without calling any paid AI API.

It can test:

- campaign creation;
- schedule creation;
- pause / resume;
- run now;
- prompt queue;
- sitemap discovery;
- feed discovery;
- post type;
- taxonomy;
- category;
- draft / publish status;
- WordPress post creation;
- `last_run`;
- `next_run`;
- processed-source state.

For safe testing, use:

```text
Provider = Mock / Test
Post Status = Draft
```

### OpenRouter

Default model:

```text
openrouter/free
```

Endpoint:

```text
https://openrouter.ai/api/v1/chat/completions
```

### OpenAI

Default model:

```text
gpt-5.6
```

Endpoint:

```text
https://api.openai.com/v1/responses
```

### Together AI

Default model:

```text
Qwen/Qwen2.5-7B-Instruct-Turbo
```

---

## Native WordPress Admin

Campaign actions:

```text
Edit
Run now
Pause / Resume
Duplicate
Delete
```

The campaigns table shows:

```text
Campaign
Source
Provider
Frequency
Last run
Next run
Status
```

No custom JavaScript application is used.

---

## Architecture

```text
unisefe-autopost/
│
├── unisefe-autopost.php
├── README.md
└── LICENSE
```

Runtime:

```text
unisefe-autopost.php
```

---

## No JavaScript / No AJAX

```text
JavaScript = 0
AJAX       = 0
```

All administration actions use native WordPress forms, `admin-post.php`, nonces and redirects.

No taxonomy AJAX selector is required.

Taxonomies and categories are selected directly from native WordPress data loaded server-side.

```text
Taxonomy selector
+
Term selector
+
Category selector
```

No AJAX lookup is required.

---

## Design Principle

```text
WordPress first
PHP first
Native admin first
Single file when possible
Vanilla JavaScript only when truly necessary
AJAX only when technically necessary
No unnecessary dependencies
```

---

## Installation

1. Download the plugin ZIP.
2. Open **WordPress → Plugins → Add Plugin**.
3. Upload the ZIP.
4. Activate **UNISEFE Autopost**.
5. Open **UNISEFE Autopost → Campaigns**.
6. Create a campaign.
7. For a free test choose **Mock / Test** and **Draft**.

---

## Version

**0.0.1**

First UNISEFE campaign-engine release.

---

## Author

**Riccardo Bastillo**  
UNISEFE

---

## License

MIT License
