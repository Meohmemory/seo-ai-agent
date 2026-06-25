# SEO AI Agent System

ระบบ Multi-Agent สำหรับทำ SEO แบบครบวงจร — ตั้งแต่รับ brief จนถึง publish-ready content พร้อม technical package และ QC

---

## Pipeline Flow

```
USER
  │
  ▼
┌─────────────────────────────────┐
│         seo-ae                  │  ← Entry point (Main Agent)
│     Account Executive           │
│  - รับ brief จากผู้ใช้           │
│  - ถามให้ครบ (≤ 5 คำถาม)        │
│  - สรุปผลกลับผู้ใช้             │
└──────────────┬──────────────────┘
               │ internal brief
               ▼
┌─────────────────────────────────┐
│       seo-strategist            │  ← Main Agent (Strategy Layer)
│        The Strategist           │
│  - กำหนดกลยุทธ์ SEO             │
│  - Orchestrate sub-agents       │
│  - สร้าง Strategy Brief         │
└──────┬───────────────┬──────────┘
       │               │
       ▼               ▼
┌─────────────┐  ┌─────────────────────────┐
│seo-intel-   │  │  seo-performance-analyst │
│architect    │  │   (GSC-powered)          │
│             │  │                          │
│Sub-Agent 1.1│  │  Sub-Agent 1.2           │
│- Keyword    │  │  - Google Search Console │
│  research   │  │  - Clicks/CTR/Position   │
│- SERP       │  │  - Striking-distance     │
│  analysis   │  │    keywords              │
│- Topic      │  │  - Indexing status       │
│  cluster    │  │  - Sitemap analysis      │
│  blueprint  │  │                          │
└──────┬──────┘  └───────────┬─────────────┘
       │                     │
       └──────────┬──────────┘
                  │ Strategy Brief
                  ▼
┌─────────────────────────────────┐
│       seo-creator-tech          │  ← Main Agent (Production Layer)
│    The Creator & Technician     │
│  - แปลง blueprint → content จริง│
│  - Orchestrate sub-agents       │
│  - รวม output เป็น launch package│
└──────┬───────────────┬──────────┘
       │               │
       ▼               ▼
┌─────────────┐  ┌─────────────────────────┐
│seo-semantic-│  │    seo-tech-schema       │
│copywriter   │  │                          │
│             │  │  Sub-Agent 2.2           │
│Sub-Agent 2.1│  │  - JSON-LD Schema        │
│- Pillar page│  │  - Meta tags             │
│- Cluster    │  │    (title/OG/Twitter)    │
│  article    │  │  - Canonical / hreflang  │
│- Landing    │  │  - URL structure         │
│  page / FAQ │  │  - Image alt / headings  │
│- Semantic   │  │  - Page speed checks     │
│  & entity   │  │  - Robots / Sitemap      │
│  coverage   │  │                          │
└──────┬──────┘  └───────────┬─────────────┘
       │                     │
       └──────────┬──────────┘
                  │ article + technical package
                  ▼
┌─────────────────────────────────┐
│       seo-qc-reviewer           │  ← Sub-Agent 3.1
│     QC Reviewer                 │
│  - Fact-check & citations       │
│  - Brand voice & messaging      │
│  - Google Search Essentials     │
│  - E-E-A-T & Helpful Content    │
│  - Schema validity              │
│  - AEO / AI Overview readiness  │
└──────────────┬──────────────────┘
               │ QC Report + Verdict
               ▼
        ✅ PUBLISH-READY
       (หรือส่งกลับแก้ไข)
               │
               ▼
            seo-ae
     (สรุปผลกลับผู้ใช้)
```

---

## Agents Overview

### Main Agents

| Agent | Role | Model |
|-------|------|-------|
| `seo-ae` | Account Executive — หน้าบ้าน รับ brief, รายงานผล, Monthly Report & SEO Audit | opus |
| `seo-strategist` | The Strategist — กำหนดกลยุทธ์ & orchestrate layer 1 | opus |
| `seo-creator-tech` | The Creator & Technician — ผลิตงาน & orchestrate layer 2 | opus |

### Sub-Agents

