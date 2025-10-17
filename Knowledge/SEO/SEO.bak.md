# SEO Knowledge Base

```dataview 
TABLE WITHOUT ID key AS "Table of Contents" FROM "Knowledge/SEO" GROUP BY file.folder SORT key ASC
```

```
Knowledge/SEO/
├── 0_fundamentals/
├── 1_research-and-strategy/
├── 2_content-and-on-page/
├── 3_technical-seo/
├── 4_ai-and-automation/
├── 5_measurement-and-optimization/
└── 6_tools-and-platforms/
```

### **0_fundamentals/**

> Introduce the pillars of SEO theory and core terminology.

**Keep & refine:**

- what-is-seo.md
- how-search-engines-work.md
- search-intent.md
- serp-features.md
- eeat-signals.md

Add:

- `seo-evolution-and-policy.md` → timeline + Google updates / regulatory trends
- Each file should start with YAML front matter for `difficulty`, `seo_category`, `kb_status`, `tags`, and `related_topics`.

---

### **1_research-and-strategy/**

> From keyword research to integrated content strategy.

Merge `keyword-research/` + some `fundamentals` concepts.

Proposed contents:

```
1_research-and-strategy/
├── keyword-research-basics.md
├── search-intent-and-user-journeys.md
├── competitor-and-gap-analysis.md
├── content-audit-framework.md
├── topical-authority-and-clustering.md
└── seo-strategy-frameworks.md
```

→ Purpose: Define _why_ and _how_ SEO decisions are made before execution.

---

### **2_content-and-on-page/**

> Explain optimization techniques around content, structure, and internal linking.

Merge on-page and relevant semantic topics.

```
2_content-and-on-page/
├── content-architecture.md
├── title-tags-and-meta.md
├── header-structure.md
├── url-and-slug-best-practices.md
├── internal-linking.md
├── schema-and-rich-results.md
└── content-optimization-guide.md
```

→ Purpose: Foundational “craft” of content and layout.

---

### **3_technical-seo/**

> Infrastructure for crawler accessibility, indexing, and performance.

Keep existing `technical-seo/` files, structure them by theme.

```
3_technical-seo/
├── 1_crawlability-and-indexation.md  (Can they get in?)
├── 2_javascript-and-rendering.md     (Can they see the content?)
├── 3_core-web-vitals.md              (Is the experience good?)
├── 4_page-speed-optimization.md      (Is it fast?)
├── 5_mobile-and-responsive-seo.md    (Does it work on all devices?)
├── 6_semantic-seo.md                 (Do they understand the content?) 
├── 7_site-migrations-and-canonicalization.md (How to manage changes?)
└── 8_edge-seo-implementation.md      (Advanced technical implementation)
```

Add: `Edge SEO` here (currently under ai‑seo but fits architectural SEO).

---

### **4_ai-and-automation/**

> Dedicated space for how AI transforms traditional SEO.

Merge `ai-seo/` and add conceptual links to `Knowledge/AI`.

```
4_ai-and-automation/
├── 1_ai-in-seo-overview.md  <-- Stays as the main intro
├── 1_using-ai-for-seo/
│   ├── 1_ai-powered-text-generation.md
│   ├── 2_ai-powered-image-generation.md
│   ├── 3_ai-powered-video-creation.md
│   ├── 4_best-practices-for-ai-visuals.md
│   ├── 5_ai-keyword-research.md
│   ├── 6_ai-content-optimization.md
│   ├── 7_ai-content-marketing-campaigns.md
│   └── index.md
├── 2_optimizing-for-ai/
│   ├── 1_geo-optimization.md
│   ├── 2_agentic-seo.md
│   └── index.md
└── index.md
```


→ Purpose: Bridge human SEO strategy with AI/ML infrastructure (integrate with AI/1_methods-and-systems and AI tools).

---

### **5_measurement-and-optimization/**

> Analytics, experimentation, and feedback loops.

```
5_measurement-and-optimization/
├── seo-analytics-basics.md
├── rank-tracking-and-reporting.md
├── conversion-rate-optimization.md
├── a-b-testing-for-seo.md
├── user-experience-signals.md
├── ongoing-site-maintenance.md
└── forecasting-and-roi-models.md
```

→ Purpose: Continuous improvement, mirroring the "feedback loop" concepts in AI stack.

---

### **6_tools-and-platforms/**

> Practical reference: tools, stacks, implementations.

Expand and format similar to AI's tools structure:

```
6_tools-and-platforms/
├── overview.md
├── keyword-and-research-tools.md
├── content-and-on-page-tools.md
├── technical-analysis-tools.md
├── ai-seo-tools.md
├── analytics-and-measurement-tools.md
└── implementation-guides/
    ├── surfseo-integration.md
    ├── semrush-vs-ahrefs.md
    └── workflow-templates.md
```

---
# SEO Knowledge Base Structure

Each numbered directory represents a conceptual progression—from fundamentals to advanced analysis.

---

## 0_fundamentals/

**Purpose:** Introduces SEO theory, search engine mechanisms, and ranking principles.

### Contains
- what-is-seo.md
- how-search-engines-work.md
- search-intent.md
- serp-features.md
- eeat-signals.md

---

## 1_research-and-strategy/

**Purpose:** Develop keyword, user-intent, and content strategies.

### Contains
- keyword-research-basics.md
- search-intent-and-user-journeys.md
- competitor-and-gap-analysis.md
- topical-authority-and-clustering.md
- seo-strategy-frameworks.md

---

## 2_content-and-on-page/
