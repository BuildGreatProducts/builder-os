# Tech Stack Options

Default comparison data for the Product Planner tech stack questions. Use these as a baseline and adapt recommendations based on the specific product’s needs. The comparison format and pros/cons should be adjusted to reflect how each option fits the founder’s particular product.

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
- **Best for:** Most web apps. Default recommendation unless there’s a specific reason not to.

**Remix** — Full-stack React framework focused on web standards and progressive enhancement.

- ✓ Excellent form handling and data loading patterns
- ✓ Progressive enhancement — works without JavaScript
- ✓ Simpler mental model than Next.js (loaders + actions)
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

### Mobile Apps

**Expo / React Native** — Cross-platform mobile framework with managed workflow.

- ✓ Write once, run on iOS and Android
- ✓ Expo managed workflow eliminates native build complexity
- ✓ React knowledge transfers directly
- ✓ Over-the-air updates
- ✗ Performance can lag behind native for graphics-heavy apps
- ✗ Some native APIs require ejecting from managed workflow
- **Best for:** Most mobile apps. Default recommendation for mobile.

**Flutter** — Google's cross-platform UI toolkit using Dart.

- ✓ Excellent performance — compiles to native
- ✓ Beautiful, customizable UI components
- ✓ Single codebase for iOS, Android, web, desktop
- ✗ Dart is a separate language to learn
- ✗ Less ecosystem integration with JS/TS backends
- ✗ Less AI coding tool support than React Native
- **Best for:** Apps needing pixel-perfect custom UI or very high performance.

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
- **Best for:** Most products, especially real-time apps, solo developers, MVPs. Default recommendation.

**Supabase** — Open-source Firebase alternative built on PostgreSQL.

- ✓ PostgreSQL under the hood — full SQL power, relational data
- ✓ Real-time subscriptions, auth, storage, edge functions
- ✓ Open source — can self-host if needed
- ✓ Large and growing community
- ✗ More setup than Convex — manual schema migrations
- ✗ Real-time requires explicit subscription setup
- ✗ Edge functions are less integrated than Convex functions
- **Best for:** Products with complex relational data, teams that want SQL and open-source.

**Node.js + Express + PostgreSQL** — Traditional server setup with full control.

- ✓ Maximum flexibility — build exactly what you need
- ✓ Largest ecosystem of packages and middleware
- ✓ Full control over infrastructure and hosting
- ✗ Significant boilerplate — auth, validation, error handling, CORS, etc.
- ✗ You manage everything: database migrations, deployment, scaling
- ✗ Slower to iterate as a solo developer
- **Best for:** Experienced backend developers who want full control, or products with unusual requirements.

-----

## Database

**Convex Database** — Document-relational database built into the Convex platform.

- ✓ Automatic reactive queries — UI updates when data changes
- ✓ ACID transactions with optimistic concurrency
- ✓ Automatic indexing — define indexes in schema, they just work
- ✓ TypeScript schema validation built-in
- ✗ Only available with Convex backend
- ✗ Document-oriented — different from SQL thinking
- **Best for:** Any product using Convex backend. Use this — it’s part of the package.

**PostgreSQL** — The gold-standard open-source relational database.

- ✓ Rock-solid reliability and ACID compliance
- ✓ Full SQL power — complex queries, joins, aggregations
- ✓ Excellent for relational data with complex relationships
- ✓ Massive ecosystem of tools and extensions
- ✗ Requires migrations for schema changes
- ✗ No built-in real-time — need separate pub/sub
- **Best for:** Products with complex relational data. Pairs with Supabase or traditional backends.

**Supabase Database (PostgreSQL)** — Managed PostgreSQL via the Supabase platform with a dashboard, auto-generated APIs, and real-time subscriptions.

- ✓ Full PostgreSQL — complex queries, joins, extensions, relational power
- ✓ Auto-generated REST and GraphQL APIs from your schema
- ✓ Real-time subscriptions built in
- ✓ Row Level Security for fine-grained access control
- ✓ Dashboard with table editor — visual schema management
- ✗ Only makes sense with Supabase backend
- ✗ Migrations still needed for production schema changes
- **Best for:** Supabase backends — use this, it’s part of the package. Excellent for relational data.

**None (local-only / no database)** — The app stores data on-device only (AsyncStorage, SQLite, UserDefaults, local files).

