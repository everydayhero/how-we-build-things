# Next steps for implementation

- Lock down schema for projection(s)
- Compare this schema to existing segment events, and have Olga start PLANNING improved events that make better use of the new reporting projections.

### Leader system (Supporter, in this example)

- Add 1 new ActiveRecord model + migrations for reporting_intentions table
- Add 1 new ActiveRecord model + migrations for reporting table(s)
- Add 1 sidekiq job on supporter
    - Inside a transaction, updates reporting table and updates reporting_update_intention

Modify existing event emission logic for pertinent events to use the above models / worker.

### Follower system (Panoramix, in this example)

- 1 new Ecto Schema in panoramix which points at the supporter replica reporting table

> point of usefulness.. you can query a nice table in **panoramix**!!

- 1 new Ecto Schema + Migrations in panoramix which points at the redshift reporting table

- 1 new exq worker to read from the leader, and write to the follower

- 1 new API endpoint on panoramix
    - receives "pull this row please".. entity, ID
    - enqueues a job to update the follower projection

> point of usefulness.. you can query a nice table in **Looker**!!

### Later...

Add the failure-mode protections as discussed.

##### Don'ts

- Touch the aggregate store (it's not involved in this feature)
- Remove events (for now.. keep Olga safe)

#### Entities Reporting Schemata
##### Page:
  - uuid _to be created_
  - name
  - slug
  - url
  - campaign_uuid
  - charity_uuid
  - owner_uuid
  - owner_type
  - target
  - country_code
  - currency_code `Page.currency.iso_code`
  - online_total
  - offline_total
  - total
  - expiry_date
  - status
  - custom_metric_total
  - team_uuid *to be created nullable*
  - updated_at
  - created_at
  - team_position *if team = true*

##### Team:
  - name
  - team_page_url
  - target
  - offline_total
  - online_total
  - total
  - created_at
  - updated_at
  - uuid _To be created_

##### User:
  - uuid
  - name
  - birthday _nullable_
  - email
  - phone _nullable_
  - address _nullable_
  - street_address _nullable_
  - city _nullable_
  - postal_code _nullable_
  - locality _nullable_
  - region _nullable_
  - country _nullable_

##### Charity:
- uuid
- name
- country_code
- created_at
- updated_at

###### Campaign:
  - uuid
  - name
  - updated_at
  - created_at

##### Notes about the relationship between existing Supporter events versus their reporting-projection use cases

Report Field                            | Source Event
----------------------------------------|-------------------------
Charity Name                            | Charity Created/Updated
Campaign Name                           | Campaign Created/Updated
Campaign ID                             | Supporter Page Created/Updated
Supporter Page UID                      | Supporter Page Created/Updated
Supporter Page Name                     | Supporter Page Created/Updated
Supporter Page URL                      | Supporter Page Created/Updated
Supporter Page Target                   | Supporter Page Created/Updated
Supporter Page Online Total             | Donation Received
Supporter Page Offline Total            | Offline Donation Received
Supporter Page Total                    | Offline Donation Received/Donation Received
Supporter Page Custom Counter Total     | Custom Counter Updated
Supporter Page Expiry Date              | Supporter Page Created/Updated
Supporter Page Creation Date            | Supporter Page Created
Supporter Page Status                   | Supporter Page Created/Updated
Team UID                                | Supporter Page Created/Updated
Team Name                               | Team Created/Updated
Team Page URL                           | Team Created/Updated
Team Online Total                       | Donation Received
Team Offline Total                      | Offline Donation Received
Team Total                              | Offline Donation Received/Donation Received
Team Position                           | Supporter Page Created/Updated
Team Creation Date                      | Team Created/Updated
Supporter UID                           | Supporter Page Created/Updated
Supporter First Name                    | User Created/Updated
Supporter Last Name                     | User Created/Updated
Supporter Birthday                      | User Created/Updated
Supporter Email Supporter Phone Number  | User Created/Updated
Supporter Address 1                     | User Created/Updated
Supporter Address 2                     | User Created/Updated
Supporter City                          | User Created/Updated
Supporter Postal Code                   | User Created/Updated
Supporter State County                  | User Created/Updated
Supporter Country                       | User Created/Updated
