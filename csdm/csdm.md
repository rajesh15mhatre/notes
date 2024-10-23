# CSDM: Common Service Data Model
CSDM has 4 Domains: Foundation, Design, Manage Technical Services, and Sell/Consume.
## Foundation Domain: table has base critical referencial data like CMDB group, Product model, contracts.
The tables identified in the Foundation Domain are not used in CMDB relationships.
Common personas in this domain are Process Owner, Data Steward, Product Owner, and Contract Manager.
- CMDB Groups: CMDB Group is a collection of CIs based on the results of saved Query Builder queries, encoded queries, or manual entries.
  It is not a CI. CMDB Groups are recorded in the **cmdb_group** table
- Product Model: Product Models provide the ability to identify a Product Owner, the status of a product within
your organization, compatibility to other products, reference to product catalog, and reference list of
deployed configuration items. Product Models are extended into 7 base types: Application Model, Consumable Model, Contract
Model, Facility, Hardware, Service, Software. Product Models are recorded in the **cmdb_model** table or its extended tables aligned to the 7 base
types.The Product Model tables are NOT configuration items

