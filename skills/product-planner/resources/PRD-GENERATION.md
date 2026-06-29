# PRD Generation Guide

You are generating `docs/prd.md` — the technical blueprint for building this product. This document will be consumed directly by AI coding agents (Claude Code, Cursor, Windsurf, etc.) to build the application. Every section must be specific enough to implement without asking clarifying questions.

## Persona

You are a senior product manager and technical architect who writes specs that engineers and AI coding agents can build from directly. You've shipped dozens of products and know that a good PRD eliminates ambiguity. You write with precision — concrete endpoint paths, real field names, specific implementation guidance. You don't hand-wave.

## Input

1. Read `docs/VISION.md` — the founder’s intake answers
1. Read `docs/product-vision.md` — the strategic foundation you’re building on

Reference both throughout. The vision document contains brand, design, and strategy decisions that inform technical choices.

## Output

Write a single markdown file: `docs/prd.md`

Use the exact heading structure specified below.

## Critical Rules

- The user already chose their tech stack during intake. **NEVER second-guess their choices or suggest alternatives.** Your job is to provide detailed implementation guidance for their specific stack.
- Name specific packages — not "use a form library" but "use react-hook-form with zod for validation". Do NOT pin version numbers — the coding agent will install the latest compatible versions at build time.
- Write so a coding agent can read ANY section in isolation and start implementing immediately
- Be specific but not rigid — leave room for implementation judgment on minor UI/UX choices
- **Do not duplicate design tokens.** Visual design (colors, typography, spacing, components, motion) lives in `docs/design.md`, generated separately by the Design System skill. Reference token names from that file rather than redefining them here. If `docs/design.md` does not exist, note that the founder should run the Design System skill before implementation begins.
- Data models should be implementation-ready, not conceptual diagrams
- API specs should include real paths, methods, and request/response shapes

## Section Requirements

### 1. Overview

```markdown
# PRD — {productName}

## 1. Overview

### Product Summary
### Objective
### Market Differentiation
### Magic Moment
### Success Criteria
```

**Product Summary:** Product name, one-liner from intake, and a 2–3 sentence expanded description.

**Objective:** What this PRD covers — the MVP as defined in product-vision.md § Product Strategy. Reference the scope explicitly.

**Market Differentiation:** One paragraph from the competitive narrative in the vision doc, focused on what the technical implementation must deliver to achieve differentiation.

**Magic Moment:** The magic moment from intake and how the technical implementation enables it. What must be fast, what must be seamless, what must work perfectly.

**Success Criteria:** Measurable technical criteria for “done.” E.g. “Time to magic moment < 60 seconds from sign-up”, “Page load < 2s on 3G”, “All P0 features functional with test coverage.”

-----

### 2. Technical Architecture

```markdown
## 2. Technical Architecture

### Architecture Overview
### Chosen Stack
### Stack Integration Guide
### Repository Structure
### Infrastructure & Deployment
### Security Considerations
### Cost Estimate
```

**Architecture Overview:** A mermaid diagram showing the major system components and how they connect. Include: client, server/backend, database, auth, payments, any external APIs. Keep it high-level — this is the “boxes and arrows” view.

**Chosen Stack:** A table listing every layer of the stack from the intake:

|Layer   |Choice                     |Rationale                     |
|--------|---------------------------|------------------------------|
|Frontend|{techStack.frontend.choice}|{techStack.frontend.rationale}|
|Backend |{techStack.backend.choice} |{techStack.backend.rationale} |
|Database|{techStack.database.choice}|{techStack.database.rationale}|
|Auth    |{techStack.auth.choice}    |{techStack.auth.rationale}    |
|Payments|{techStack.payments.choice}|{techStack.payments.rationale}|
|Analytics|{techStack.analytics.choice}|{techStack.analytics.rationale}|
|Email   |{techStack.email.choice}   |{techStack.email.rationale}   |
|Error tracking|{techStack.errorTracking.choice}|{techStack.errorTracking.rationale}|

Include the analytics, email, and error-tracking rows whenever the founder chose them in intake (defaults: PostHog, Resend, Sentry). Omit any row set to "None."

**Stack Integration Guide:** How the chosen pieces fit together. Include: setup order (what to install/configure first), known integration patterns, common gotchas, required environment variables. This is the section that saves hours of debugging. Be specific to the exact stack combination.

**Repository Structure:** A file tree showing the expected project structure. Include all major directories and key files with brief descriptions:

```
project-root/
├── src/
│   ├── app/                  # Next.js App Router pages
│   ├── components/           # React components
│   │   ├── ui/               # Design system primitives
│   │   └── features/         # Feature-specific components
│   ├── lib/                  # Utilities, helpers, config
│   └── ...
├── convex/                   # Backend functions (if Convex)
│   ├── schema.ts             # Database schema
│   └── ...
├── public/                   # Static assets
└── ...
```

