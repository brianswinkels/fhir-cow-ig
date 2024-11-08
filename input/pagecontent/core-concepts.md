### Overview of actors

This IG describes actions between those who create requests for service and those who perform services. A variety of terms are used. For our purposes:

* Placer, (service) requestor, referrer, and prescriber may all be considered equivalent for the purposes of this IG. We generally try to avoid the term "Requestor" to avoid ambiguity with the client-server interactions. 
* Performer and fulfiller may be considered equivalent. In many contexts, these actors may also be thought of as *potential* fulfillers or performers. This is excluded in most places for brevity unless the distinction may lead to confusion.
* Patient - this is the person to whom a service will be given, or on whom a service will be performed. In many interaction diagrams, "Patient" may also be a stand-in for a patient's healthcare agent or some other non-healthcare decision maker who helps to coordinate the patient's care. For example, a young patient's parent may help to coordinate where that patient will receive care. 

### Tasks, Requests, and Outputs Events
* A **Request** is the FHIR representation of the request for service created, proposed, and/or authorized by a prescriber. This includes at least minimal information about the service to be performed, its overall status, and links to supporting information.
* A **Task** can serve many purposes. In this IG, Tasks serve a core-role of helping a placer and a _specific_ fulfiller manage the status of a request (in scenarios where FHIR servers are used). Many Tasks may correspond to the same ServiceRequest, and (for the purposes of coordination) separate sets of Tasks are generated for each potential fulfiller.  
* **Output Events** - requests for service may result in a variety of output events, each with their own representation in FHIR. For the purposes of this IG, we refer to these generically without specifying their form. Example outputs that could be generated include a DocumentReference for a Consult Note, a DiagnosticReport and set of Observations for a lab, a CarePlan describing proposed care, or even new ServiceRequests. In scenarios with FHIR servers, this IG specifies that Outputs may be linked back to an originating ServiceRequest via Task.Output, where the Tasks (eventually) point back to a ServiceRequest via Task.BasedOn and ServiceRequest.basedOn.

### Notifications in FHIR ###
A core focus for this guide is to describe how notifications may be communicated between the actors involved in an order, referral, or transfer, and to provide guidance to help spec authors building on top of this IG manage these notifications in a consistent way.

FHIR provides several mechanisms for how notifications may be accomplished between two actors, and in many contexts the important part is that they align. At a high-level, regardless of the specific FHIR mechanism chosen, all 'push' based exchanges require pre-coordination to define:
* The events of interest to the expected recipient of notifications.
* The destination endpoint to which notifications should be sent
* The 'payload' of the notification, both in terms of its structure and semantic content. For example - a notification may be sent between two parties that is entirely self-contained, and which implicitly communicates a notification ("a result has been generated for this patient") and it's content ("no abnormal findings found"). This is analogous to HL7 v2 exchanges. Alternatively, a notification might indicate "data is ready - retrieve the data when ready based on the mechanism we've pre-coordinated or content in this notification".   
* Operational business agreements - the actors involved in an exchange must agree to business practices, such as whether a fulfiller must confirm their ability to perform a service, when and how a placer may cancel or modify a request for service once the fulfiller has accepted it, etc. 
* Correction mechanisms - invariably, exchanges as complex as these will generate errors. The parties involved must define processes and responsibility for how these errors will be managed and remediated.

The differnet options for communicating notifications in FHIR address these areas in different ways. 

#### Brief Survey of Mechanisms for Pushing FHIR Content 
Most of these mechanism are not addressed within this guide. This section is provided for context.
* POST of a resource (RESTful FHIR Creates or Updates):
  + This mechanism may be used alongside others. It requires the availability of FHIR servers.
  + Actors need to pre-coordinate where FHIR resources of interest will 'live' within the exchange ecosystem, when the resources should be posted, who may update them, and under what circumstances.
  + Note that posting of a resource, while it sounds simple, may require more complex supporting transactions. For example, to POST a ServiceRequest, the client must first obtain the FHIR ID that will be used for ServiceRequest.subject (such as a Patient's FHIR ID on the target server).
* Batch or Transaction bundles:
  + These may operate similar to the RESTful Create and Update described above, but provide a mechanism for a client to submit several transactions as a set, which can minimize some network traffic. This guide does not explore this option ind detail.
* FHIR Messaging:
  + A bundle may be sent between actors based on some Event. That bundle contains a MessageHeader resource and other resources of interest.
  + Note that there is no requirement within Messaging itself (separate from our purposes in this IG) that the resources within a Message Bundle have an independent and persistent existence, or that they be surfaceable in response to a FHIR query.
  + For our purposes, we can view Messaging as providing functionality largely analogous to most event-driven HL7 v2, where the content of the messages to be exchanged are now resources rather than PID segments, ORC segments, etc. This mechanism therefore has similar considerations, like that the sender and receiver must make tight agreements about events of interest and the content of messages. Senders should generally err on the side of sending content at a given event trigger that they expect the recipient *may* want, since (if we assume 'pure' messaging) there's no guarantee that a recipient can request or query for additional content later.
* FHIR Subscriptions:
  + These can also function very similarly to HL7 v2 exchanges (in which trading partners pre-coordinate events of interest, endpoints, and the content of messages). A Subscription may exist indicating that a party would like to receive content from a server when certain events occur. Upon these triggers, a subscription-notification bundle may be sent to the party desiring data.
  + Subscriptions includes two additional features that are potentially relevant for order, referral, and transfer workflows. The first is a mechanism for a data-holder to make a "SubscriptionTopic" available to which authorized data requestors may then subscribe for updates. Data requestors can then specify their own endpoint and select from a menu of options (chosen by the data holder) the events they're interested in and the desired format of messages. The second additional capability is a standard mechanism for a data holder to indicate to a potential recipient how they could query for specific additional information later. For example, if a patient's insurance may change between the time a referral is created and when a service will be performed, subscriptions provide a way for a referrer to inform a fulfiller of how they can obtain the patient's Coverage information later, closer to when it is needed.    
  
### Basic workflow

### Basic workflow in FHIR

### More complicated workflows we may need

#### Ability to bid

#### Multiple fulfillers

#### Ability for fulfiller to reject

#### Output document

#### Ability for placer to edit and host output document

### Bigger scarier FHIR workflow without too much guidance

### Promise to explain in more detail on later pages
