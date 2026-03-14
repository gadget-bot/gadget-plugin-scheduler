# gadget-plugin-scheduler Specification

## Purpose
Provide standalone recurring job scheduling for Gadget plugins, with optional integration by consumers.

## Standalone and Optional Integrations
- Operates standalone as a generic scheduler service.
- Consumer plugins (for example, engagement ledger) register jobs only if scheduler capability is available.
- If absent, consumers must degrade gracefully to manual/internal triggers.

## v1 Functional Requirements
1. Support recurring schedules suitable for weekly and monthly jobs.
2. Support workspace-scoped job registration and execution.
3. Provide retry behavior for transient failures.
4. Provide dedupe/idempotency hooks so downstream jobs do not double-apply effects.
5. Provide capability discovery so consumer plugins can detect availability at runtime.
6. Support timezone-aware schedules and pass effective timezone context to job handlers.

## Required Integration Use Cases
1. Weekly engagement leaderboard publishing.
2. Monthly active-user engagement awards.

## API Contract (Minimum)
- Register job: `name`, `workspace_id`, `schedule`, `timezone_mode`, `payload`
- Trigger metadata: `run_id`, `scheduled_for`, `attempt`
- Capability endpoint: `scheduler.available=true`

## Non-Goals in v1
- Domain-specific business logic (points, spam, leaderboard calculations).
- UI for schedule management.

## v2 Targets
- Advanced delivery guarantees and dead-letter handling.
- Multi-region clock coordination.

## Extractable Issues
1. **Implement recurring job engine for weekly and monthly schedules**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:infra`, `area:scheduler`, `priority:p0`, `standalone`
2. **Implement workspace-scoped registration and execution model**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:feature`, `area:scheduler`, `priority:p0`
3. **Add retry semantics and run metadata (`run_id`, `attempt`)**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:infra`, `area:scheduler`, `priority:p1`
4. **Add capability discovery API for optional consumers**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:api`, `area:scheduler`, `integration:optional`, `priority:p0`
5. **Support timezone-aware scheduling context (`workspace_local` or `utc`)**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:feature`, `area:scheduler`, `priority:p0`
