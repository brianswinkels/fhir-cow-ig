### Introduction
This implementation guide provides basic guidance for how service-request fulfillment workflows, such as Orders, Referrals, and Transfers, may be facilitated in FHIR. It includes an overview of relevant FHIR exchange mechanisms, actor definitions, and core concepts to help promote alignment.

This implementation guide creates a basic representation of the state of a request for service in FHIR. This guide also outlines how actors involved in a request for service may coordinate and act on that request for service using a RESTful approach (with subscription notifications or polling). It also outlines the commonality of such an appraoch with FHIR messaging.   

For example, if the RESTful workflow approach to convey information uses Task, then the MessageHeader.focus or custom operation body should also make use of Task populated in the same manner.  Similarly, rules around what resource instances are controlled by which party should also be adhered to regardless of paradigm (e.g. the original Request should not be updated by the filler).

The guide is intended only to provide a starting point on which other implementation guides may be built. Those other IGs may be tailored to particular care domains, such as specialist consults, social care referrals, placement in long term care facilities, requests for imaging, requests for medical equipment, etc. The guidance may also be built upon to meet the needs of specific jurisdictions, such as the United States, the Netherlands, etc. 

### Structure of this Implementation Guide
This guide is split into the below sections. 

- **Background** - describes key actors, why this guide considers order, referral, and transfer workflows together, and challenges that IG authors operating in this space should consider, only some of which this guide addresses. 

- **Core Concepts** -  This section describes how key FHIR resources are used in this guide and points to consider for event-driven exchanges in FHIR. This section also includes this guide's recommendation for representing the state of a request for service throughout its lifecycle. 

- **Workflow Patterns** - this section contains an overview of common workflow exchange patterns relevant to orders, referrals, and transfers. This includes how placers may first notify potential fulfillers of a request for service, how placers would select a fulfiller, how fulfillers may request additional information, and cancellation workflows. Each of these 'archetypical patterns" is accompanied by a basic FHIR pattern, with details for particular care-domains removed, and a list of some example workflows to which it may apply. 

- **Examples** - this section provides non-binding guidance for how the concepts developed in this guide may be applied to particular care domains. For example - it includes an overview of how a basic lab order and result may be managed, how placement in a long-term care facility may occur, etc. These examples include a variety of FHIR exchange mechanisms.   

- **Resource profiles** - profiles of specific resources used as part of this base guidance. 

### Boundaries and Relationships
This guidance is intended to be universal-realm and does not reference any national base or core profiling. This guide leverages concepts from [FHIR-Workflow](https://hl7.org/fhir/workflow.html) and applies them to Order, Referral, and Transfer workflows. 

Many of the concepts in this guide are built on top of work by the Netherlands FHIR community in their work on the Notified Pull framework which was incorporated into FHIR Subscriptions in R6, work by the Canadian FHIR community to facilitate referrals, work by BSeR to facilitate social care referrals, and earlier guidance created for Durable Medical Equipment orders. This guide also draws inspiration from work on 360X. 

### Dependencies
This IG Contains the following dependencies on other IGs.

{% include dependency-table.xhtml %}

### Cross Version Analysis

{% capture cross-version-analysis %}{% include cross-version-analysis.xhtml %}{% endcapture %}{{ cross-version-analysis | remove: '<p>' | remove: '</p>'}}

### Global Profiles

{% include globals-table.xhtml %}
