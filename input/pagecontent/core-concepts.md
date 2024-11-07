### Overview of actors

This IG describes actions between those who create requests for service and those who perform services. A variety of terms are used. For our purposes:

* Placer, (service) requestor, and prescriber may all be considered equivalent
* Performer and fulfiller may be considered equivalent. In many contexts, these actors may also be thought of as *potential* fulfillers or performers. This is excluded in most places for brevity unless the distinction may lead to confusion.
* Patient - this is the person to whom a service will be given, or on whom a service will be performed. In many interaction diagrams, "Patient" may also be a standin for a patient's healthcare agent or some other non-healthcare decision maker who helps to coordinate the patient's care. For example, a young patient's parent may help to coordinate where that patient will receive care. 

### Tasks, ServiceRequests, and Outputs
* A **ServiceRequest** is the FHIR representation of the request for service created, proposed, and/or authorized by a prescriber. This includes at least minimal information about the service to be performed, its overall status, and links to supporting information.
* A **Task** can serve many purposes. In this IG, Tasks serve a core-role of helping a placer and a _specific_ fulfiller manage the status of a request (in scenarios where FHIR servers are used). Many Tasks may correspond to the same ServiceRequest, and (for the purposes of coordination) separate sets of Tasks are generated for each potential fulfiller.  
* Outputs - requests for service may result in a variet of outputs, each with their own representation in FHIR. For the purposes of this IG, we refer to these generically without specifying their form. Example outputs that could be generated include a DocumentReference for a Consult Note, a DiagnosticReport and set of Observations for a lab, a CarePlan describing proposed care, or even new ServiceRequests. In scenarios with FHIR servers, this IG specifies that Outputs may be linked back to an originating ServiceRequest via Task.Output, where the Tasks (eventually) point back to a ServiceRequest via Task.BasedOn and ServiceRequest.basedOn.
  
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
