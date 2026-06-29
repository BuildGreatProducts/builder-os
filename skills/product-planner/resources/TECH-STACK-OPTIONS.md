# Tech Stack Options

Default comparison data for the Product Planner tech stack questions. Use these as a baseline and adapt recommendations based on the specific product's needs. The comparison format and pros/cons should be adjusted to reflect how each option fits the founder's particular product.

**This list is a starting point, not a boundary.** The ecosystem moves fast and no static list stays current. Research beyond it whenever:

- The founder names a tool that isn't listed here — research it and give a fair assessment rather than steering them back to this list
- The product has unusual needs this list doesn't serve well (e.g. heavy ML inference, hardware integration, blockchain, real-time video, HIPAA compliance)
- You're unsure whether an option here is still the current best-in-class — verify with a web search before presenting it as the recommendation
- The founder asks "what else is out there?" — do a fresh search of the category, not just a recital of this file

When you research, evaluate candidates with the same lens used here: maturity and community size, AI-coding-tool familiarity, integration with the rest of the chosen stack, pricing and free tier, and operational burden for a small team. Present researched options in the same comparison format so the founder can weigh them against the defaults.

-----

## Frontend Frameworks

### Web Apps

**Next.js** — React framework with server-side rendering, file-based routing, and excellent deployment options.

- ✓ Largest React ecosystem, huge community, extensive documentation
- ✓ App Router with server components for performance
- ✓ Excellent integration with Vercel, Convex, Clerk, and most services
- ✓ Best-supported by AI coding tools (most training data)
- ✗ Can be complex — many ways to do things (server vs client components)
- ✗ Opinionated about project structure
- **Best for:** Most web apps. Default recommendation unless there's a specific reason not to.

**Vite + React (SPA)** — Single-page React app with Vite's fast build tooling, no server-rendering framework.

- ✓ Simplest mental model — it's just React, no server/client component split
- ✓ Extremely fast dev server and builds
- ✓ Pairs naturally with backend-as-a-service platforms (Convex, Supabase, Firebase) that handle the server side
- ✓ Deploys anywhere static files can be hosted
- ✗ No server-side rendering — weaker SEO for content-heavy public pages
- ✗ Routing, data fetching, and head management are assembled from libraries rather than built in
- **Best for:** App-like products behind a login (dashboards, tools, SaaS apps) where SEO doesn't matter and a BaaS handles the backend.

**React Router (formerly Remix)** — Full-stack React framework focused on web standards and progressive enhancement; Remix merged into React Router v7.

- ✓ Excellent form handling and data loading patterns (loaders + actions)
- ✓ Progressive enhancement — works without JavaScript
- ✓ Simpler mental model than Next.js
- ✗ Smaller ecosystem than Next.js
- ✗ Less AI coding tool familiarity
- **Best for:** Form-heavy apps, content-heavy sites, apps that need to work without JS.

**SvelteKit** — Svelte framework with file-based routing and server-side rendering.

- ✓ Significantly less boilerplate than React
- ✓ Excellent performance — smaller bundle sizes
- ✓ Built-in state management (no Redux/Zustand needed)
- ✗ Smaller ecosystem and community than React
- ✗ Fewer component libraries available
- ✗ Less AI coding tool support
- **Best for:** Performance-critical apps, developers who prefer less boilerplate.

**Nuxt** — Vue framework with server-side rendering, file-based routing, and a strong module ecosystem.

- ✓ Vue's gentle learning curve with batteries-included conventions
- ✓ Strong module ecosystem (auth, content, images) with minimal wiring
- ✓ Good performance defaults and hybrid rendering options
- ✗ Vue ecosystem is smaller than React's — fewer component libraries and integrations
- ✗ Less AI coding tool familiarity than Next.js
- **Best for:** Teams that know or prefer Vue. Don't switch to React just for the ecosystem if Vue expertise already exists.

**Astro** — Content-first framework that ships zero JavaScript by default, with islands of interactivity in any UI framework.