| Agent | Role | Model |
|-------|------|-------|
| `seo-intel-architect` | Keyword Intelligence + Content Architecture | sonnet |
| `seo-performance-analyst` | SEO Performance via Google Search Console | sonnet |
| `seo-semantic-copywriter` | Semantic SEO Copywriter | sonnet |
| `seo-tech-schema` | Technical SEO & Structured Data | sonnet |
| `seo-qc-reviewer` | QC: Fact-check + Brand + Compliance | sonnet |

---

## Agent Files

```
SEO AI agent/
├── seo-ae.md                  # Main: Account Executive
├── seo-strategist.md          # Main: The Strategist
├── seo-creator-tech.md        # Main: The Creator & Technician
├── seo-intel-architect.md     # Sub 1.1: Keyword + Architecture
├── seo-performance-analyst.md # Sub 1.2: GSC Performance
├── seo-semantic-copywriter.md # Sub 2.1: SEO Copywriter
├── seo-tech-schema.md         # Sub 2.2: Technical & Schema
└── seo-qc-reviewer.md         # Sub 3.1: QC Reviewer
```

---

## How to Use

1. **เริ่มที่ `seo-ae`** — ใส่ brief ของโปรเจกต์ เช่น ชื่อเว็บ, กลุ่มเป้าหมาย, เป้าหมาย
2. AE จะถาม clarifying questions (ไม่เกิน 5 ข้อ)
3. AE ส่ง brief ให้ `seo-strategist` โดยอัตโนมัติ
4. Strategist เรียก sub-agents วิเคราะห์ keyword และ performance
5. Creator & Technician รับ strategy brief → ผลิตคอนเทนต์ + technical package
6. QC Reviewer ตรวจก่อน publish
7. AE สรุปผลกลับมาให้คุณ พร้อมถามว่า approve หรือต้องการ revision

---

## Required MCP (Optional)

- **`mcp-gsc-server`** — ใช้โดย `seo-performance-analyst` เพื่อดึงข้อมูลจาก Google Search Console (จำเป็นเฉพาะเมื่อมีเว็บที่ live แล้ว)

---

## Monthly Report (AE Skill)

AE รับผิดชอบจัดทำ Monthly SEO Report โดยตรง — ไม่ผ่าน seo-strategist

**Trigger:** "monthly report", "รายงานประจำเดือน", "สรุปเดือน", "report เดือนนี้"

**Flow:**
```
AE → seo-performance-analyst (GSC data)
   → seo-strategist (recommendations)
   → AE สังเคราะห์ → monthly-report-[YYYY-MM].md
```

**Report ครอบคลุม:** Executive Summary, KPIs (Clicks/Impressions/CTR/Position), Top Keywords, Striking-Distance Keywords, Top Pages, ปัญหาที่พบ, แผนเดือนหน้า, Strategic Recommendations

---

## SEO Audit (AE Skill)

AE รับผิดชอบจัดทำ SEO Audit Report หลังได้ brief จาก user — เรียก sub-agents พร้อมกัน (parallel) แล้วสังเคราะห์ผลเป็น report ฉบับเดียว

**Trigger:** "seo audit", "ตรวจเว็บ", "audit เว็บ", "เช็ค seo", "audit ก่อนเริ่ม"

**Flow:**
```
AE → seo-intel-architect     (keyword gap + competitor + content gap)
   → seo-tech-schema         (meta, schema, crawlability, speed)
   → seo-performance-analyst (GSC: rankings, CTR, indexing)
   → AE สังเคราะห์ → seo-audit-[domain]-[YYYY-MM-DD].md
```

**Report ครอบคลุม:** Critical Issues, Overview Score, Technical Audit, Keyword & Competitor Audit, Performance Audit (GSC), Action Plan (Quick Wins / Medium / Long-Term)

---

## Output ที่ได้รับ

- **Strategy Brief** — keyword map, topic cluster, content blueprint
- **SEO Article** — semantic-optimized content พร้อม meta suggestions
- **Technical Package** — JSON-LD schema, meta tags, canonical, sitemap recommendations
- **QC Report** — verdict พร้อม issue list ก่อน publish
