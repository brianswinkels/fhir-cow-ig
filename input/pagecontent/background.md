A significant part of healthcare delivery is based on workflows with many context-specific requirements. This implementation guide strives to bring together common patterns that support FHIR-based interoperability for order-based workflows.
### Scope and Goals

### Referrals, Orders, Transfers
Modern healthcare delivery includes a variety of stakeholders, settings, specialties, interventions, and equipment. This often leads to overloading of terms, and lack to understanding of commonalities among healthcare processes. One example of that is how the term `referral` is used.

One attempt to describe the different types of processes that may be called `referrals` can be as follows:

<table border="1" borderspacing="0" style='border: 1px solid black; border-collapse: collapse'>
    <thead>
      <tr class="header">
        <th>Transfer</th>
        <th>Referral</th>
        <th>Order</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td>
          <ul>
            <li>Usually not service-specific</li>
            <li>Requestor usually does not expect an outcome after the coordination</li>
            <li>Usually changing primary responsibility for a patient's care, often with patient movement
              <ul>
                <li>LTC - with functional status</li>
                <li>Burn Unit - with specific tests</li>
              </ul>
            </li>
            <li>Often includes a patient summary. May include a more specific `reason for transfer`</li>
          </ul>
        </td>
        <td>
          <ul>
            <li>Usually more high level:
              <ul>
                <li>Imaging referral for knee pain</li>
                <li>Surgical Consult</li>
                <li>Referral for physical therapy</li>
              </ul>
            </li>
            <li>May be evaluative:
              <ul>
                <li>Consider for transport assistance</li>
              </ul>
            </li>
            <li>
              May not be authorized at the moment of creation (insurance, etc.)
            </li>
            <li>
              Recipients sometimes refuse
            </li>
          </ul>
        </td>
        <td>
          <ul>
            <li>Usually more specific:
              <ul>
                <li>X-Ray 3 view knee</li>
              </ul>
            </li>
            <li>General expectation is that the service will occur and the overall performer is known
              <ul>
                <li>Some lab in the network <em>will</em> perform this test</li>
              </ul>
            </li>
            <li><em>Usually</em> the recipient can't refuse (but may not be able to fulfill the request for some reason, e.g. specimen condition/quantity)
            </li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>

#### Kinds of Requests

#### Common Ordering Patterns
These high-level patterns are common in many jurisdictions. This IG uses them as a starting point to provide context in the subsequent discussions.

##### Simplest Request

This is the highest level description of a request and an outcome, usually described as the *Happy Path* where the user places the request, and, as if by magic, the outcome shows up in a reasonable amount of time.

{% include img.html img="request-simplest.png" %}


##### Request with Acceptance

When the fulfillment of a request is not immediate, which is the usual case for cross-organizational workflows, the requestor deserves the courtesy of knowing that the known performer has received the request and is committed to fulfilling it.

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
