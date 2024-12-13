# Representing the State of a Request for Service

The below represents a 'normal flow' for tracking the status of a request. Not all of these states states will apply to all workflows or use-cases. This guidance builds on the [Task State 
Machine]([url](https://build.fhir.org/workflow-communications.html#12.10.2)) and is intended to provide a way to communicate the status of a request regardless of the choice of FHIR exhcange mechanism.

## Normal Flow

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-7btt{border-color:inherit;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-fymr{border-color:inherit;font-weight:bold;text-align:left;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg"><thead>
  <tr>
    <th class="tg-7btt">Workflow State to Represent</th>
    <th class="tg-fymr">Output State</th>
    <th class="tg-fymr">FHIR Representation</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-0pky">Drafted/Pended</td>
    <td class="tg-0pky"><span style="font-style:italic">*Not set*</span><br><br></td>
    <td class="tg-0pky">Service Request<br>- Status: draft</td>
  </tr>
  <tr>
    <td class="tg-0pky">Proposed</td>
    <td class="tg-0pky"><span style="font-style:italic">*Not set*</span></td>
    <td class="tg-0pky">ServiceRequest<br>- Status: active<br>- Intent: proposal/plan<br>- 0..* Input</td>
  </tr>
  <tr>
    <td class="tg-0pky">First Placed (no <br>designated performer)</td>
    <td class="tg-0pky"><span style="font-style:italic">*Not set*</span></td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br>- 0..* SupporingInfo<br><br>Task:<br>- Status: requested<br>- Focus: [the ServiceRequest]<br>- Performer: [null]<br>- 0..* Input</td>
  </tr>
  <tr>
    <td class="tg-0pky">Performer Selected<br>(Someone Placer may <br>*<span style="font-style:italic">Instruct*)</span></td>
    <td class="tg-0pky">*Not set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task: <br>- Status: requested<br>- Performer: [specified]<br>- Code: Fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky">Placed and Multiple<br>Potential Performers<br>Notified</td>
    <td class="tg-0pky">*Not set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1..* Tasks:<br>- Status: requested<br>- BusinessStatus: SeekingFulfillment<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky">Potential Fulfiller Awaiting<br>Information</td>
    <td class="tg-0pky">*Not set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1 Task:<br>- Status: Received<br>- BusinessStatus: Awaiting Information<br>- StatusReason/ReasonReference: [details on the information needed]<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order<br><br>0..* Tasks for other potential fulfillers still at seeking fulfillment<br><br>NOTE - challenge still to address. StatusReason is 0..1. Could be<br>waiting for multiple pieces of information.</td>
  </tr>
  <tr>
    <td class="tg-0pky">Fulfiller Accepted</td>
    <td class="tg-0pky">*Not set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1..* Task:<br>- Status: accepted<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky">Fulfiller Selected</td>
    <td class="tg-0pky">*Not Set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>1 Task:<br>- Status: accepted<br>- BusinessStatus: selected<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order<br><br>0..* Task:<br>- Status: cancelled<br>- StatusReason: not selected<br>- Performer: [specified]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky">In Progress</td>
    <td class="tg-0pky">* Not set*</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- intent: order<br><br>Task:<br>- Status: in-progress<br>- Performer: [specified]<br>- Code: Fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">Awaiting Interpretation</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-progress<br>- BusinessStatus: [images available, end exam, awaiting interp, etc.]<br>- Code: fulfill<br>- Intent: order</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">Draft</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-progress<br>- BusinessStatus: [images available, end exam, awaiting interp, etc.] <br>- Code: fulfill<br>- Intent: order<br>- 1..* Output</td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky">Preliminary</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: active<br>- Intent: order<br><br>Task:<br>- Status: in-rogress<br>- BusinessStatus: Preliminary<br>- Code: fulfill<br>- Intent: order<br>- 1..* Output</td>
  </tr>
  <tr>
    <td class="tg-0pky">Complete</td>
    <td class="tg-0pky">Complete (if service<br>has an output)</td>
    <td class="tg-0pky">ServiceRequest:<br>- Status: completed<br>- Intent: order<br><br>Task:<br>- Status: completed<br>- Performer: specified<br>- Code: fulfill<br>- Intent: order<br>- 0..* Output (DiagnosticReport, Observations, DocumentReference, <br>CarePlan, etc.)<br></td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax">Corrected</td>
    <td class="tg-0lax"></td>
  </tr>
</tbody></table>