- ✓ Outstanding performance for content sites — HTML-first output
- ✓ Use React, Svelte, or Vue components only where interactivity is needed
- ✓ First-class markdown/MDX content handling
- ✗ Not designed for highly interactive app experiences — islands have limits
- ✗ App-like features (auth flows, dashboards) require more assembly
- **Best for:** Marketing sites, blogs, docs, and content-heavy products where speed and SEO are the priority. Often paired with a separate app framework for the product itself.

### Mobile Apps

**Expo / React Native** — Cross-platform mobile framework with managed workflow.

- ✓ Write once, run on iOS and Android
- ✓ Expo managed workflow eliminates native build complexity
- ✓ React knowledge transfers directly
- ✓ Over-the-air updates
- ✗ Performance can lag behind native for graphics-heavy apps
- ✗ Some native APIs require custom native modules
- **Best for:** Most mobile apps. Default recommendation for mobile.

**Flutter** — Google's cross-platform UI toolkit using Dart.

- ✓ Excellent performance — compiles to native
- ✓ Beautiful, customizable UI components
- ✓ Single codebase for iOS, Android, web, desktop
- ✗ Dart is a separate language to learn
- ✗ Less ecosystem integration with JS/TS backends
- ✗ Less AI coding tool support than React Native
- **Best for:** Apps needing pixel-perfect custom UI or very high performance.

**SwiftUI (native iOS)** — Apple's declarative UI framework for iOS, iPadOS, watchOS, and macOS.

- ✓ Best possible iOS experience — native performance, platform conventions, immediate access to new Apple APIs
- ✓ Tight integration with Apple frameworks (HealthKit, ARKit, widgets, App Intents)
- ✓ Strong AI coding tool support for Swift
- ✗ iOS-only — an Android version means a second codebase
- ✗ Requires a Mac to build; TestFlight/App Store only distribution
- **Best for:** iOS-first products, apps leaning on Apple-specific capabilities, or founders who accept iOS-only for v1.

**Jetpack Compose (native Android)** — Google's declarative UI toolkit for native Android in Kotlin.

- ✓ Best possible Android experience — native performance and platform integration
- ✓ Kotlin is modern and pleasant; strong tooling in Android Studio
- ✗ Android-only — an iOS version means a second codebase
- ✗ Smaller indie/startup mindshare than cross-platform options
- **Best for:** Android-first products or markets where Android dominates.

### Desktop Apps

**Electron** — Build cross-platform desktop apps with Chromium and Node.js. Powers VS Code, Slack, Discord, Figma, and Notion.

- ✓ Most mature desktop framework — battle-tested at massive scale
- ✓ Full web technology stack (HTML, CSS, JS/TS) — no new language to learn
- ✓ Largest ecosystem of plugins, tools, and community resources
- ✓ Excellent AI coding tool support (most training data)
- ✗ Heavy memory footprint — each app bundles its own Chromium instance
- ✗ Large bundle sizes (100MB+ minimum)
- ✗ Can feel non-native on macOS — requires extra work to match platform conventions
- **Best for:** Most desktop apps. Default recommendation for desktop. Especially strong when the team already knows web technologies.

**Tauri** — Lightweight desktop framework using the OS's native webview and a Rust backend.

- ✓ Dramatically smaller bundles than Electron (often 5-10MB vs 100MB+)
- ✓ Lower memory usage — uses the OS webview instead of bundling Chromium
- ✓ Rust backend for performance-critical operations and system access
- ✓ Strong security model — fine-grained permission system for system APIs
- ✗ Younger ecosystem — fewer community resources and plugins than Electron
- ✗ Rust knowledge needed for backend plugins and system integrations
- ✗ OS webview inconsistencies can cause cross-platform rendering differences
- **Best for:** Desktop apps where bundle size and memory matter, or when deep system integration is needed. Good for developers comfortable with Rust.

**Flutter (Desktop)** — The same Flutter framework listed under Mobile, with support for macOS, Windows, and Linux.

- ✓ Single codebase across mobile, web, and desktop — true cross-platform
- ✓ Compiles to native — good performance without a webview
- ✓ Consistent UI across all platforms
- ✗ Desktop support is less mature than mobile — some platform APIs are missing
- ✗ Dart ecosystem is smaller than JS/TS for desktop-specific needs
- ✗ Apps don't follow native platform UI conventions by default
- **Best for:** Projects that need a single codebase across mobile AND desktop. Not recommended for desktop-only apps — Electron or Tauri are better choices there.

