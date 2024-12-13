# Representing the State of a Request for Service

The below represents a 'normal flow' for tracking the status of a request. Not all of these states states will apply to all workflows or use-cases. This guidance builds on the [Task State 
Machine]([url](https://build.fhir.org/workflow-communications.html#12.10.2)) and is intended to provide a way to communicate the status of a request regardless of the choice of FHIR exhcange mechanism.

## Normal Flow

|  Workflow State to Represent |          Output State        |     FHIR Representation      |
|:----------------------------:|:----------------------------:|:-----------------------------|
| Drafted / Pended             | *Not set*                    | ServiceRequest               |
|                              |                              | - Status: draft              |
| Proposed                     | *Not set*                    | ServiceRequest               |
|                              |                              | - Status: active             |
|                              |                              | - Intent: proposal/plan      |
|                              |                              | - 0..* input:                |
+------------------------------+------------------------------+------------------------------+


<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg"><thead>
  <tr>
    <th class="tg-c3ow">Workflow State to Represent</th>
    <th class="tg-0pky">Output State</th>
    <th class="tg-0pky">FHIR Representation</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-0pky">Drafted/Pended</td>
    <td class="tg-0pky"><span style="font-style:italic">*Not set*</span><br><br></td>
    <td class="tg-0pky">Service Request<br><br>- Status: draft</td>
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
    <td class="tg-0pky"></td>
    <td class="tg-0pky"></td>
    <td class="tg-0pky"></td>
  </tr>
  <tr>
    <td class="tg-0pky"></td>
    <td class="tg-0pky"></td>
    <td class="tg-0pky"></td>
  </tr>
</tbody></table>
