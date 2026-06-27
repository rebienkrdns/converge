# React Pack

Activates the React Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within React applications or component libraries.

## Activate When

The decision involves a React application, a shared component library, client-side state management, rendering behavior, hook design, or bundle composition. Also activate when evaluating the React layer within a Next.js or Remix application.

## Added Concerns

**Architectural concerns:**
- Is state located at the correct level? State that is needed by only one component should not be lifted to a global store.
- Are component boundaries drawn around responsibility (what a component is for) rather than convenience (what is easy to extract)?
- Is the component API designed for callers, not for the current implementation? Internal implementation details should not leak through props.

**Performance concerns:**
- Are expensive computations wrapped in `useMemo` only when there is measured evidence of a rendering bottleneck? Premature memoization adds complexity without benefit.
- Are re-render boundaries understood? A context value change re-renders every consumer. Is the context scope appropriate for its update frequency?
- Is the bundle split at meaningful route or feature boundaries, keeping initial load times within budget?

**Security concerns:**
- Is `dangerouslySetInnerHTML` used? Every use must be explicitly justified and must operate only on sanitized content.
- Are sensitive tokens or secrets stored in component state, local storage, or URL query parameters? These are accessible to XSS attacks.

**Observability concerns:**
- Are user-facing errors caught by Error Boundaries and surfaced to the monitoring system?
- Are meaningful user interactions instrumented for analytics or RUM?

## Council Interactions

The React Specialist defers to the Security Architect on XSS risk and data exposure, to the Performance Engineer on rendering optimization and bundle size, and to the Software Architect on state management architecture. Activate the Next.js Pack concurrently when the React code runs inside the App Router or Pages Router.
