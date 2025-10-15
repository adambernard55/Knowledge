


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
├── crawlability-and-indexation.md
├── core-web-vitals.md
├── page-speed-optimization.md
├── mobile-and-responsive-seo.md
├── javascript-and-rendering.md
├── structured-data-and-sitemaps.md
├── site-migrations-and-canonicalization.md
└── edge-seo-implementation.md
```

Add: `Edge SEO` here (currently under ai‑seo but fits architectural SEO).

---

### **4_ai-and-automation/**

> Dedicated space for how AI transforms traditional SEO.

Merge `ai-seo/` and add conceptual links to `Knowledge/AI`.

```
4_ai-and-automation/
├── ai-in-seo-overview.md
├── ai-keyword-research.md
├── ai-content-optimization.md
├── agentic-seo.md
├── semantic-search-and-vector-seo.md
├── geo-optimization.md
└── automation-workflows.md
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