-----

## Backend

**Convex** — Reactive backend-as-a-service with built-in database, real-time sync, and TypeScript-native functions.

- ✓ Real-time data sync out of the box — no WebSocket setup
- ✓ Zero backend boilerplate — define functions, they just work
- ✓ Built-in auth, file storage, scheduling, search
- ✓ TypeScript end-to-end with full type safety
- ✓ Excellent DX for solo developers — fast iteration
- ✓ ACID transactions on the database
- ✗ Newer ecosystem — fewer community resources
- ✗ Vendor dependency — data lives on Convex Cloud
- ✗ Different mental model from traditional REST APIs
- **Best for:** Real-time and collaborative apps, solo developers, fast-moving MVPs in TypeScript.

**Supabase** — Open-source Firebase alternative built on PostgreSQL.

- ✓ PostgreSQL under the hood — full SQL power, relational data
- ✓ Real-time subscriptions, auth, storage, edge functions
- ✓ Open source — can self-host if needed
- ✓ Large and growing community
- ✗ More setup than Convex — manual schema migrations
- ✗ Real-time requires explicit subscription setup
- ✗ Edge functions are less integrated than Convex functions
- **Best for:** Products with complex relational data, teams that want SQL and open-source.

**Firebase** — Google's mature backend-as-a-service: Firestore, auth, storage, cloud functions, push notifications.

- ✓ Very mature — over a decade of production hardening, deep docs
- ✓ Best-in-class mobile SDKs and push notification support (FCM)
- ✓ Generous free tier; scales automatically
- ✓ Tight Google Cloud integration when you outgrow it
- ✗ Firestore's NoSQL model makes complex relational queries painful
- ✗ Vendor lock-in is real — migrating off Firestore is hard
- ✗ Pricing can spike with chatty read/write patterns
- **Best for:** Mobile-first products (especially with push notifications), or founders already in the Google ecosystem.

**Hono / lightweight TypeScript API** — Minimal, fast TypeScript web framework that runs on Node, Bun, and edge runtimes (Cloudflare Workers, Vercel).

- ✓ Tiny, fast, modern — Express-style routing without the legacy baggage
- ✓ Runs on edge runtimes for low latency worldwide
- ✓ Full control with far less boilerplate than Express
- ✗ You still assemble the rest: database client, auth, validation, deployment
- ✗ Smaller middleware ecosystem than Express
- **Best for:** Founders who want a real API server with full control but minimal framework weight; pairs well with Neon/Turso and Better Auth.

**Node.js + Express + PostgreSQL** — Traditional server setup with full control.

- ✓ Maximum flexibility — build exactly what you need
- ✓ Largest ecosystem of packages and middleware
- ✓ Full control over infrastructure and hosting
- ✗ Significant boilerplate — auth, validation, error handling, CORS, etc.
- ✗ You manage everything: database migrations, deployment, scaling
- ✗ Slower to iterate as a solo developer
- **Best for:** Experienced backend developers who want full control, or products with unusual requirements.

**FastAPI (Python)** — Modern Python API framework with automatic OpenAPI docs and async support.

- ✓ The natural choice when the product's core is Python (ML, data science, AI pipelines)
- ✓ Type-hint-driven validation and auto-generated API docs
- ✓ Huge Python ecosystem for ML/AI/data work
- ✗ Two-language stack if the frontend is TypeScript — duplicated types and tooling
- ✗ You assemble auth, ORM (SQLAlchemy/SQLModel), and deployment yourself
- **Best for:** Products whose differentiation is ML/AI/data processing in Python. Otherwise prefer a TypeScript-native option to keep one language end to end.

**Ruby on Rails** — The original batteries-included full-stack framework.

- ✓ Extremely productive conventions — auth, ORM, jobs, mailers all standard
- ✓ Mature ecosystem with decades of solved problems
- ✓ Hotwire/Turbo gives interactive UIs without a separate frontend framework
- ✗ Ruby talent and AI-tool familiarity are thinner than TypeScript's
- ✗ Separate language from a JS frontend if you go SPA
- **Best for:** Founders who know Rails, or server-rendered CRUD-heavy products where one framework doing everything is an advantage.

