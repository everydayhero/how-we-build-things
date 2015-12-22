# Data Pipeline
> tl;dr distributed systems are hard.

All **write** attempts in the everydayhero ecosystem are served by a leader system. **Reads** aren't quite so straightforward. Let's begin by considering the categories of reads as we see them, and the expectations for each.

- Public API (JSON)
    - Presents data that spans more than one leader system
    - Performs many aggregations (statistics)
- Traditional reports (PDF / CSV / XLSX)
    - Generally a time-window is specified
    - Explicit latency is tolerable, within reason
    - Given that these are frequently re-purposed by consumers, consistency (especially for $$$) is expected
- Neartime / streaming / data-subscriptions
    - Some of our more sophisticated customers (who are frequently high-value) expect near-time feedback of the performance of their campaigns.
    - eg. telethons, showing donation-tickers
- Business intelligence software
    - Explicit latency is tolerable
    - Multiple data representations are required (entities and semantic event histories are both useful)
- Behaviour marketing requirements
    - Semantic events are a basic requirement
    - "Sue updated her page's story".

### Design considerations & goals

- All reads (including serving reports / multi-system APIs) should be as near-time as possible
    - When considering implementation, we can roughly translate this to mean "batches alone won't get this done"
- When serving any data to a customer, we as an engineering team should think about the following criteria before choosing a mechanism..
    - Consistency, or availability? Which is the more suitable strategy for this use-case in general?
    - What are the required performance characteristics? Will this aggregation place an unacceptable burden on my OLTP system?
    - Is this about serving discrete entities, in which case it may be suitable for these to be served by a leader system?
    - Where is this read coming from? What is my implied warranty for the interface? Internal contracts may evolve faster than external ones.
    - Does this request fall within the responsibility of a single leader system? If it crosses boundaries, it must be served by a follower system.

### Implemention outcomes of these goals

- Reports & cross-system APIs require us to maintain an OLAP system with at least the following representations of data
    - 1 or more fit-for-purpose (don't be lazy and push complex schema into your reporting consumer!!) tables from each leader system.
    - Semantic event tables which may be used to create ad-hoc user classifications that allow us to find interesting associations between behaviours and commercial performance.
        - Emitting semantic events have additional benefits outside this system, eg health-monitoring, behavioural triggers (in Lytics) and as live-streams which may be used for real-time and 'social proof' use cases.
