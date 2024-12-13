# Representing the State of a Request for Service

The below represents a 'normal flow' for tracking the status of a request. Not all of these states states will apply to all workflows or use-cases. This guidance builds on the [Task State 
Machine]([url](https://build.fhir.org/workflow-communications.html#12.10.2)) and is intended to provide a way to communicate the status of a request regardless of the choice of FHIR exhcange mechanism.

## Normal Flow

+------------------------------+------------------------------+------------------------------+
|  Workflow State to Represent |          Output State        |     FHIR Representation      |
+==============================+==============================+==============================+
| Drafted / Pended             | *Not set*                    | ServiceRequest               |
|                              |                              | - Status: draft              |
+------------------------------+------------------------------+------------------------------+
| Proposed                     | *Not set*                    | ServiceRequest               |
|                              |                              | - Status: active             |
|                              |                              | - Intent: proposal/plan      |
|                              |                              | - 0..* input:                |
+------------------------------+------------------------------+------------------------------+