-----

## Database

**Convex Database** — Document-relational database built into the Convex platform.

- ✓ Automatic reactive queries — UI updates when data changes
- ✓ ACID transactions with optimistic concurrency
- ✓ Automatic indexing — define indexes in schema, they just work
- ✓ TypeScript schema validation built-in
- ✗ Only available with Convex backend
- ✗ Document-oriented — different from SQL thinking
- **Best for:** Any product using Convex backend. Use this — it's part of the package.

**Supabase Database (PostgreSQL)** — Managed PostgreSQL via the Supabase platform with a dashboard, auto-generated APIs, and real-time subscriptions.

- ✓ Full PostgreSQL — complex queries, joins, extensions, relational power
- ✓ Auto-generated REST and GraphQL APIs from your schema
- ✓ Real-time subscriptions built in
- ✓ Row Level Security for fine-grained access control
- ✓ Dashboard with table editor — visual schema management
- ✗ Only makes sense with Supabase backend
- ✗ Migrations still needed for production schema changes
- **Best for:** Supabase backends — use this, it's part of the package. Excellent for relational data.

**PostgreSQL (self-managed or generic host)** — The gold-standard open-source relational database.

- ✓ Rock-solid reliability and ACID compliance
- ✓ Full SQL power — complex queries, joins, aggregations
- ✓ Excellent for relational data with complex relationships
- ✓ Massive ecosystem of tools and extensions
- ✗ Requires migrations for schema changes
- ✗ No built-in real-time — need separate pub/sub
- **Best for:** Products with complex relational data on traditional backends.

**Neon (serverless PostgreSQL)** — Managed Postgres with serverless scaling, branching, and scale-to-zero.

- ✓ Real PostgreSQL with a generous free tier and scale-to-zero pricing
- ✓ Database branching — spin up a copy per environment or PR
- ✓ Works with any backend that speaks Postgres; great with serverless/edge functions
- ✗ Cold starts after scale-to-zero on the free tier
- ✗ It's just the database — auth, APIs, and storage live elsewhere
- **Best for:** Custom backends (Hono, Express, FastAPI) that want managed Postgres without running a server.

**Turso / SQLite (libSQL)** — SQLite-compatible database, either embedded locally or hosted at the edge.

- ✓ Tiny, fast, zero-ops — SQLite's simplicity with optional hosted replication
- ✓ Great fit for local-first and offline-capable apps
- ✓ Per-tenant database patterns are cheap (one DB per user/team)
- ✗ Not built for high-concurrency write-heavy workloads
- ✗ Smaller ecosystem than Postgres for tooling and extensions
- **Best for:** Local-first apps, desktop/mobile embedded storage, read-heavy edge apps, and per-tenant architectures.

**MongoDB (Atlas)** — The most widely used document database, managed via Atlas.

- ✓ Flexible document model — easy to evolve schemas early on
- ✓ Mature managed offering with search and vector search built in
- ✓ Huge ecosystem and driver support in every language
- ✗ Relational integrity and multi-document joins are weaker than SQL
- ✗ Easy to make data-modeling mistakes that hurt later
- **Best for:** Document-shaped data, teams already fluent in Mongo. For most new SaaS products, Postgres-family options are the safer default.

**None (local-only / no database)** — The app stores data on-device only (AsyncStorage, SQLite, UserDefaults, local files).

- ✓ Zero infrastructure — no backend costs, no latency
- ✓ Works offline by default
- ✓ Simpler architecture — no sync, no API calls
- ✗ Data is lost if the user deletes the app (unless backed up)
- ✗ No cross-device sync
- ✗ No server-side logic or shared data
- **Best for:** Mobile apps that are primarily tools (calculators, trackers, utilities), offline-first apps, or MVPs that don't need shared data. Consider adding a backend later if the product grows.

-----

## Auth Providers

**Convex Auth** — Native auth built into the Convex platform.

