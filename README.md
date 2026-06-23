# SEO AI Agent Pipeline

ระบบ AI Agent สำหรับ SEO Agency โครงสร้าง **3 Main / 6 Sub** ครอบคลุมตั้งแต่วางกลยุทธ์ → ผลิต → ตรวจคุณภาพ ก่อน publish

## แผนผัง Pipeline

```
                    ┌───────────────────────────┐
                    │  USER / CLIENT BRIEF      │
                    └────────────┬──────────────┘
                                 ▼
            ┌────────────────────────────────────────┐
            │   1. seo-strategist (The Strategist)   │ ← Main
            └──────┬───────────┬───────────────┬─────┘
                   ▼           ▼               ▼
       ┌──────────────┐ ┌────────────┐ ┌────────────────────┐
       │ 1.1 keyword- │ │ 1.2 content│ │ 1.3 performance-   │
       │     intel    │ │  -architect│ │    analyst (GSC)   │
       └──────────────┘ └────────────┘ └────────────────────┘
                     │                       │
                     └───────────┬───────────┘
                                 ▼
                     [Strategy Brief + Blueprint]
                                 ▼
            ┌────────────────────────────────────────┐
            │ 2. seo-creator-tech (Creator&Tech)     │ ← Main
            └────────┬───────────────────────┬───────┘
                     ▼                       ▼
        ┌────────────────────┐   ┌────────────────────────┐
        │ 2.1 seo-semantic-  │   │ 2.2 seo-tech-schema    │
        │     copywriter     │   │ (Technical & Schema)   │
        └────────────────────┘   └────────────────────────┘
                     │                       │
                     └───────────┬───────────┘
                                 ▼
                  [Content + Technical Package]
                                 ▼
            ┌────────────────────────────────────────┐
            │   3. seo-qc-lead (Quality Controller)  │ ← Main
            └────────┬───────────────────────┬───────┘
                     ▼                       ▼
        ┌────────────────────┐   ┌────────────────────────┐
        │ 3.1 seo-fact-brand │   │ 3.2 seo-compliance-    │
        │ (Fact + Brand)     │   │     guard (Google/AEO) │
        └────────────────────┘   └────────────────────────┘
                     │                       │
                     └───────────┬───────────┘
                                 ▼
                          [QC Verdict]
                                 ▼
                  PASS → publish / FAIL → ส่งกลับ
```

## รายชื่อ Agents

### Main Agents (Orchestrators)
| File | บทบาท | เรียกใช้เมื่อ |
|------|------|--------------|
| `seo-strategist.md` | The Strategist | เริ่มโปรเจกต์ใหม่ / วางกลยุทธ์ |
| `seo-creator-tech.md` | Creator & Technician | มี brief แล้วต้องการผลิตจริง |
| `seo-qc-lead.md` | Quality Controller | มี deliverable พร้อมตรวจก่อน publish |

### Sub-Agents
| File | บทบาท | สังกัด |
|------|------|--------|
| `seo-keyword-intel.md` | Competitor & Keyword Intelligence | Strategist |
| `seo-content-architect.md` | Topic Cluster Planner | Strategist |
| `seo-performance-analyst.md` | GSC Performance Analyst (ต้องใช้ MCP) | Strategist |
| `seo-semantic-copywriter.md` | Semantic SEO Copywriter | Creator |
| `seo-tech-schema.md` | Technical & Schema Optimizer | Creator |
| `seo-fact-brand.md` | Fact-Checker & Brand Alignment | QC |
| `seo-compliance-guard.md` | AI & Search Engine Compliance | QC |

### MCP Integration
- `seo-performance-analyst` ต้องการ `mcp-gsc-server` (โฟลเดอร์ `../mcp-gsc-server/`)
- Setup: ดู `../mcp-gsc-server/SETUP.md`
- ลงทะเบียนผ่าน `.mcp.json` ที่ root project แล้ว — เปิด Claude Code session ใหม่หลัง setup

## วิธีเรียกใช้

### Workflow ครบวงจร
สั่ง main agent ทีละขั้น:
```
1. "ใช้ seo-strategist วางแผน SEO สำหรับธุรกิจ [X] เป้าหมาย [Y]"
   → ได้ Strategy Brief + Content Blueprint

2. "ใช้ seo-creator-tech ผลิต content ตาม blueprint นี้: [paste]"
   → ได้ Articles + Technical Package

3. "ใช้ seo-qc-lead ตรวจ deliverable นี้: [paste]"
   → ได้ QC Verdict
```

### เรียก sub-agent ตรง (เฉพาะงานเล็ก)
ทำได้ในกรณีที่งานชัดเจนและไม่ต้องการ orchestration เต็มรูปแบบ:
```
"ใช้ seo-keyword-intel หา keyword opportunities สำหรับ niche [X]"
"ใช้ seo-tech-schema สร้าง JSON-LD สำหรับ FAQ page นี้"
"ใช้ seo-fact-brand ตรวจ fact ใน article นี้"
```

## หลักการออกแบบ

1. **Separation of concerns** — แต่ละ agent มีบทบาทชัดเจน ไม่ซ้ำซ้อน
2. **Orchestrator pattern** — Main agents สั่ง sub-agents ผ่าน `Agent` tool
3. **Hand-off ชัดเจน** — output ของ stage หนึ่งเป็น input ของ stage ถัดไป
4. **Quality gate** — ทุก deliverable ผ่าน QC ก่อน publish เสมอ
5. **Parallelizable** — sub-agents ในชั้นเดียวกันรันพร้อมกันได้
6. **AEO-ready** — คิดถึงทั้ง traditional SEO และ AI search ตั้งแต่ต้น

## ข้อควรรู้

- Agents เป็น **project-scoped** — ใช้ได้เฉพาะใน working directory นี้
- ภาษา system prompt: **ไทย** (technical term ใช้อังกฤษได้)
- Model: Main agents = `opus`, Sub-agents = `sonnet`
- Tools: full access (รวม WebSearch/WebFetch สำหรับวิเคราะห์ SERP/คู่แข่ง)

## การปรับแต่ง

แก้ system prompt ใน `.md` ไฟล์แต่ละตัวเพื่อ:
- เพิ่ม brand guidelines เฉพาะของลูกค้า
- กำหนด niche ที่เชี่ยวชาญ (เช่น e-commerce, B2B SaaS, healthcare)
- เปลี่ยน output format
- เพิ่ม checklist เฉพาะ

หาก agent บางตัวต้องการ tool น้อยลง เช่น QC ที่ไม่ควร edit ไฟล์:
แก้ frontmatter `tools:` ให้เป็น read-only set
