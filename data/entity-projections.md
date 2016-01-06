# Distributing Entity-Projections
> What does our perfect redshift instance look like?

- It doesn't push unnecessary complexity into the Looker models
- It allows us to avoid unnecessary complexity in our semantic events
    - eg. A consistent approach to ids, rather than exposing dirty internal secrets to downstream consumers.

{% mermaid %}

sequenceDiagram
  participant LeaderApp
  participant LeaderDB
  participant LeaderWorker
  participant FollowerApp
  participant FollowerWorker
  participant FollowerDB

LeaderApp->>LeaderDB: create ProjectionUpdateIntention
LeaderApp->>LeaderWorker: Enqueue job (entity, id)
LeaderWorker->>LeaderDB: Update projection
LeaderWorker->>LeaderDB: Record intention as fullfilled
LeaderWorker->>FollowerApp: Notify follower (entity, id)
FollowerApp->>FollowerWorker: Enqueue job (entity, id)
FollowerWorker->>LeaderDB: Read data (entity, id)
FollowerWorker->>FollowerDB: Write data (entity, id)

{% endmermaid %}

## Failure modes (AKA What if...)

##### What if no job is created to update the leader-system's reporting projection?

This job can live inside the leader system's transaction for the work it's actually trying to perform.

Leader systems MAY want to periodically compare their content to their local reporting projections. eg. Supporter may perform a sanity check on page totals for some campaign, or within some recent time period.

> How would we discover this problem?

Spike in failed transactions? How could we measure this? Do we explode on failure?
Supporter's API versus reporting API disparity.

##### What if the job (to update the leader system's local reporting projection..) is created, but never worked?

> How would we discover this problem as the customer?

This would manifest as stale reports / missing rows.

> How SHOULD we discover this problem as engineers?

We could look at the table of ProjectionIntent and produce a graph (with an alert in Librato) showing the oldest (unworked) intention to update. This is known as the sweeper process (Lobsang).

We should see errors in our workers, which explain their inability to process this job. Where this DOESN'T happen, we probably have the job in the SQL store, but not the job queue.

> How do we automate recovery?

For this edge-case, we need a mechanism to re-publish jobs from their SQL representation to the job queue. Note here for clarity, that we NEVER use the SQL version as the queue itself.

To achieve this, we ASSUME failure, and have the sweeper dump new jobs on redis for every intention-row it finds. Sure, this will cause duplicates. Act accordingly.

##### The follower and leader reporting projections are out of sync beyond normal 'eventual consistency' bounds.

> How would we discover this problem as the customer?

This would manifest as stale reports / missing rows.

> How SHOULD we discover this problem as engineers?

A sweeper!!! Could periodically compare aggregations and/or individual rows between the leader and follower copies of the projections. This sweeper alarms on a failure.

> How do we automate recovery?

We need a mechanism to re-build (and subsequently re-copy) whole (or part) projections. This should be a ```rake my-first-projection:heal``` style operation.