- ✓ Zero-config integration with Convex backend
- ✓ Supports email/password, OAuth providers, magic links
- ✓ User data lives in Convex — no external service calls
- ✗ Only works with Convex backend
- ✗ Fewer pre-built UI components than Clerk
- **Best for:** Convex backends where simplicity is priority.

**Clerk** — Drop-in auth with pre-built UI components.

- ✓ Beautiful, pre-built sign-in/sign-up components
- ✓ Social login, MFA, organization management out of the box
- ✓ Excellent React/Next.js integration
- ✓ Generous free tier (10,000 MAUs)
- ✗ External service dependency
- ✗ Monthly cost at scale
- **Best for:** Products that want polished auth UI fast. Works with any backend.

**Better Auth** — Open-source, framework-agnostic TypeScript auth library you own and host.

- ✓ Open source and self-hosted — user data stays in your database, no per-MAU fees
- ✓ Comprehensive: email/password, OAuth, magic links, 2FA, organizations, plugins
- ✓ Works across Next.js, Hono, Express, and most TS backends
- ✗ You build the UI and own the security configuration
- ✗ Younger project — smaller community than Auth.js or Clerk
- **Best for:** TypeScript founders who want full ownership of auth and user data without vendor fees.

**Auth.js (NextAuth)** — Open-source auth for Next.js.

- ✓ Open source — no vendor dependency
- ✓ Supports many OAuth providers
- ✓ Database adapters for most databases
- ✗ More setup and configuration than Clerk
- ✗ Less polished UI — you build your own forms
- ✗ Session management can be tricky
- **Best for:** Next.js developers who want open-source auth with full control.

**Supabase Auth** — Auth built into the Supabase platform.

- ✓ Integrated with Supabase — Row Level Security uses auth
- ✓ Email/password, magic links, OAuth providers
- ✓ Free with Supabase
- ✗ Only makes sense with Supabase backend
- ✗ Less polished than Clerk's UI components
- **Best for:** Supabase backends — use this, it's part of the package.

**Firebase Auth** — Auth built into the Firebase platform.

- ✓ Mature, battle-tested, generous free tier
- ✓ Excellent mobile SDK support — phone auth, anonymous auth, social providers
- ✓ Integrates with Firestore security rules
- ✗ Only makes sense with Firebase backend
- ✗ Customizing flows beyond the defaults gets awkward
- **Best for:** Firebase backends and mobile apps needing phone or anonymous auth.

**WorkOS / Auth0 (enterprise-grade)** — Hosted identity platforms with enterprise SSO (SAML, OIDC), directory sync, and compliance features.

- ✓ The fastest route to "Sign in with [corporate SSO]" — required for selling to enterprises
- ✓ Compliance, audit logs, and directory sync handled for you
- ✗ Overkill for consumer or early-stage products
- ✗ Cost scales steeply (Auth0 especially)
- **Best for:** B2B products whose buyers will demand SSO. Usually a later addition, not an MVP choice — but plan for it if enterprise is the explicit go-to-market.

**None (no auth needed)** — The app doesn't require user accounts or sign-in.

- ✓ Simpler UX — no sign-up friction, instant access
- ✓ Less infrastructure to manage
- ✓ Better for tools, utilities, and single-player experiences
- ✗ No personalization or saved preferences across devices
- ✗ Can't gate features behind subscription tiers (without device-level checks)
- **Best for:** Mobile utility apps, offline tools, calculators, single-player experiences, or MVPs testing core value before adding accounts. Can always add auth later.

-----

## Payment Providers

### Web / SaaS Payments

**Polar** — Developer-first payment platform for SaaS and digital products.

- ✓ Built specifically for developers and SaaS products
- ✓ Handles subscriptions, one-time payments, and licensing
- ✓ Merchant of record — handles global tax compliance for you
- ✓ Excellent API and webhook support; built-in customer portal
- ✗ Newer platform — smaller community than Stripe
- ✗ Less suitable for physical goods or complex billing
- **Best for:** SaaS products, digital products, developer tools sold by small teams.

**Stripe** — The most flexible and widely-used payment platform.

