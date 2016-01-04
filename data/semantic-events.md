# Emitting Semantic Events
### Example

An example set of hypothetical semantic events from an onboarding application:

- CharityCreated(charity_id, display_name, dba)
- CharityCommencedKYC(charity_id, commenced_at)
- CharityCompletedKYC(charity_id, completed_at, approved_by)
- CharityCommencedOnboarding(charity_id, commenced_at)
- CharityCompletedOnboarding(charity_id, completed_at, approved_by)

Only someone with domain knowledge understands what each of these events relate to and when it is appropriate to raise them but anyone familiar with our day to day operations would know that there is an onboarding process, not the finer details.

### FAQs

> These events touch many systems. Who's responsible for their quality?

The **validity** of the data is the responsibility of the emitting application. Delivery guarantees should be negotiated case by case, until we're comfortable that we have suitable mechanisms in place to cover a spectrum of delivery-failure risk-profiles.

> Um.. What's this about delivery-failure risk-profiles??

It's ok to lose some things. It's never great, but sometimes it's preferable to investing effort in prevention. Think about how this idea applies to the message you're sending.