- ✓ Zero infrastructure — no backend costs, no latency
- ✓ Works offline by default
- ✓ Simpler architecture — no sync, no API calls
- ✗ Data is lost if the user deletes the app (unless backed up)
- ✗ No cross-device sync
- ✗ No server-side logic or shared data
- **Best for:** Mobile apps that are primarily tools (calculators, trackers, utilities), offline-first apps, or MVPs that don’t need shared data. Consider adding a backend later if the product grows.

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

**Auth.js (NextAuth)** — Open-source auth for Next.js.

- ✓ Open source — no vendor dependency
- ✓ Supports many OAuth providers
- ✓ Database adapters for most databases
- ✗ More setup and configuration than Clerk
- ✗ Less polished UI — you build your own forms
- ✗ Session management can be tricky
- **Best for:** Developers who want open-source auth with full control.

**Supabase Auth** — Auth built into the Supabase platform.

- ✓ Integrated with Supabase — Row Level Security uses auth
- ✓ Email/password, magic links, OAuth providers
- ✓ Free with Supabase
- ✗ Only makes sense with Supabase backend
- ✗ Less polished than Clerk’s UI components
- **Best for:** Supabase backends — use this, it’s part of the package.

**None (no auth needed)** — The app doesn’t require user accounts or sign-in.

- ✓ Simpler UX — no sign-up friction, instant access
- ✓ Less infrastructure to manage
- ✓ Better for tools, utilities, and single-player experiences
- ✗ No personalization or saved preferences across devices
- ✗ Can’t gate features behind subscription tiers (without device-level checks)
- **Best for:** Mobile utility apps, offline tools, calculators, single-player experiences, or MVPs testing core value before adding accounts. Can always add auth later.

-----

## Payment Providers

### Web / SaaS Payments

**Polar** — Developer-first payment platform for SaaS and digital products.

- ✓ Built specifically for developers and SaaS products
- ✓ Handles subscriptions, one-time payments, and licensing
- ✓ Excellent API and webhook support
- ✓ Generous free tier — no monthly fee, only transaction fees
- ✓ Built-in customer portal
- ✗ Newer platform — smaller community than Stripe
- ✗ Less suitable for physical goods or complex billing
- **Best for:** SaaS products, digital products, developer tools. Default recommendation for web.

**Stripe** — The most flexible and widely-used payment platform.

- ✓ Supports virtually any payment model
- ✓ Largest ecosystem — extensive documentation, libraries, integrations
- ✓ Stripe Checkout for quick integration
- ✓ Billing portal, invoicing, subscription management
- ✗ Complex — many concepts to learn (Products, Prices, Subscriptions, etc.)
- ✗ You handle tax calculation separately (or use Stripe Tax)
- ✗ More setup than Polar for simple SaaS billing
- **Best for:** Products with complex billing needs, marketplaces, or if you need maximum flexibility.

**Lemon Squeezy** — Merchant of record for digital products.

- ✓ Handles global tax compliance — they’re the merchant of record
- ✓ Simple setup for subscriptions and one-time payments
- ✓ Built-in affiliate program
- ✓ No need to register for tax in different jurisdictions
- ✗ Higher fees than Stripe (they handle tax liability)
- ✗ Less flexible than Stripe for complex billing
- ✗ Smaller ecosystem
- **Best for:** Solo founders selling internationally who don’t want to deal with tax compliance.

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
- ✓ Analytics on conversion, trial starts, and revenue per paywall
- ✓ Works with RevenueCat or handles purchases directly via StoreKit/Billing
- ✗ Focused on paywall presentation — not a full subscription backend (pair with RevenueCat for that)
- ✗ Adds SDK overhead to your app
- ✗ Free tier is limited — paid plans required for A/B testing
- **Best for:** Mobile apps that want to optimize subscription conversion through paywall experimentation. Best paired with RevenueCat for the full billing stack.

**None (no payments needed)** — The app is free with no monetization, or monetization will be added later.

- ✓ Ship faster — no payment integration complexity
- ✓ No App Store commission considerations
- ✓ Focus entirely on core product value
- ✗ No revenue from day one
- ✗ Adding payments later requires an app update and review
- **Best for:** Free utility apps, apps exploring product-market fit before monetizing, or apps monetized through other channels (ads, enterprise contracts, etc.).