- ✓ Supports virtually any payment model
- ✓ Largest ecosystem — extensive documentation, libraries, integrations
- ✓ Stripe Checkout for quick integration
- ✓ Billing portal, invoicing, subscription management
- ✗ Complex — many concepts to learn (Products, Prices, Subscriptions, etc.)
- ✗ You are the merchant of record — tax registration and compliance is on you (or Stripe Tax at extra cost)
- **Best for:** Products with complex billing needs, marketplaces, or maximum flexibility.

**Lemon Squeezy** — Merchant of record for digital products (acquired by Stripe).

- ✓ Handles global tax compliance — they're the merchant of record
- ✓ Simple setup for subscriptions and one-time payments
- ✓ Built-in affiliate program
- ✗ Higher fees than raw Stripe (they take on tax liability)
- ✗ Less flexible than Stripe for complex billing
- **Best for:** Solo founders selling internationally who don't want to deal with tax compliance.

**Paddle** — Established merchant of record for SaaS billing.

- ✓ Merchant of record — global tax, compliance, and chargebacks handled
- ✓ Mature platform with strong subscription and invoicing features
- ✓ Good for selling to both consumers and businesses internationally
- ✗ Approval process before you can sell — not instant setup
- ✗ Checkout customization is more limited than Stripe
- **Best for:** SaaS businesses that want the merchant-of-record model from an established provider.

### Mobile In-App Payments

For mobile apps distributed through the App Store or Google Play, in-app purchases (IAP) are often required by platform policies. These tools manage subscriptions and purchases through the native store billing systems.

**RevenueCat** — Cross-platform in-app subscription management.

- ✓ Abstracts Apple and Google billing APIs into one SDK
- ✓ Handles receipt validation, entitlements, and subscription status server-side
- ✓ Excellent dashboard with analytics, cohorts, and churn tracking
- ✓ Generous free tier — free up to $2,500/month in tracked revenue
- ✓ Works with React Native/Expo, Flutter, Swift, Kotlin
- ✓ Webhook support for backend integration
- ✗ Another dependency and point of failure in the payment flow
- ✗ Paid tiers add up as revenue grows (1% of tracked revenue after free tier)
- **Best for:** Any mobile app with subscriptions or one-time IAP. Default recommendation for mobile payments.

**Superwall** — Paywall A/B testing and management platform.

- ✓ Build and deploy paywalls remotely — no app update needed to change pricing UI
- ✓ Built-in A/B testing for paywall designs, pricing, and placement
- ✓ Pre-built paywall templates that convert well
- ✓ Works with RevenueCat or handles purchases directly via StoreKit/Billing
- ✗ Focused on paywall presentation — not a full subscription backend (pair with RevenueCat for that)
- ✗ Free tier is limited — paid plans required for A/B testing
- **Best for:** Mobile apps that want to optimize subscription conversion through paywall experimentation. Best paired with RevenueCat.

**Adapty** — RevenueCat alternative with built-in paywall builder and A/B testing.

- ✓ Subscription infrastructure plus no-code paywall builder in one SDK
- ✓ A/B testing and remote paywall config included on lower tiers
- ✓ Strong analytics on funnels and cohorts
- ✗ Smaller community and integration ecosystem than RevenueCat
- ✗ Pricing scales with revenue like RevenueCat
- **Best for:** Mobile apps that want subscriptions and paywall experimentation from a single vendor.

**None (no payments needed)** — The app is free with no monetization, or monetization will be added later.

- ✓ Ship faster — no payment integration complexity
- ✓ No App Store commission considerations
- ✓ Focus entirely on core product value
- ✗ No revenue from day one
- ✗ Adding payments later requires an app update and review
- **Best for:** Free utility apps, apps exploring product-market fit before monetizing, or apps monetized through other channels (ads, enterprise contracts, etc.).

-----

## Supporting Services

Three services most products want by launch, even though they aren't part of the core build: product analytics (so you can see what users do), transactional email (password resets, magic links, receipts, notifications), and error tracking (so you find crashes before users report them). Recommend the defaults below unless the product clearly needs something else, and let the founder skip any they don't want yet — each can be added later.

### Analytics

