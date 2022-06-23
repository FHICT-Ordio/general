# Business processes
## What is a business process
As taken from Madhura Gaikwad, a marketing professional at Processology: 
> A business process is a collection of business tasks and activities that when performed by people or systems in a structured course, produces an outcome that contributes to business goals.

A business process describes the steps taken by a stakeholder, automated system or team member to achieve a set goal. The steps described in a business process are preset and repeatable, ending with one product or result. A few examples of business processes are:

- An HR recruitment process
- Order placement and handling process
- Quality assurance process

<br>

Usually in business, a business process is defined for processes that occure regularly to define rules and workflows.

<br><br>

## Sofware in business processes
As mentioned before, business processes are not only handeled by stakeholders or team members themselves, but parts are also very often automatable. Software plays a key role in this. Think about for example Testing processes for applications, where automated tests can be run. But also many business processes in for example the financial sector benefit from software: Image how much longer handling invoices would take if every single invoice was received in mail on paper, read, processed and checked by a human team member, and the money for the invoice be brought to the bank by said human team member. Instead, software solution are made that automate large chunks of this, human interaction is only required for oversight and final aprovement, software applications like digital banking systems and invoice managers like SAP or Airbase can handle the rest automatically

<br><br>

## Ordio business process
Within my individual project "Ordio" there are also multiple business processes that happen on regular basis. One example of this is for example the contribution of code by a developer to the larger codebase. This process happens in steps with automation spread throughout, and can be detailed in a BPMN business process diagram:

<br>

![Commit BPMN diagram](./Media/Commit%20business%20process.PNG)

<br>
In this process the developer starts the feature-commitment process by creating a merge request of their code on GitHub. Next up, testing and security analysis is started through GitHub actions. If these succeed, the merge request will be passed on to a review. This reviewer will most likely be a supervisor or project manager. They wil review the feature, see if the functionality of the feature lines up with the project requirements, and if so aprove the merge request which will then be automatically merged into the larger codebase by GitHub. If the testing and security analysis does not the pass, the developer will have to go back to the development step and fix issues, bugs or security vulnerabilities that show up in these steps. If the tests do pass but the reviewer finds that the feature functionality does not line up with what is expected through the requirements, this will be communicated to the developer and changes will have to be made to align the feature with said requirements. The process ends once the feature is merged into the larger codebase and the product will be an integrated feature.

<br><br>

## References
> Madhura Gaikwad. *What is a business process*. Retreived June 23, 2022, from Processology: https://blog.processology.net/what-is-a-business-process#example-of-business-processes

> Helena Haidu (2019, April 3). *6 Business Process Examples and Automation Ideas*. Retreived June 23, 2022, from Comindware: https://www.comindware.com/blog-6-business-process-examples-automation-ideas/