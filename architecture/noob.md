## Background

We allow supporters to raise money for a US charity without any involvement from the charity in question. This is achieved via a local cache of the list of roughly 1M 501(c)(3) organisations, as sourced from the IRS.


### Problem Statement

The original list of roughly 1M charities was loaded into Supporter's DB quite some time ago. Since then many new organisations have been granted 501(c)(3) status, as have many lost their status. When a newly minted US charity wants to begin fundraising via EDH they need to be inserted into the DB manually. Conversely, there have been rare instances of fundraising for charities whose charitable status has since been revoked.

> We need a way to identify changes between the US charities as they exist in Supporter and as the IRS states they exist.

### Proposed Solution

A tool can be written (codename: NOOB) which can be invoked whenever a new list of US charities is discovered. This tool will calculate the delta between the two universes and identify which charities have been created, updated or deleted. In order to reflect these changes in the EDH platform, NOOB will then notify any interested parties, in this case Supporter and NFP.

> These downstream applications will then be responsible for reconciling this information against their local database.

For example, Supporter will be responsible for inserting a newly-minted charity into its DB and ElasticSearch index.

Changes to a charity will most likely be applied directly, unless the change is to their name, in which case NOOB will ask for confirmation.

In the case of revocation, Supporter (as the source of truth for US charities) will do one of the following:

- if the charity has not been onboarded, simply delete them
- if the charity has been onboarded, respond with 422 Unprocessable Entity (or similar)

The second revocation scenario will cause NOOB to raise the error with the Compliance team for manual intervention. Once this process has been fleshed out a little more, we can bring it into the fold and automate it within NOOB.

### Goals. NOOB will be...

Idempotent

- Because you know it's a good idea!
- It also means that we can handle Supporter and NFP being down, because we can simply run it as many times as we feel necessary.

Ephemeral

- The IRS only publishes a new list periodically (I've heard between monthly and quarterly), so why keep it running in the mean time?
- This means we invoke NOOB with something like plain-utils run noob-prod sync.

Stateless

- Calculating the delta only requires the previous CSV and the newly released one.
- Technically we'd need to store the CSVs (potentially in an S3 bucket), or maybe even in the Git repository and Docker image, but that's easier than managing a DB.

Required Libraries

- CSV parser (assuming the IRS don't offer it in a more arcane format)
- HTTP library (to trigger the webhooks in Supporter and NFP)

Proposed Tech Stack (Elixir)

- Most of the complexity is deciding what do with within Supporter and NFP when these applications are pinged by NOOB about a charity, be it new or old.
- Given the relative simplicity of the required logic, plus the potential to leverage the concurrency features of the Erlang VM, I'd like to propose we write this in Elixir.

### Future Considerations

Outside the US

The US is the only country with favourable legislation allowing us to fundraise without an agreement in place with a given charity. That being said, other countries in which we operate do offer similar exports of locally-recognised charitable organisations. The revocation of a charity in Ireland, for example, would be of interest to NFP, in order to trigger the deactivation process.

### Sequence Diagram

{% mermaid %}

sequenceDiagram
NOOB->>NOOB: Calculate delta
opt New Charity
  NOOB->>NOOB: Charity is new
  NOOB->>Supporter: POST /charities
  alt success
    Supporter->>Supporter: INSERT INTO charities …
    Supporter->>NOOB: 201 Created
  else already exists
    Supporter->>NOOB: 208 Already Reported
  end
end
opt Updated Charity
  NOOB->>NOOB: Charity has changed
  alt only basic details have changed
    NOOB->>Supporter: PUT /charities/123
Supporter->>Supporter: UPDATE charities …
    Supporter->>NOOB: 202 Accepted
    NOOB->>NFP: PUT /organisations/123
    alt organisation exists
      NFP->>NFP: UPDATE organisations …
      NFP->>NOOB: 202 Accepted
    else doesn't exist
      NFP->>NOOB: 404 Not Found
    end
  else name has changed
    NOOB->>Compliance: Trigger manual intervention
  end
end
opt Revoked Charity
  NOOB->>NOOB: Charity has been removed
  NOOB->>Supporter: DELETE /charities/123
  alt non onboarded
    Supporter->>Supporter: DELETE FROM charities …
    Supporter->>NOOB: 202 Accepted
  else onboarded
    Supporter->>NOOB: 422 Unprocessable Entity
    NOOB->>Compliance: Trigger manual intervention
  end
end

{% endmermaid %}