**PostHog** — Open-source product analytics suite with events, funnels, session replay, feature flags, and A/B testing.

- ✓ All-in-one: analytics, session replay, feature flags, and experiments in one tool
- ✓ Generous free tier (1M events/month); open-source and self-hostable
- ✓ Autocapture plus custom events — start getting data with minimal instrumentation
- ✓ First-class SDKs for web, React Native, and most backends
- ✗ Broad surface area can feel heavy if you only want basic metrics
- ✗ Self-hosting is real ops work — most teams use the cloud
- **Best for:** Most products. Default recommendation — the free tier and feature-flag/replay bundle are hard to beat for a solo founder.

**Mixpanel** — Mature, focused product analytics built around events, funnels, and retention.

- ✓ Best-in-class funnel, cohort, and retention analysis
- ✓ Polished, fast dashboards non-technical teammates can use
- ✓ Generous free tier (up to ~1M monthly events)
- ✗ Analytics only — no session replay, flags, or experiments (separate tools needed)
- ✗ Event taxonomy needs deliberate planning to stay clean
- **Best for:** Teams that want deep, dedicated funnel/retention analytics and don't need PostHog's wider toolkit.

### Email

**Resend** — Developer-first transactional email API from the team behind React Email.

- ✓ Clean, modern API — sending email takes minutes
- ✓ React Email for type-safe, component-based templates
- ✓ Generous free tier (3,000 emails/month); simple domain + DNS setup
- ✓ Great fit with Next.js / TypeScript stacks
- ✗ Younger than SendGrid/Postmark — smaller track record at huge scale
- ✗ Transactional-focused — not a marketing automation platform
- **Best for:** Most products' transactional email (auth, receipts, notifications). Default recommendation, especially on a TypeScript stack.

**Loops** — Email platform combining transactional sending with lightweight marketing automation.

- ✓ Transactional email plus drip campaigns, onboarding sequences, and broadcasts in one place
- ✓ Visual editor and automation loops aimed at SaaS lifecycle email
- ✓ Simple API and good defaults for founders who want marketing + transactional together
- ✗ Less of a pure-developer primitive than Resend
- ✗ Younger platform; marketing features are simpler than dedicated tools like Customer.io
- **Best for:** Founders who want both transactional and lifecycle/marketing email from one tool without wiring up two services.

### Error Tracking

**Sentry** — The standard for application error and performance monitoring.

- ✓ Captures exceptions with stack traces, breadcrumbs, and release/context tagging
- ✓ SDKs for every major frontend, backend, and mobile framework
- ✓ Performance monitoring and session tracking alongside errors
- ✓ Free tier covers a solo founder's early volume
- ✗ Quota management needs attention as traffic grows
- ✗ Full feature set is more than a tiny app strictly needs (but the defaults are sensible)
- **Best for:** Every product that ships to real users. Default recommendation — find crashes before your users tell you about them.

**None (add later)** — Skip a supporting service for the MVP.

- ✓ Fewer integrations and accounts to set up before launch
- ✗ Flying blind: no usage data, no error alerts, or manual email wiring later
- **Best for:** The earliest throwaway prototypes. For anything real, analytics and error tracking are cheap insurance — recommend at least PostHog and Sentry.

-----

## Researching Beyond This List

When a category here doesn't fit the product — or the founder asks for alternatives — run a fresh comparison:

1. **Search the current landscape** (e.g. "best [category] for [product type] [current year]") rather than relying on this file or training data alone.
2. **Shortlist 2–3 candidates** that fit the product's platform, the rest of the chosen stack, and the founder's experience level.
3. **Present them in the standard comparison format** (one-line description, ✓ pros, ✗ cons, "Best for") alongside any relevant defaults from this list, and mark a clear recommendation.
4. **Note verification dates for volatile facts** — pricing, free tiers, and acquisition status change; say "as of [date]" when quoting them.

Categories intentionally not covered here (research when relevant): hosting/deployment platforms, AI/LLM APIs, vector databases, search services, CMS, and file storage. The PRD generation step picks sensible defaults for these based on the chosen core stack. (Analytics, email, and error tracking now have first-class recommendations under Supporting Services above.)
