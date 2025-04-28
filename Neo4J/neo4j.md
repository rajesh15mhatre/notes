
# Neo4j fundamental:
## 1. Graph Thinking
URL: https://graphacademy.neo4j.com/courses/neo4j-fundamentals/?category=beginners

Neo4j is graph DB which consists of Node and vertices. Node has 1 or more labels as text and properties as key-value pairs.
Vertices have one or two-way relationships.
So finally, a graph DB consists of nodes, labels, properties, vertices, and relationships.

- Node: means noun company, person or object like: facebook
- Label: node facebook may have the label as "company"
- Properties:  node facebook can have a property in key-value format ex,  "website: www.facebook.com"
- Relationship: Node can have a Bi-directional relationship ex: Jhon --"works at"--> facebook and facebook --"employs"--> Jhon


## 2. Thinking in Graphs:
Different NoSQL DB use case:
- Document stores offer flexibility.
- Wide-column stores offer scalability for large datasets.
- Key-value stores provide simplicity and high performance.
- Graph databases enable efficient modeling and querying of complex relationships.
 
A graph database yields much faster results for queries across entities and is a great fit when you need to:
Understand the relationships between entities - for example, how two people are connected.
Self referenced data of the same type - for example, a hierarchy of employees within a company. 
Explore relationships of varying or unknown depth - for example, the use of parts within a factory. 
Calculate a route between two points in a network - for example, finding the most efficient route on public transport. 
Graph databases are a great fit for scenarios when you need to handle relationships efficiently.


## 3. Graphs Are Everywhere

Graphs allow you to uncover patterns in your data, whether that be:

- Customer: using customer data for recommendations, churn prevention, tailored offers, and targeted ads, enhancing customer retention and revenue growth.

- Network & Security: analyzing IT asset data to support comprehensive security monitoring and proactive threat response.

- Employee: storing employee data to support talent development, career management, and resource allocation, helping align workforce capabilities with business needs.

- Transactions: capturing transactional data to detect illegal activities, supporting anti-money laundering, fraud detection, credit risk assessment, and credit fraud detection by revealing hidden patterns, anomalies, and connections.

- Product: centralizing product data to support personalized recommendations, optimize new product launches, enhance customization, manage inventory, and refine pricing strategies.

- Supliers: storing data on supplier performance, inventory, costs, logistics, and compliance to optimize supply chain management, supporting programs like route planning, real-time visibility, inventory planning, and risk analysis.

- Process: creating a graph of process-related data can identify bottlenecks, improve efficiency, automate tasks, and monitor performance by analyzing operational, resource, quality, and cost data.

- Graphs are getting used in AI in the form of Knowledge graph

## 4. Querying Graphs
GQL, an ISO standard for graph databases. Cypher is Neo4j's GQL implementation. Below is sample query.
```
-- Finding person
MATCH (n:Person)
WHERE n.name = 'Tom Hanks'
RETURN n

-- finding actors in a movie
MATCH (m:Movie)<-[r:ACTED_IN]-(p:Person)
WHERE m.title = 'Toy Story'
RETURN m, r, p

-- querying genre of a movie
MATCH (m:Movie)-[r:IN_GENRE]->(g:Genre)
WHERE m.title = 'Toy Story'
RETURN m, r, g

-- Result in tabular format by using properties in the return statement rather than a node
MATCH (m:Movie)-[r:IN_GENRE]->(g:Genre)
WHERE m.title = 'Toy Story'
RETURN m.title, g.name

--Exercise query 1
MATCH (m:Movie)<-[r:ACTED_IN]-(p:Person)
WHERE p.name = "Emma Stone"
RETURN m, p, r

--Exercise query 2
MATCH (m:Movie)<-[r:ACTED_IN]-(p:Person)
WHERE m.title = "Babe"
    AND p.name = "Danny Mann"
RETURN m, p, r

```

## 5. Pattern Matching
Cypher is a declarative query language that allows you to identify patterns in your data using an ASCII-art style syntax consisting of brackets, dashes and arrows.
- (p:Person)-[r:ACTED_IN]→(m:Movie) : This pattern finds all nodes with a label of Person, that have an outgoing ACTED_IN relationship to a node with a label of Movie. p is a variable
- (p:Person) : nodes are used inside parentheses with their label(s) or properties.
- -[r:ACTED_IN]→ : Relationships are used inside Square bracket and - and --> used to define direction of relationship. he relationship type is prefixed with a colon - [:TYPE]. r is a variable used to store a relationship object

## 6. Creating Graphs
MERGE clause is used to update node properties and create graph
```
--Add property to movie node
MERGE (m:Movie {title: "Arthur the King"})
SET m.year = 2024
RETURN m

--Create a relationship between nodes
MERGE (m:Movie {title: "Arthur the King"})
MERGE (u:User {name: "Adam"})
MERGE (u)-[r:RATED {rating: 5}]->(m)
RETURN u, r, m

--Create your own favourite movie
MERGE (m:Movie {title: "Inception"})
MERGE (u:User {name: "Rajesh"})
MERGE (u)-[r:RATED {rating: 1}]->(m)
RETURN u, r, m

```

## 7. Get Neo4j
There are many options to use Neo4j, including a fully hosted cloud solution (AuraDB), local installations, and Docker containers.
Links:
- [Installations](https://neo4j.com/docs/operations-manual/current/installation/?_gl=1*kn51eh*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MDU4Ny4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MDU4Ny4wLjAuMA..)
- [Aura DB](https://neo4j.com/product/auradb/?_gl=1*kn51eh*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MDU4Ny4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MDU4Ny4wLjAuMA..)
- [Editions](https://neo4j.com/docs/operations-manual/current/introduction/?_gl=1*kn51eh*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MDU4Ny4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MDU4Ny4wLjAuMA..#_neo4j_editions)
- [Neo4j Desktop](https://neo4j.com/download/?_gl=1*1cryvm4*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MTEyOC4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MTEyOC4wLjAuMA..)
- [Cloud Deployments](https://neo4j.com/docs/operations-manual/current/cloud-deployments/?_gl=1*e8lpbw*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MTE4OC4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MTE4OC4wLjAuMA..)
- [Docker](https://neo4j.com/docs/operations-manual/current/docker/?_gl=1*e8lpbw*_gcl_au*MjAwNjI2OTMwOS4xNzQ1NjUxOTYx*_ga*Mzc1MTg2MjkyLjE3NDU2NTE5NjE.*_ga_DL38Q8KGQC*MTc0NTc0OTkyOS40LjEuMTc0NTc1MTE4OC4wLjAuMA..*_ga_DZP8Z65KK4*MTc0NTc0OTkyOC40LjEuMTc0NTc1MTE4OC4wLjAuMA..)

## 8. Neo4j Tools
- The Neo4j Console
- Programming drivers
- APIs & Libraries
- Neo4j labs innovations
