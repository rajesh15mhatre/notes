# Importing data fundamentals
## 1. Options to import data
![image](https://github.com/user-attachments/assets/9fcd67fe-2d2d-4165-a491-b454e2084054)

## 2. Tools 

### Data Importer
The Neo4j [Data Importer](https://neo4j.com/docs/data-importer/current/?_gl=1*1sjh6t9*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYxLjEzMjE2MTM0NTMuMTc0NjAzNTY3MS4xNzQ2MDM1Njcx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*czE3NDg4NDA5NTQkbzE4JGcxJHQxNzQ4ODQzMTg0JGo2MCRsMCRoMA..*_ga_DZP8Z65KK4*czE3NDg4NDA5NTQkbzEwJGcxJHQxNzQ4ODQzMTg1JGo2MCRsMCRoMA..)
is a "no-code" tool that facilitates data importing from relational data sources into Neo4j. Its graphical user interface allows for simple data conversion into nodes and relationships.
Data Importer allows you to:
- Visually define the graph data model, including nodes, relationships, and properties.
- Upload CSV files and link to relational databases
- Map fields to properties
- Define unique ID constraints and indexes
If your Neo4j database is hosted on [Aura DB](https://console.neo4j.io/?_gl=1*egwnqi*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYxLjEzMjE2MTM0NTMuMTc0NjAzNTY3MS4xNzQ2MDM1Njcx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*czE3NDg4NDA5NTQkbzE4JGcxJHQxNzQ4ODQzMTg0JGo2MCRsMCRoMA..*_ga_DZP8Z65KK4*czE3NDg4NDA5NTQkbzEwJGcxJHQxNzQ4ODQzMTg1JGo2MCRsMCRoMA..)
you can import data directly from several cloud hosted relational databases, including:
- PostgreSQL
- MySQL
- SQL Server
- Oracle
- Snowflake

### Cypher and LOAD CSV
Cypher has built-in support for importing data from CSV files using the LOAD CSV clause
```
LOAD CSV WITH HEADERS FROM 'file:///transactions.csv' AS row

MERGE (t:Transactions {id: row.id})
SET
    t.reference = row.reference,
    t.amount = toInteger(row.amount),
    t.timestamp = datetime(row.timestamp)
```
You can control the import process by writing Cypher queries to:
- Load data from CSV files
- Create the data model
- Transform and aggregate data
- Control transactions

### neo4j-admin
The neo4j-admin import command line interface supports importing large data sets. neo4j-admin import converts CSV files into the internal binary format of Neo4j and can import millions of rows within minutes.
