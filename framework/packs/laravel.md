# Laravel Pack

Activates the Laravel Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Laravel applications or ecosystems.

## Activate When

The decision involves a Laravel application, any package in the Laravel ecosystem (Livewire, Inertia, Horizon, Telescope, Sanctum, Passport, Octane), or a system that exposes or consumes a Laravel API.

## Added Concerns

**Architectural concerns:**
- Is Eloquent being used within or beyond its designed access layer, creating hidden coupling between presentation and persistence?
- Are service classes respecting the container's binding lifecycle (singleton vs. transient) in a way that is safe under concurrent requests?
- Is the application structured around Laravel conventions, or fighting them? Fighting conventions increases maintenance cost without architectural gain.

**Performance concerns:**
- Are N+1 queries being introduced through eager loading omissions?
- Are jobs and queued events designed to be idempotent and safe to retry?
- Are database queries scoped with appropriate indexes for the expected query patterns?

**Security concerns:**
- Is mass assignment protected via `$fillable` or `$guarded` on all models receiving external input?
- Are raw queries parameterized, or is string interpolation used with user data?
- Are authorization policies registered and consistently applied rather than relying on inline checks?

**Observability concerns:**
- Are Horizon metrics monitored for queue depth, failure rates, and throughput?
- Are slow queries surfaced via Telescope or a query log in non-production environments?

## Council Interactions

The Laravel Specialist defers to the Security Architect on authentication and authorization patterns, to the Performance Engineer on query and queue optimization, and to the Software Architect on service layer design. When Livewire or Inertia are in scope, activate the React or Vue adaptive specialist as well.

