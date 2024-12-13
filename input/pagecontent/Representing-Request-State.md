## Representing the State of a Request for Service

The below represents a 'normal flow' for tracking the status of a request. Not all of these states states will apply to all workflows or use-cases. This guidance builds on the [Task State 
Machine]([url](https://build.fhir.org/workflow-communications.html#12.10.2)) and is intended to provide a way to communicate the status of a request regardless of the choice of FHIR exhcange mechanism.

## Normal Flow

<table border="1" borderspacing="0" style='border: 1px solid black; border-collapse: collapse' class="table">
  <thead>
  <tr class="header">
    <th class="col-1">Workflow State to Represent</th>
    <th class="col-1">Output State</th>
    <th class="col-1">FHIR Representation</th>
  </tr></thead>
<tbody>
  <tr>
    <td>First Placed (no <br>designated performer)</td>
    <td>*Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br>- 0..* SupporingInfo<br><br>Task:<br>- Status: requested<br>- Focus: [the ServiceRequest]<br>- Performer: [null]<br>- 0..* Input</td>
  </tr>
  <tr>
    <td>Performer Selected<br>(Someone Placer may <br>*Instruct*)</td>
    <td>*Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task: <br>- Status: requested<br>- Performer: [specified]<br>- Code: Fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td>Placed and Multiple<br>Potential Performers<br>Notified</td>
    <td>*Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1..* Tasks:<br>- Status: requested<br>- BusinessStatus: SeekingFulfillment<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td>Potential Fulfiller Awaiting<br>Information</td>
    <td>*Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1 Task:<br>- Status: Received<br>- BusinessStatus: Awaiting Information<br>- StatusReason/ReasonReference: [details on the information needed]<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order<br><br>0..* Tasks for other potential fulfillers still at seeking fulfillment<br><br>NOTE - challenge still to address. StatusReason is 0..1. Could be<br>waiting for multiple pieces of information.</td>
  </tr>
  <tr>
    <td>Fulfiller Accepted</td>
    <td>*Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1..* Task:<br>- Status: accepted<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td>Fulfiller Selected</td>
    <td>*Not Set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1 Task:<br>- Status: accepted<br>- BusinessStatus: selected<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order<br><br>0..* Task:<br>- Status: cancelled<br>- StatusReason: not selected<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td>In Progress</td>
    <td>* Not set*</td>
    <td>ServiceRequest:<br>- Status: active<br>- intent: order<br><br>Task:<br>- Status: in-progress<br>- Performer: [specified]<br>- Code: Fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td></td>
    <td>Awaiting Interpretation</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-progress<br>- BusinessStatus: [images available, end exam, awaiting interp, etc.]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td></td>
    <td>Draft</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-progress<br>- BusinessStatus: [images available, end exam, awaiting interp, etc.] <br>- Code: fulfill<br>- Intent: order<br>- 1..* Output</td>
  </tr>
  <tr>
    <td></td>
    <td>Preliminary</td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-rogress<br>- BusinessStatus: Preliminary<br>- Code: fulfill<br>- Intent: order<br>- 1..* Output</td>
  </tr>
  <tr>
    <td>Complete</td>
    <td>Complete (if service<br>has an output)</td>
    <td>ServiceRequest:<br>- Status: completed<br>- Intent: order<br><br>Task:<br>- Status: completed<br>- Performer: specified<br>- Code: fulfill<br>- Intent: order<br>- 0..* Output (DiagnosticReport, Observations, DocumentReference, <br>CarePlan, etc.)<br></td>
  </tr>
  <tr>
    <td></td>
    <td>Corrected</td>
    <td></td>
  </tr>
</tbody></table>

## Placer Initiated Cancellation
This is equivalent to the normal flow through the step that an intended performer has been selected. This step assumes that the service provider has not already begun service. 

<table border="1" borderspacing="0" style='border: 1px solid black; border-collapse: collapse' class="table"><thead>
  <tr class="header">
    <th class="col-1">Workflow State to Represent</th>
    <th class="col-1">Output State</th>
    <th class="col-1">FHIR Representation</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Placer Initiates Cancellation<br>(Before performance)</td>
    <td>*Not set*<br><br></td>
    <td>ServiceRequest<br>- Status: Revoked<br><br><br>1..* Task:<br>- Status: cancelled<br>- Code: fulfil<br>- Intent: order<br>- Focus: [the ServiceRequest]<br><br>OR - if a placer can't cancel on their own, BusinessStatus of CancelRequested? <br>Or a New Task with:<br><br><br>New Task:<br>- Status: Requested<br>- Code: Abort<br>- Input: [original Task]</td>
  </tr>
</tbody></table>

## Fulfiller Decline to Perform
This flow is equivalent to the normal flow up to the point that a placer first notifies a potential fulfiller of a service request. In this flow, a fulfiller declines to perform the service, and may or may not specify a reason. 

<table border="1" borderspacing="0" style='border: 1px solid black; border-collapse: collapse' class="table"><thead>
  <tr class="header">
    <th class="col-1">Workflow State to Represent</th>
    <th class="col-1">Output State</th>
    <th class="col-1">FHIR Representation</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Fulfiller Declines Request</td>
    <td>*Not set*<br><br></td>
    <td>ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1 Task:<br>- Status: rejected<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
</tbody></table>
