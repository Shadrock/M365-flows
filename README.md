# A Lightweight Data Subject Request Tracker

This repository contains files and general instructions for creating a basic Data Subject Request tracker using Microsoft Power Automate. The tracker is made up of two flows. The specific requirements and configuration for each flow are contained in the `README` of each flow's folder along with the files you can use to import the flow into your own environment. These flows were created using [Microsoft Power Automate](https://docs.microsoft.com/power-automate/) and some familiarity with various aspects of Microsoft (MS) Sharepoint - specifically [Forms](https://support.microsoft.com/en-us/office/create-a-form-with-microsoft-forms-4ffb64cc-7d5d-402f-b82e-b1d49418fd9d), [Lists](https://support.microsoft.com/en-us/office/introduction-to-lists-0a1c3ace-def0-44af-b225-cfa8d92c52d7), and [Approvals](https://support.microsoft.com/en-us/office/what-is-approvals-a9a01c95-e0bf-4d20-9ada-f7be3fc283d3) - is helpful for replicating or modifying the tracker yourself.

#### Compatibility
![Premium License](https://img.shields.io/badge/Premium%20License-Not%20Required-green.svg "Premium license not required")
![On-Premises Connectors](https://img.shields.io/badge/On--Premises%20Connectors-No-green.svg "Does not use on-premise connectors")
![Custom Connectors](https://img.shields.io/badge/Custom%20Connectors-Not%20Required-green.svg "Does not use custom connectors")
<!-- Check this -->

#### Applies to
* [Microsoft Power Automate](https://docs.microsoft.com/power-automate/)
* Uses: Microsoft Forms, Outlook, Teams, and SharePoint.

## Use Case
Data Subject Requests (or Data Subject Access Requests) are required by various privacy laws. They allow an individual to access information about their personal data and how an organization is managing those data. This is generally for the purposes of understanding what data are held, updating the data, or asking the organization to delete their data. At Mercy Corps, such a request requires coordinating with the owners of 11 different information systems and my team needed a relatively easy way to track these requests.

The initial "system" that was developed for these was simply to receive a request via an email, then email all the system owners at regular intervals to comply with the request. There were several problems with this and the system was:
- unresponsive to data subjects who didn't get an immediate response;
- inefficient: clogged inboxes, searching for emails, etc;
- difficult to track and hard to see who has complied with what;
- difficult to report on, since any audit trail would require searching through and stitching together multiple emails;
- insecure because emails containing personal information related to the request could stay in people’s inboxes.

## Summary
The solution presented here uses a Form, a List, and Power Automate to create a more secure, more efficient system based on a two-part workflow.

### Part 1 - Automated Request Creation
The first flow takes input from a form and creates a new item in a Sharepoint list, which in turn notifies the "DSAR owner" (person responsible for managing requests) that a new item has been created. The flow also sends an automated email to the requestor informing that the request has been received. Below is a diagram of Flow 1.

![Flow diagram of Part 1](images/M365_Flow1_for_DSR.jpg)

In the flow I've created, the DSAR owner manually reviews the request and then continues the flow (in Part 2, below) by updating the request. This step could also be part of the flow, but it was easiest for me to simply review the list manually and update each item at regular intervals.

### Part 2 - Semi-Automated Request Compliance
Upon reviewing the request and deciding to proceed, the DSAR owner updates the status of the list item from `New` to `Approved`, which triggers an approval to multiple system owners. System owners receive an Approval via both email and as a Teams message. The system owner can simply click the appropriate response type, add a comment, and submit. This automatically updates the list item with whatever response was taken. The approval I've created allows for 3 types of response:
- The data subject was not found in the system.
- The data subject was found and deleted from the system.
- The data subject was found in the system but was not deleted (e.g. due to legal hold, data retention policies, etc.)

You can create whatever response types you need.

This allows the DSAR owner to track the status of all requests across all systems. The actions of each system owner are captured in the Microsoft Approvals history in case an audit trail is required. Membership to the Sharepoint lists that holds the requests can be easily managed to limit access to only those who need it and the fact that Approvals transform from a request to a receipt once the request is complete, means that no personal information remains inadvertently inside of emails.

Below is a diagram of Flow 2.

![Flow diagram of Part 2](images/M365_Flow2_for_DSR.jpg)

### Future Improvements
There are two primary changes that should be considered for this flow. The first is to combine them. I've purposefully created a two-step process to force myself to interact with the list that contains requests, but you could easily introduce an approval to flow 1 in which the DSAR owner would approve the incoming request and that approval would kick off flow 2.

The other improvement would be to generate an automatic email to the requestor once all system owners have responded. I've purposefully kept this manual since I may want to alter my correspondence with the requestor based on _how_ the system owners comply, but this could be automated as well.

## Acknowledgements / Resources
I found the following extremely helpful for developing my flow:

- [Create your first flow](https://docs.microsoft.com/en-us/power-automate/getting-started#create-your-first-flow)
- [Microsoft Power Automate documentation](https://docs.microsoft.com/en-us/power-automate/)
- This walk-through for creating a [Simple Ticketing System in SharePoint Online](https://concurrency.com/blog/february-2019/create-a-simple-ticketing-system-in-sharepoint-onl) is what got me started thinking about managing DSRs in a List versus the current spreadsheet workflow.
- This sample [request/review/approval flow from the the M365 user community](https://github.com/pnp/powerautomate-samples/tree/main/samples/request-review-and-approval-for-a-selected-file)
- A [Power Automate template from Microsoft](https://powerautomate.microsoft.com/en-us/templates/details/d62b2527bb5343d689d5107b0922e57b/start-approval-when-a-new-item-is-added/) or the [larger library of Power Automate templates](https://powerautomate.microsoft.com/en-us/templates/)
- Documentation from Microsoft outlining [parallel approvals](https://docs.microsoft.com/en-us/power-automate/parallel-modern-approvals#insert-a-parallel-branch-approval-action-for-the-sales-team) and how to [package Power Automate flows](https://powerautomate.microsoft.com/en-us/blog/import-export-bap-packages/) for sharing, import, and export.
