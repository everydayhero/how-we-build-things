# Notifications

Why a distinction between domain-events, versus notifications?

- Domain events should be as small as can be achieved
- They aren't fit for consumption outside of their aggregate

Notifications, on the other hand, can reasonably be allowed to include additional data for consumer-convenience, even where this data crosses contexts.

- Webhooks are a typical mechanism by which notifications can be delivered, but others (eg pub-sub) are equally valid.

### Example, to illustrate the difference (TODO)
