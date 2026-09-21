# SkillBridge

**SIH26044** · Portal for Academia-Industry Collaboration for Skill Mapping, Internships and Placement
*Sponsored by: Ministry of Ayush, All India Institute of Ayurveda (AIIA)*

🔗 **Live demo:** [internal-kohl.vercel.app](https://internal-kohl.vercel.app)

---

## The Problem

There is no single, trusted, skill-based platform connecting a specific institution's students to verified industry opportunities, with institutional oversight built in.

| Who | What's broken today |
|---|---|
| **Students** | Don't know which skills employers actually want for their target career. Internship/job search is scattered across WhatsApp groups, notice boards, and generic sites with no institutional trust layer. |
| **Industries / Companies** | Struggle to find candidates with the right skill sets quickly, and have no direct, verified channel into a specific institution's student pool. |
| **Institutions (Placement Cells / TPOs)** | Manage this entire process manually — spreadsheets, email chains, ad-hoc verification — with no single source of truth or audit trail. |
| **Academicians** | Limited visibility into current industry needs, so curriculum can drift out of sync (official PS's stated concern — see [Scope](#scope--roadmap) below). |

## Our Solution

A web platform with **three real, working roles**, each backed by a real database and real authentication:

- **Student** — builds a profile with real skills, browses institution-approved listings ranked by genuine skill overlap, applies, and tracks application status.
- **Company** — posts internship/job listings with required skills, views applicants ranked by real skill match, manages listings.
- **Institution / TPO** — reviews and approves or rejects every company listing before it reaches students, with visibility into registered students and all listings.

## What Makes SkillBridge Different

- **Real skill-tag matching** — an honest overlap count (e.g. "3 of 4 skills match"), not a fabricated AI score. Listings are sorted best-match-first per student.
- **Institutional approval workflow** — no listing reaches a student until the Institution/TPO approves it. A genuine trust layer generic platforms don't have.
- **Single source of truth per institution** — registered students, live listings, and applications are all visible to the Institution in one dashboard.
- **Discipline-agnostic architecture** — the same tables and logic work identically for any sector, including Ayush institutions (Ayurveda, Yoga, Unani, Siddha, Homeopathy programs), directly relevant to the sponsoring ministry.
- **Built cost-consciously** — cloud-native infrastructure (Supabase + Vercel), realistic for a college or ministry to actually pilot.

| Platform | What it's missing vs. SkillBridge |
|---|---|
| LinkedIn | Generic global network — no institution concept, no TPO approval workflow. |
| Naukri / Internshala | Open job aggregators — not tied to a verified student pool, no institutional oversight. |
| AICTE Internship Portal | Generic listing aggregator — not skill-mapped per student, no per-institution analytics. |
| Excel / WhatsApp (status quo) | No tracking, no matching logic, no audit trail, breaks down at scale. |

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend framework | React 19 + Vite + TypeScript |
| Styling | Tailwind CSS |
| Animation | Motion (successor to Framer Motion) |
| Icons | lucide-react |
| Backend / Database | Supabase (hosted PostgreSQL + Auth + auto-generated APIs) |
| Authentication | Supabase Auth (email + password) |
| API layer | `@supabase/supabase-js` — queried directly from the frontend, no hand-written backend server |
| Hosting / Deployment | Vercel (auto-deploys from `main`) |
| Version control | Git + GitHub |
| AI-assisted development | Google AI Studio and v0.dev for UI generation; Claude (Anthropic) for architecture guidance, debugging, and database-integration code |

## Database Design

Six real PostgreSQL tables, all protected by **Row Level Security (RLS)** — access control enforced by the database itself, not just frontend code:

- **`profiles`** — every user's base record: id, role (student/company/admin), full name
- **`student_profiles`** — skills (array), major, university, graduation year (1:1 with a profile)
- **`companies`** — company name, description, website, headquarters, industry, tech stack
- **`colleges`** — institution name, campus, contact email, office phone, linked to its admin/TPO account
- **`listings`** — job/internship postings: title, required skills, location, work mode, description, status (pending/approved/rejected)
- **`applications`** — links a student to a listing with a status (applied/shortlisted/selected/rejected); unique constraint prevents duplicate applications

## Getting Started

```bash
git clone https://github.com/singhmayank091git-ai/SkillBridge.git
cd SkillBridge
npm install
npm run dev       # start local dev server
```

No `.env` setup is required to run this locally. The Supabase project URL and anon/publishable key are set directly in `src/lib/supabaseClient.ts` and work out of the box — see [Security Note](#security-note) for why that's intentional and safe.

Other scripts:

```bash
npm run build     # production build
npm run preview   # preview production build locally
npm run lint      # type-check with tsc
```

## Scope & Roadmap

The official problem statement describes a larger 4-role system (Student, Academician, Industry, Institution) with skill assessments and digital portfolios. Given our timeline, we deliberately scoped a **focused 3-role MVP** we could build fully and honestly. The Academician role, skill-assessment quizzes, and digital portfolios are our documented **Phase 2** — a conscious, disclosed scoping decision, not something hidden.

**Known limitation:** any institution account can currently approve any company's listing, rather than only listings relevant to its own students. This is a disclosed next step (adding an institution-company relationship), not a fundamental design flaw.

## Security Note

The Supabase URL and anon/publishable key are used directly in frontend code. This is safe by design:

- The anon/publishable key is meant to sit in frontend code that runs in users' browsers — it is not a secret.
- Real access control comes from **Row Level Security**, not from hiding this key — even with the anon key, RLS policies determine what can actually be read or written.
- The `service_role`/secret key is never used anywhere in this project.
- Current RLS policies are intentionally permissive for demo purposes and are documented as needing per-role tightening before any production launch.

## How We Built This

1. Selected SIH26044, read the official problem statement, researched the sponsoring ministry's context.
2. Wrote planning documents first — PRD, architecture (schema + stack), team working agreement, phased timeline — before writing any code.
3. Built the UI first with placeholder data, using AI-assisted tools to generate every screen across all 3 roles against one locked design system.
4. Caught and removed fabricated content from early UI drafts (invented statistics, an institution name used without basis) — replaced with honest content as a firm team rule.
5. Set up the real backend — created the Supabase project and all 6 tables with proper relationships and RLS policies.
6. Wired real authentication, replacing the placeholder instant-login with real Supabase sign-up/sign-in — debugging a real chain of issues (a missing file, a JSON syntax error, a component prop mismatch, a silent validation bug, an RLS policy issue).
7. Connected every screen to the real database, one at a time, replacing every hardcoded array with a real Supabase query.
8. Added real skill-matching logic — an honest overlap count used to rank both listings (for students) and applicants (for companies).
9. Removed the temporary role-switcher used during early UI development — real accounts now determine dashboard access automatically.
10. Final cleanup pass removing all remaining fabricated UI content across every screen.

## License

This project is licensed under the MIT License — see [LICENSE](./LICENSE) for details.
