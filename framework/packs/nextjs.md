# Next.js Pack

Activates the Next.js Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Next.js applications.

## Activate When

The decision involves a Next.js application, the App Router or Pages Router, Server Components, Server Actions, Edge Runtime, or any caching and revalidation behavior tied to Next.js primitives.

## Added Concerns

**Architectural concerns:**
- Is the rendering mode (SSR, SSG, ISR, CSR) chosen based on data freshness requirements and performance goals, or by convention?
- Is the server/client boundary placed deliberately, minimizing the client bundle while keeping interactivity where it is needed?
- Are Server Components used to avoid unnecessary data fetching on the client, or is the component tree mixed without clear rationale?

**Performance concerns:**
- Are Core Web Vitals (LCP, CLS, INP) considered for every page that introduces new above-the-fold content?
- Is the image pipeline using `next/image` with appropriate sizing and format optimization?
- Are cache headers and revalidation strategies (`revalidate`, `cache: 'no-store'`) set intentionally and not left at defaults?

**Security concerns:**
- Are Server Actions validating and sanitizing all inputs before processing, since they are exposed as POST endpoints?
- Is sensitive logic and data access confined to Server Components or API Routes, not leaking into client bundles?
- Are environment variables that should remain server-only not prefixed with `NEXT_PUBLIC_`?

**Observability concerns:**
- Are route-level performance metrics and error rates captured through the framework's instrumentation hooks or an APM integration?
- Are build output sizes tracked to detect bundle regressions before deployment?

## Council Interactions

The Next.js Specialist defers to the Security Architect on data exposure via Server Actions and API Routes, to the Performance Engineer on Core Web Vitals and caching strategy, and to the Software Architect on server/client boundary design. Activate the React Pack concurrently when client-side state management is in scope.

