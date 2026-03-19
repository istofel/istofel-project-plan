# istofel_project_plan

A professional Claude skill that guides you through the complete product planning process — from raw idea to implementation-ready documentation — in three structured steps: **MVP Scope → PRD → SPEC**.

Each document is generated one at a time. Claude asks for your confirmation before moving to the next step, ensuring you review and approve each phase before proceeding.

---

## What It Does

Given a product idea, this skill produces three professional technical documents:

| Document | Purpose |
|----------|---------|
| **MVP Scope** | Market research, competitive landscape, recommended tech stack, architecture overview, data model preview, business rules, monetization, feature roadmap, and risks |
| **PRD** | Design principles, personas, use case map, functional requirements with acceptance criteria, UI states, ASCII layout, user flows, feature specification, data schema, sprint roadmap, and global acceptance criteria |
| **SPEC** | Module-by-module technical specification with typed signatures, critical logic, sequence diagrams, DB schema with constraints, API contracts, error hierarchy, security, observability, test strategy, and CI/CD pipeline |

---

## Installation

This skill is distributed as a `.skill` file compatible with [Claude.ai](https://claude.ai).

### Option A — Install from Release

1. Download `istofel_project_plan.skill` from the [Releases](https://github.com/istofel/istofel_project_plan/releases) page
2. Open [Claude.ai](https://claude.ai)
3. Go to **Settings → Skills**
4. Click **Install Skill** and upload the `.skill` file

### Option B — Build from Source

```bash
git clone https://github.com/istofel/istofel_project_plan.git
cd istofel_project_plan
```

Then package it:

```python
python -c "
import zipfile, os
with zipfile.ZipFile('istofel_project_plan.skill', 'w', zipfile.ZIP_DEFLATED) as zf:
    for root, dirs, files in os.walk('istofel-project-plan'):
        for file in files:
            filepath = os.path.join(root, file)
            arcname = os.path.relpath(filepath, os.path.dirname('istofel-project-plan'))
            zf.write(filepath, arcname)
print('Done: istofel_project_plan.skill')
"
```

Upload the generated `.skill` file to Claude.ai as described in Option A.

---

## How to Use

### Step 1 — Start with your idea

Trigger the skill by describing your product. You can be as brief or detailed as you like. The skill will ask clarifying questions if critical information is missing.

**Example prompt:**

```
I want to build a mobile app that helps freelancers track their working hours and 
generate invoices automatically. Target users are designers and developers who work 
for multiple clients. I want to charge a monthly subscription.
```

**Or even just:**

```
Help me plan a SaaS for restaurant inventory management.
```

Claude will ask up to 5 focused questions if anything critical is missing (target audience, monetization model, distribution platform, tech constraints), then generate the MVP Scope.

---

### Step 2 — Review the MVP Scope

Claude generates the full MVP Scope document covering:

- Product vision and value proposition
- Market size and competitive landscape
- Recommended tech stack with trade-offs
- Architecture overview (ASCII diagram)
- Data model preview
- Core business rules
- Feature roadmap (Essential MVP / Post-MVP v1 / Future)
- Monetization model
- Risks and out-of-scope items
- Executive summary

After reviewing, Claude asks:

> *"Would you like to proceed to the PRD?"*

Reply **yes** to continue or request changes first.

---

### Step 3 — Review the PRD

Claude generates the full PRD covering:

- Design principles
- Personas with use case map
- Functional requirements with acceptance criteria and error handling
- Non-functional requirements
- ASCII layout of the main interface
- Screen states (offline, empty, loading, error, ready)
- User flows as pseudoflowcharts (onboarding, happy path, etc.)
- Feature specification with components, rules, and MVP limitations
- Complete data schema with indexes and migration strategy
- Sprint roadmap
- Global acceptance criteria (binary definition of done)
- Risks, out-of-scope, and glossary

After reviewing, Claude asks:

> *"Would you like to proceed to the SPEC?"*

---

### Step 4 — Review the SPEC

Claude generates the full SPEC covering:

- Architecture diagram with layers and protocols
- Project directory tree with per-file responsibility
- Global constants and environment variables
- Module-by-module specification (typed signatures, critical logic, framework notes)
- Session state documentation
- Complete SQL schema with CHECK constraints and migration strategy
- API contracts (internal endpoints + external APIs consumed)
- Error hierarchy with UI handling per exception type
- Security checklist (sanitization, rate limiting, secrets management)
- Observability (logging format, metrics, alerts)
- Test strategy with fixtures, critical tests per module, and coverage targets
- Sequence diagrams for critical flows
- CI/CD pipeline and environments

---

## Tips for Best Results

### Provide context upfront

The more context you give initially, the fewer clarifying questions Claude needs to ask. Ideal input covers:

| Information | Example |
|-------------|---------|
| **What it does** | "A local AI assistant that runs fully offline" |
| **Who uses it** | "Python developers who want privacy" |
| **Core problem** | "Existing tools require cloud APIs or Docker" |
| **Distribution** | "pip install, desktop app, web SaaS, mobile" |
| **Monetization** | "Free/open-source, freemium, monthly subscription" |
| **Tech constraints** | "Must be Python, team of 1, no paid infra" |
| **Key features** | "Chat, file upload, conversation history, metrics" |

### Review before proceeding

Each document builds on the previous one. If something is wrong in the MVP Scope (wrong stack choice, incorrect target audience), it will propagate to the PRD and SPEC. Take time to review each step.

### Request changes before moving on

You can ask Claude to revise any section before confirming to proceed. For example:

```
Before we move to the PRD, please revise the tech stack section — 
I want to use FastAPI instead of Django, and PostgreSQL instead of SQLite.
```

### The skill flags what you forgot

Throughout all three documents, Claude proactively signals omitted but important items with:

> 💡 **Suggestion:** [explanation of what was missed and why it matters]

These cover: data retention policy, LGPD/GDPR compliance, observability setup, i18n, accessibility (WCAG), test strategy, license definition, rollback strategy for third-party dependencies.

---

## Document Examples

### MVP Scope — excerpt

```markdown
## 3. Recommended Tech Stack

| Layer       | Choice         | Alternative   | Trade-off                                      |
|-------------|----------------|---------------|------------------------------------------------|
| Backend     | FastAPI        | Django        | FastAPI is lighter and async-native; Django has more batteries |
| Database    | SQLite         | PostgreSQL    | SQLite needs zero infra; migrate to Postgres at 10k+ users |
| Auth        | JWT (PyJWT)    | Auth0         | JWT is self-contained; Auth0 adds cost but saves implementation time |
| Deploy      | Railway        | Fly.io        | Railway is simpler for solo devs; Fly gives more control |

**Premise:** Single-developer team. No existing infrastructure.

## 9. Feature Roadmap

| Feature              | Priority       | Complexity | Dependencies     |
|----------------------|----------------|------------|------------------|
| User auth (JWT)      | Essential MVP  | Medium     | —                |
| Dashboard overview   | Essential MVP  | Medium     | Auth             |
| Invoice generation   | Essential MVP  | High       | Dashboard        |
| PDF export           | Post-MVP v1    | Medium     | Invoice          |
| Stripe integration   | Post-MVP v1    | High       | Invoice          |
| Mobile app           | Future         | High       | API stable       |
```

---

### PRD — excerpt

```markdown
## RF-03: Invoice Generation

**Description:** The system must allow users to generate invoices from tracked time entries.

**Acceptance criteria:**
- User selects a client and a date range
- System calculates total hours and applies the configured hourly rate
- Invoice is rendered with line items, subtotal, taxes, and total
- User can edit line items before finalizing
- Finalized invoice receives a sequential number and is locked for editing

**Rules:**
- Hourly rate defaults to the client's configured rate; can be overridden per invoice
- Tax rate is configured per user account (default: 0%)
- Invoice number format: `INV-{YEAR}-{SEQUENCE}` (e.g., INV-2026-0042)

**Error handling:**
- No time entries in selected range → show empty state with CTA to log hours
- Client has no hourly rate configured → show inline warning before generation

---

## 6.3 Screen States

| State      | Trigger                    | What appears                                      |
|------------|----------------------------|---------------------------------------------------|
| Empty      | No invoices yet            | Illustration + "Generate your first invoice" CTA  |
| Loading    | Fetching invoice list      | Skeleton rows                                     |
| Ready      | Data loaded                | Invoice list with filters                         |
| Error      | API failure                | Banner: "Could not load invoices. Try again."     |
```

---

### SPEC — excerpt

```markdown
## 4. Module Specification

### Module: Invoice Service
**File:** `src/invoices/service.py`  
**Responsibility:** Orchestrates invoice creation — aggregates time entries, applies rates and taxes, persists to DB. Does NOT handle PDF rendering or email delivery.

```python
@dataclass
class InvoiceLineItem:
    description: str
    hours: float
    rate: float
    amount: float  # hours * rate

@dataclass
class InvoiceResult:
    id: str
    number: str          # INV-2026-0042
    client_id: str
    line_items: list[InvoiceLineItem]
    subtotal: float
    tax_rate: float
    tax_amount: float
    total: float
    created_at: str      # ISO 8601 UTC

class InvoiceService:
    def create(
        self,
        user_id: str,
        client_id: str,
        date_from: str,
        date_to: str,
    ) -> InvoiceResult:
        """Creates invoice from time entries in date range.
        
        Raises:
            NoTimeEntriesError: If no entries exist in range.
            ClientNotFoundError: If client does not exist.
            MissingHourlyRateError: If client has no rate configured.
        """
```

### Sequence Diagram — Invoice Creation

```
User      API        InvoiceService   TimeEntryRepo   InvoiceRepo    DB
  │         │               │               │              │           │
  │─ POST ─→│               │               │              │           │
  │         │─ create() ───→│               │              │           │
  │         │               │─ find_by_range→│              │           │
  │         │               │←── entries ───│              │           │
  │         │               │─ calc totals  │              │           │
  │         │               │─ save() ──────────────────→│─ INSERT ──→│
  │         │               │←── invoice ───────────────│            │
  │         │←── 201 ───────│               │              │           │
  │←── resp─│               │               │              │           │
```
```

---

## Repository Structure

```
istofel-project-plan/
├── SKILL.md                        # Main skill logic, flow, triggers, proactive rules
└── references/
    ├── mvp-scope.md                # Complete MVP Scope structure and checklist
    ├── prd.md                      # Complete PRD structure and checklist
    └── spec.md                     # Complete SPEC structure and checklist
```

---

## License

MIT License — Copyright (c) 2026 Vinícius Istofel Oliveira.

See [LICENSE](LICENSE) for full text.
