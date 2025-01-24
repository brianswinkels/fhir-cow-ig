This section describes several high-level patterns of interaction for order, referral, and transfer workflows, with a brief description of clinical scenarios in which they may apply.The set of interaction patterns described is not exhaustive: this IG uses them as a starting point to provide context in the subsequent discussions, and as a way to demonstrate the shared features of these exchanges.

The areas identified in this Implementation Guide are:

* **Order initiation**:  
(TO DO) This addresses some aspects of how orders are created - some of those aspects are workflow-related and are identified here, while others are detailed in other specifications or are not currently in scope.
* **Fulfiller determination**  
(TO DO) This addresses the workflow implications of how a fulfiller is determined - by patient-mediated selection, or direct assignment by the placer...

* **Cancelling and modifying orders**  
(TO DO) 

* **Order grouping**  
(TO DO) 

* **Sharing outputs from an order**  
(TO DO) 

* **Fulfiller need for additional information**  
(TO DO) 

<hr>
<hr>
<hr>
### The patterns described are:
* A [simple order](./simple-request.html), in which a placer creates a request and they are confident a specific performer will fulfill it. 
* A simple referral, in which a placer creates a request for a specific intended performer, but the selected performer may decline.
* A request that may be broadcasted to several potential performers, in which any of them may claim the request.
* A request sent to many potential performers, where the requestor and patient will then review information sent back by the performers before selecting a definite performer.
* A request to a central coordinator who will assign the request to a performer, and notify the requestor of who will fulfill the request.
* TODO: A request to a central coordinator, who will assign the request to a performer and additionally mediate further communication between the requestor and performer.
* A request authorized by a prescriber, which a patient then uses to seek service at a place of their choosing.


### Overview Example - Subscriptions with Task at Placer
The below is a general overview of how Subscriptions could be used with the coordinating Task hosted at the Placer. Many details are only alluded to in this draft. 

<!--
<figure>
  {% include subscriptions-general-example-task-at-placer.svg %} 
</figure>
-->

{% include svg.html img="subscriptions-general-example-task-at-placer.svg" %}

### Overview Example - Subscriptions with Task at Fulfiller

<div>
  {% include subscriptions-general-example-task-at-fulfiller.svg %} 
</div>

### Overview Example - Messaging + REST with Task at Placer
The below is a general overview of how Messaging + REST  could be used with the coordinating Task hosted at the Placer. Many details are only alluded to in this draft. 

<figure>
  {% include messaging-rest-general-task-at-placer.svg %} 
</figure>

### Overview Example - Messaging + REST with Task at Fulfiller

<figure>
  {% include messaging-rest-general-task-at-fulfiller.svg %} 
</figure>

### Overview Example - Messaging
<figure>
  {% include pure-messaging-with-placer-identifiers.svg %} 
</figure>