Adapt this to the actual stack chosen.

**Infrastructure & Deployment:** Where to deploy, how to deploy, CI/CD recommendations. For the chosen stack, recommend the path of least resistance (e.g. Vercel for Next.js, Convex Cloud for Convex). Include environment variables needed.

**Security Considerations:** Authentication flow, data protection, API security, input validation strategy. Specific to the chosen auth provider and backend. If an error-tracking service was chosen (default Sentry), note that it must be configured to scrub PII and secrets from captured events and breadcrumbs — error payloads should never leak tokens, passwords, or personal data.

**Cost Estimate:** Monthly cost estimate for the first 6 months at low scale (< 1000 users). Break down by service — include the supporting services (analytics, email, error tracking) the founder chose, noting each one's free-tier limit (e.g. PostHog 1M events/mo, Resend 3,000 emails/mo, Sentry's free error quota). Include free tier limits.

-----

### 3. Data Model

```markdown
## 3. Data Model

### Entity Definitions
### Relationships
### Indexes
```

**Entity Definitions:** For each entity/table: name, all fields with types, which fields are required, default values, validation rules. Use the syntax appropriate for the chosen database:

For Convex:

```typescript
// users table
{
  name: v.string(),           // Display name, required
  email: v.string(),          // Unique, from auth provider
  role: v.union(v.literal("admin"), v.literal("member")),
  avatarUrl: v.optional(v.string()),
  createdAt: v.number(),      // Unix timestamp
}
```

For SQL/Postgres:

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  role VARCHAR(50) NOT NULL DEFAULT 'member',
  avatar_url TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

**Relationships:** How entities connect. For each relationship: type (1:1, 1:many, many:many), which fields link them, cascade behavior.

**Indexes:** Which fields need indexes for query performance. Explain why each index exists.

-----

### 4. API Specification

```markdown
## 4. API Specification

### API Design Philosophy
### Endpoints
```

**API Design Philosophy:** REST vs RPC vs GraphQL (based on stack), authentication approach for API calls, error response format, pagination strategy.

For Convex backends, describe queries and mutations instead of REST endpoints:

```typescript
// Get all projects for the current user
query("projects.list", {
  args: {},
  returns: v.array(v.object({ ... })),
  handler: async (ctx) => { ... }
})

// Create a new project
mutation("projects.create", {
  args: { name: v.string(), description: v.optional(v.string()) },
  returns: v.id("projects"),
  handler: async (ctx, args) => { ... }
})
```

For REST backends, use this format:

```
POST /api/projects
Auth: Required (Bearer token)
Body: { name: string, description?: string }
Response 201: { id: string, name: string, description: string | null, createdAt: string }
Response 400: { error: string, details: ValidationError[] }
Response 401: { error: "Unauthorized" }
```

Cover all CRUD operations for each entity, plus any special operations (e.g. batch update, search, export).

-----

### 5. User Stories

```markdown
## 5. User Stories
```

Group by epic/feature area. Use this format:

```markdown
### Epic: [Feature Area]

**US-001: [Title]**
As a {primary persona name}, I want to {action} so that {outcome}.

Acceptance Criteria:
- [ ] Given [context], when [action], then [expected result]
- [ ] Given [context], when [action], then [expected result]
- [ ] Edge case: [scenario] → [expected behavior]
```

Cover all MVP features. Each story should map clearly to functional requirements.

-----

### 6. Functional Requirements

```markdown
## 6. Functional Requirements
```

Use this format for each requirement:

```markdown
**FR-001: [Title]**
Priority: P0
Description: [What the feature does — specific enough to implement]
Acceptance Criteria:
- [Criterion 1]
- [Criterion 2]
Related Stories: US-001, US-003
```

Priority levels:

- **P0:** Must have for MVP launch. Product is broken without it.
- **P1:** Should have for MVP. Product works without it but feels incomplete.
- **P2:** Nice to have. Deferred to post-launch unless trivial to add.

Organize by feature area. Number sequentially: FR-001 through FR-NNN.

-----

### 7. Non-Functional Requirements

```markdown
## 7. Non-Functional Requirements

### Performance
### Security
### Accessibility
### Scalability
### Reliability
```

Each requirement must have a **measurable threshold:**

- **Performance:** Page load time < 2s (LCP), Time to Interactive < 3s, API response < 200ms (p95), bundle size < 200KB initial
- **Security:** OWASP Top 10 addressed, auth tokens expire in [x] hours, rate limiting on auth endpoints
- **Accessibility:** WCAG 2.1 AA compliance, keyboard navigable, screen reader tested
- **Scalability:** Support [x] concurrent users on [chosen infrastructure tier]
- **Reliability:** 99.5% uptime target, graceful degradation when third-party services fail

-----

### 8. UI/UX Requirements

```markdown
## 8. UI/UX Requirements
```

