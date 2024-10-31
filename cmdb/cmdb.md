## CMDB Architecture:
- Input:
  - Discovery: automated discovery of network devices
  - Service Mapping: Get the devices and apps details via advance discovery of CIs
  - API: updating tables using APIs
- Identification and reconciliation Engine: validate input data to maintain accuracy and identity of data and avoid duplicate data.
- Data loaded in CMDB CI tables 
- Data is ready to consume by org
  
## Challenges in CMDB:
-	Customization: How to classify CIs and relate them to companies' capabilities
-	Representation: Architectural design 
-	Ownership: Lack of stakeholder and CI ownership results in CI in silos.
-	Principles: Lack of guidelines
-	Informal CMDB: old or legacy way of storing data
-	Communication: CMDB visibility, Awareness and lack of CI ownership

## CSDM
- CSDM is all about doing CMDB right way
- CSDM is a standard for long-term phased approach to have robust CMDB

## Business Process
- It is a business goal like billing, payment, delivery
- It is a manually maintained CI that can identify criticality, noth declared and determined, integrity and availability.
- It is stored under cmdb_ci_business_process table

## Product model
- An out of box table not a CI. It is used to track and manage various applications with specific version and configurations.
- helps to identify product owner, team, status, compatibility and catalog.
- There are 7 base types: application, software, contract, facility, Hardware, consumable, service.

## CMDB Groups
- It is a collection of CIs based on the results of saved query builder queries, encoded queries, or manual entries. It is not a CI.
- We can group CIs on now platform

## CMDB Classification
- These are groups of CIs, who shares attributes and are stored in their own class table.
- CMDB class is an actual table name in the instance database.
- class can be dependent or independent

## reclassification of CI
- Changing class of certain CI is reclassification due to outdated ex. location/owership changes.
- goto CI --> change the class name from the dropdown menu
- we can track the changes in the history option on the CI screen
- Consequences: if ther is a custom column in source CO and which is not present in new taget class then that data will be lost
- SN recommend not to reclassify unless you are certain of the consequences


## Orphan CI
- Say application CI is dependent on server CI and if we remove Sever CI then application CI becomes Orphan because we can not find application CI is discovery as it is dependent on server CI
- Recommend to use cascade rule while deleting CI so all dependent CIs wil be deleted
- How to find Orphan CIs
  - Search for CMDB correctness dashboard --> you can see orphan records. These dashbaord shows dta based on the Health query paramenter we set under CL Class manager--> select any CI(say Linux) --> Health --> Correctness ( if this condition met then this CI will show as orphan)
  - 


## Servicenow 
- How to find any CI in SN?
  - goto SN and search by ci table_name.LIST ex: cmdb_ci.LIST will give all the CI data
- How to create CI relationship
  - There are two ways
  1. Open a CI -> related Items -> click on '+' button --> select relation types --> serarch relavant CI under filter section --> select relationship and click on + button --> verify relationship in relateed items of CI
 
  2. goto cmdb_ci_rel table --> click on "NEW" button--> select parent CI, relationship type and child CI --> click on update

- How to check import logs?
  - Search for LOgs --> select "script Log statements"
- How to search data source?
  -  search for data source --> select source

## Affected CI vs Impacted CI
- CI directly affected like damage the which is at fault and due to which other CI are impactated. 

## application service vs business service
- application service is the logical group of a app service like emai service and all it dependant CIs like server, nerwork and so on. It is more indepth and technical
- Whereas, business service is a business-related service like Food delivery. which has many application services underneath like email sevice.It is more domain related like customers, billing, orders, app services.

## stale vs orphan record
- stale: if server is decommissioned or modified but not updated in CMDB so it is a stale record
- Orphan: if a server CI has not any relationship then that CI is called as Orphan

## Independent CI
- A CI which does not have an upward relationship and operates independently like network firewall CI





## CI class manager
- Process to update data in a class:
  - goto CI class manager --> search for class ex. windows server
  - enter **basic info** and **attributes** this will be stored as a row of a table
  - Goto **Identification Rules**: defines primary key for the record, we can use derived colums like serial number, mac ID from parent CI or we  can create new one
  - Goto **reconciliation**: Identify CI data coming from the multiple sources like import sets, discovery or other. in such cases, we can give priority to pick which source data.to consider.
  - then we can select which columns to be updated from the source
  - We can define suggested relationship and type for that CI. which will be shown wen creating an CI

## cascading relationship
- if any changes (retired/config or any) happens to a parent CI those should reflect in the child CI 

## fixed Assets
- It is a container of all assets
- To create it, search Fixed Assets --> click on New button and add CIs
- We can use this to get project cost

## CI relation rollup
- help to identify accumulated usage by grouping child. Ex. Rack is parent and servers are childs we can calculate power consuption of rack by using rollup of child data.
- Search Rollup --> Goto CI relation Rollup --> cresate new --> select RElation type, aggrigation method, parent column, application, child column, rollup class(table)
- Need to activate cmdbsyncevent and cmdb_rel_ci synch event  these two business rules for rollup.



   
## CI 
- A CI can be a physical(PC), logical(DB) or a conceptual(requisition Service) entity

## How to make CMDB better
- learn all the relationship type use proper type

