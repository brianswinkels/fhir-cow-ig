A significant part of healthcare delivery is based on workflows with many context-specific requirements. This implementation guide strives to bring together common patterns that support FHIR-based interoperability for order-based workflows.
### Scope and Goals

### Referrals, Orders, Transfers

#### Kinds of Requests

#### Common Ordering Patterns
These high-level patterns are common in many jurisdictions. This IG uses them as a starting point to provide context in the subsequent discussions.
##### Simplest Request

{% include img.html img="request-simplest.png" %}


##### Request with Acceptance

{% include img.html img="request-accept.png" %}

##### Request with Multiple Potential Performers, First-Come, First-Claim

{% include img.html img="request-claim.png" %}

##### Request with Multiple Potential Performers, Bid

{% include img.html img="request-bid.png" %}

##### Requests to a Central Service

{% include img.html img="request-central.png" %}

##### Patient-Mediated Requests

{% include img.html img="request-patient.png" %}

### What Is Covered and What Is Not

### Definitions

### How to Use This IG

Here are the instructions for others building on top of for a jurisdiction or clinical domain.
