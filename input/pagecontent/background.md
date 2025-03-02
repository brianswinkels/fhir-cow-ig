This implementation guide is meant to provide a shared-base on which FHIR workflows for orders, referrals, and transfers may be built.

### Goals
Our goal in creating basic guidance, even if not directly implementable, is to help initiatives focused on referrals, orders, and transfers align on core mechanisms and exchange patterns to facilitate faster and cheaper interoperability. We hope to do so by:
1. Reducing the number of decisions needed
2. Highlighting abstractions that allow development for one domain to apply to others
3. Creating shared nomenclature to help spec authors define and share goals quickly. 

IGs developed for particular care domains (such as social care referrals, durable medical equipment, nursing home placement, imaging, etc.) or for particular jurisdictions may assert conformance to this IG if they leverage a compatible representation of the status of a request for service and compatible exchange mechanismss.

### Referrals, Orders, and Transfers
Modern healthcare delivery includes a variety of stakeholders, settings, specialties, interventions, and equipment. This often leads to overloading of terms, and lack of understanding of commonalities among healthcare processes. One example of that is how the term `referral` is used.

One attempt to describe the different types of processes that may be called `referrals` can be as follows:

<table border="1" borderspacing="0" style='border: 1px solid black; border-collapse: collapse' class="table">
    <thead>
      <tr class="header">
        <th class="col-1">Transfer</th>
        <th class="col-1">Referral</th>
        <th class="col-1">Order</th>
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

### Key Differences in Requirements Across Jurisdictions Today:

In reviewing other locale and care-domain specific work, this IG's authors have noted two factors which frequently motivate the creation of new exchange specifications for orders and referrals. These are:
1. Considerations for minimizing data access
2. Accomodating interactions with systems that mayt not yet support robust (and highly available) FHIR servers

** Groups prioritizing (1) ** have tended to focus on RESTful exchange and on minimizing the set of data which is first transmitted between the creator of a request for service and the potential performers of that service. This occasionally extends to the point of including no supporting information with the initial notification, and instead requiring that a potential fulfiller query for any information necessary to process the request. Often, these groups also provide guidance for controlling what a potential fullfiller may query within a data-category: this could incldue limiting a non-healthcare service provider to accessing only a subset of ServiceRequests, such as those related to transport assistance, or preventing service providers from querying about the volume of requests that were sent to their competitor, by allowing them to access service request data only for those patients for whom they have received a referral.

** Groups prioritizing (2) ** often focus on FHIR Messaging, which leads to considerations similar to those in HL7 v2 around broadcast interfaces and whether to include all information a fulfiller _might_ need to process a request in the notification.

This guide aims to support both groups. This guide can be seen as providing a roadmap for those prioritizing (2) to move towards RESTful exchanges as their broader exchange ecosystems develop. This also reduces implementation burden for vendors (and therefore, lock-in and silos) by providing a data model which may be represented using either paradigm. Groups prioritizing (1) should be mindful that limiting access can require pre-coordination which leads to implementation complexity, and that, often, a specialist receiving a referral is in the best position to know what data is relevant. This guide provides a brief description in the Core Concepts page for how placers may optionally limit a service provider to accessing data only to those patients for whom they have received a referral. 