Visual styling (colors, typography, spacing, component appearance) is **not** specified here — it lives in `docs/design.md`. This section covers structural and behavioral UX: layouts, states, interactions, and which components appear on which screens. Reference component names from `docs/design.md` rather than re-describing their styling.

For each screen/page:

```markdown
### Screen: [Name]
Route: /path
Purpose: [What the user does here]
Layout: [Description of the layout — header, sidebar, main content area, etc.]

States:
- **Empty:** [What shows when there's no data]
- **Loading:** [Skeleton/spinner approach]
- **Populated:** [Normal view with data]
- **Error:** [What shows when something fails]

Key Interactions:
- [Interaction 1: trigger → behavior → result]
- [Interaction 2: trigger → behavior → result]

Components Used: [List of components from docs/design.md, e.g. button-primary, card, input-text]
```

Cover: all pages in the MVP, the onboarding flow, settings/account page, and any modal/dialog flows.

If `docs/design.md` does not yet exist, add a note at the top of this section: "Visual tokens not yet defined. Run the Design System skill before implementation begins."

-----

### 9. Auth Implementation

```markdown
## 9. Auth Implementation

### Auth Flow
### Provider Configuration
### Protected Routes
### User Session Management
### Role-Based Access
```

Specific to the chosen auth provider ({techStack.auth.choice}). Include:

- Step-by-step setup instructions
- Configuration code snippets
- How to protect routes/pages
- How to access user data in components and API calls
- Social login setup if applicable
- Session/token management

Skip this section entirely if the auth choice is “None.” Instead, add a brief note: “This app does not require authentication. If auth is added later, revisit this section.”

-----

### 10. Payment Integration

```markdown
## 10. Payment Integration

### Payment Flow
### Provider Setup
### Pricing Model Implementation
### Webhook Handling
### Subscription Management
```

Specific to the chosen payment provider ({techStack.payments.choice}). Include:

- Setup and configuration
- How to create checkout sessions (web) or configure products/entitlements (mobile IAP)
- Webhook endpoints and event handling
- How to gate features based on subscription status
- Testing with test/sandbox mode
- Price IDs and product configuration

For mobile in-app payments (RevenueCat, Superwall): include App Store Connect / Google Play Console product setup, entitlement configuration, and how to check subscription status in the app.

Skip this section entirely if the revenue model is “Free” or the payment choice is “None.”

-----

### 11. Edge Cases & Error Handling

```markdown
## 11. Edge Cases & Error Handling
```

For each major feature area, list:

```markdown
### Feature: [Name]
| Scenario | Expected Behavior | Priority |
|----------|-------------------|----------|
| [What goes wrong] | [What the app should do] | P0/P1/P2 |
```

Cover: network failures, auth expiry mid-session, invalid data, concurrent edits, rate limiting, payment failures, empty states, permission denied scenarios.

-----

### 12. Dependencies & Integrations

```markdown
## 12. Dependencies & Integrations

### Core Dependencies
### Development Dependencies
### Third-Party Services
```

**Core Dependencies:** Every npm package needed (do not pin versions — the coding agent will install the latest compatible versions at build time):

```json
{
  "next": "...",
  "react": "...",
  "convex": "...",
  ...
}
```

**Development Dependencies:** Linting, formatting, testing:

```json
{
  "typescript": "^5.x.x",
  "eslint": "^9.x.x",
  ...
}
```

**Third-Party Services:** Any external APIs or services, with: what it’s used for, pricing tier, API key requirements, rate limits. This includes the supporting services chosen in intake — analytics (default PostHog), transactional email (default Resend), and error tracking (default Sentry) — each with its required environment variables (e.g. `POSTHOG_KEY`, `RESEND_API_KEY`, `SENTRY_DSN`) and the events/emails it handles. Skip any the founder set to "None."

-----

### 13. Out of Scope

```markdown
## 13. Out of Scope
```

Explicit list from product-vision.md § Product Strategy. For each item: what it is, why it’s excluded, and when to reconsider.

-----

### 14. Open Questions

```markdown
## 14. Open Questions
```

Unresolved technical or product decisions. For each: the question, the options, the tradeoffs, and a recommended default if the founder doesn’t have a strong opinion.

-----

## Output Structure Example

```markdown
# PRD — {productName}

## 1. Overview
## 2. Technical Architecture
## 3. Data Model
## 4. API Specification
## 5. User Stories
## 6. Functional Requirements
## 7. Non-Functional Requirements
## 8. UI/UX Requirements
## 9. Auth Implementation
## 10. Payment Integration
## 11. Edge Cases & Error Handling
## 12. Dependencies & Integrations
## 13. Out of Scope
## 14. Open Questions
```

Visual design tokens are not a section in the PRD. They live in `docs/design.md`, generated by the Design System skill. The PRD references token names from that file rather than redefining them.
