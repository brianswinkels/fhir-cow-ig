This implementation guide is meant to provide a shared-base on which FHIR workflows for orders, referrals, and transfers may be built.

### Goals
Our goal in creating basic guidance, even if not directly implementable, is to help initiatives focused on referrals, orders, and transfers align on core mechanisms and exchange patterns to facilitate faster and cheaper interoperability. We hope to do so by:
1. Reducing the number of decisions needed
2. Highlighting abstractions that allow development for one domain to apply to others
3. Creating shared nomenclature to help spec authors define and share goals quickly. 

IGs developed for particular care domains (such as social care referrals, durable medical equipment, nursing home placement, etc.) or for particular jurisdictions may assert conformance to this IG if they leverage the same exchange patterns.

### Referrals, Orders, and Transfers
Modern healthcare delivery includes a variety of stakeholders, settings, specialties, interventions, and equipment. This often leads to overloading of terms, and lack of understanding of commonalities among healthcare processes. One example of that is how the term `referral` is used.

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
                <li>Referral to psychiatry</li>
                <li>Authorization to see OB/MFM</li>
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
            <li><em>Usually</em> the recipient can't refuse to perform a service, although depending on the business agreements, they may be allowed to modify the request, such as by selecting a more specific service to perform. Additionally, fulfillers may indicate they are not able to fulfill the request for some reason, such as a specimen's condition or quantity.
            </li>
          </ul>
        </td>
      </tr>
    </tbody>
  </table>

All of these workflows assume that some healthcare provider, while working with a patient, decides that some action should be taken by another provider or healthcare organization. The receiving party may or may not be allowed, based on the business agreements, to reject or to modify the request for service, and the initiating party may or may not expect to receive some information back during or after the service.

#### Key Challenges Today:
The below is a summary of key challenges in the referral, orders, and transfer space today, especially for cross-organizational exchanges. A primary goal of this IG is that it facilitates improvements on at least some of these areas, though (as described below) not all of these areas are addressed.

* **Endpoint discovery** - letting actors know to where an initial notification should be pushed, or from where additional information may be queried.
* **Sharing supporting information** - it can be hard to supply all of the background with an order or referral today. For example - in HL7 v2, it's challenging to refer a patient for a surgical consult while communicating that it is based on a _particular_ set of imaging.
* **Requesting additional information** providers who receive a request for service may find that additional specific information is needed. That information may not laready exist in the patient's chart, and often that information of interest is not _always_ necessary to a given type of service. 
* **Orchestration and tracking** - coordinating which actor has the baton, monitoring the overall status of a referral, managing the earlier steps, etc.
* **Closing the loop** - sharing the outcome (and parital outcomes), such as a consult note, an imaging result, a proposed plan of care, etc. 

### Scope of this IG
Orders, Referrals, and Transfers can be challenging today for many reasons. The below is informational background and may not be comprehensive. Only some of the challenges described below are addressed as part of this implementation guide. 

#### Aspects Included in this IG
Not all of these steps apply to a given workflow for orders, transfers, or referrals, but the IG aims to provide guidance for how the following could be accomplished in FHIR
1. **A placer notifying a (potential) fulfiller of a request**
2. **Requestor sends an update** - this could be a cancellation of the request, additional supporting information, a demographics change of the patient, etc.
3. **Performer sends status updates**
4. **Performer requests additional information** - this could be a RESTful query for specific information, a letter asking for information (requiring action by the Placer), or even an instruction to the placer (such as to order and coordinate a blood test for the patient prior to service by the fulfiller). 
5. **Performer sends outcome** - this could be a result, a consult note, etc.
6. Corrections

#### Additional Pre-coordination Necessary for Push-Based Exchanges
This is covered further in the Core Concepts page in discussing how FHIR may be leveraged for push-based workflows. 

For any push-based exchange, the actors involved in the exchange must pre-coordinate on:
* The endpoints to which content should be pushed
* The events of interest for the exchange - at what workflow steps notifications should be sent between the parties
* The expected payload of these notifications - both in terms of format and semantic content
* Allowed business interactions - such as whether an order may be cancelled by either party once it has been scheduled
* Procedures for error correction and remediation - whether the sender or the receiver bears responsibility for addressing an error, how to coordinate chart corrections, service desks, etc. 

#### Aspects Not Covered in this IG:
While the below areas may be important when designing full end-to-end workflows for these areas, they are either not addressed or given only superficial treatment in this IG. 

* **Client Registration** - registering a client with an authorization server and identifying the set of authorized scopes necessary to carry out an agreed upon set of workflows, based on a business agreement.
* **Patient Matching** - this guide provides minimal guidance on how the exchanging actors should confirm the identity of the patient in the exchange, as this may vary considerably by jurisdiction based on local identifiers and the availability of national IdPs. 
* **Provider addressbooks** - identifying the set of providers, where they work, and their electronic endpoints for communication
* **Service catalogs** - what tests, procedures, or other services a fulfiller can perform, and what information they would require to perform or assess their ability to perform a service (such as their order-specific questions).
* **Scheduling** - confirming the time slot, location, and set of resources with whom a service will be performed
* **Prior authorization** - with insurance agencies or other payers
* **Authentication, Authorization, and Auditing** - while this guide assumes the use of OAuth 2.0 protocols in examples, and includes discussion of access-control considerations that spec authors should consider when designing more specific exchanges, it makes no firm requirements on details of client-system authentication, user authentication, or server authentication. It also does not discuss details for how authorized scopes of interaction may be codified.
* **Outcome / Result format and content of supporting resources** - in the overarching guidance, this IG only specifies that _some_ output may be generated from an order or referral, such as a result, a note, etc. This guide does not place any requirements on the form or content of these documents. For example - while examples in this IG may mention that a referral for imaging may conclude with a radiologist's result report, and this may be modelled in examples using DiagnosticReport and ImagingStudy resources, this guidance is not binding, and no further details are given for how the content of that DiagnosticReport may relate to particular observations, etc. This IG does provide guidance for how that output content, however it is constructed, may be tied back to an original service request to close the loop on an order or referral. 

### Definitions
* Placer - the party that creates the request for service. This is also often referred to as the "orderer" or "reqeustor". We avoid use of the term Requestor to prevent confusion in terms of client-server interactions
* Fulfiller - the actor or actors who may perform the service.

### How to Use This IG
Here are the instructions for others building on top of for a jurisdiction or clinical domain.
