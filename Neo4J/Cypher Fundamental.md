# Cypher Fundamental

## Cypher syntax
- Nodes are represented by parentheses ().
- We use a colon to signify the label(s), for example (:Person).
- Relationships between nodes are written with two dashes, for example (:Person)--(:Movie).
- The direction of a relationship is indicated using a greater than or less than symbol < or > , for example (:Person)-→(:Movie).
- The type of the relationship is written using the square brackets between the two dashes: [ and ], for example [:ACTED_IN]
- Properties drawn in a speech bubble are specified in a JSON like syntax.
  - Properties in Neo4j are key/value pairs, for example {name: 'Tom Hanks'}
Example pattern: (m:Movie {title: 'Cloud Atlas'})<-[:ACTED_IN]-(p:Person)

## How Cypher works
MATCH - Used to retrives data and  must be used with RETURN.
In Cypher labels, property keys, and variables are case-sensitive. Cypher keywords are not case-sensitive.
Neo4j best practices include:
- Name labels using CamelCase.
- Name property keys and variables using camelCase.
- Use UPPERCASE for Cypher keywords.
Data Filter:
```
 
-- Using pattern in node
MATCH(n:Person {name:'Tom Hanks'})
RETURN n.born

-- Using Where clause
MATCH (p:Person)
WHERE p.name = 'Tom Hanks'
RETURN p.born

```

## Finding relationship

The below query returns all nodes that have acted_in relationships with the person node
```
MATCH (p:Person {name: 'Tom Hanks'})-[:ACTED_IN]->(m)
RETURN m
```
The below query returns all **MOVIE**  nodes' titles that has acted_in relationships with the person node
```
MATCH (p:Person {name: 'Tom Hanks'})-[:ACTED_IN]->(m:Movie)
RETURN m.title
```

## Traversing Relationship
```
-- Cypher query to find who directed the movie Cloud Atlas.
MATCH (m:Movie {title: "Cloud Atlas"})<-[:DIRECTED]-(p:Person)
RETURN p.name

--Which movie has Emil Eifrem acted in?
MATCH (p:Person {name:'Emil Eifrem'})-[:ACTED_IN]->(m:Movie)
RETURN m.title
```


## Filter queries
```
-- OR clause
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released = 2008 OR m.released = 2009
RETURN p, m

-- 2 ways of filtering by node labels
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.title='The Matrix'
RETURN p.name

MATCH (p)-[:ACTED_IN]->(m)
WHERE p:Person AND m:Movie AND m.title='The Matrix'
RETURN p.name

--Filtering using ranges
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE 2000 <= m.released <= 2003
RETURN p.name, m.title, m.released


  MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
  WHERE m.released >= 2000  AND m.released <= 2003
  RETURN p.name, m.title, m.released

--Filtering by the existence of a property
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE p.name='Jack Nicholson' AND m.tagline IS NOT NULL
RETURN m.title, m.tagline

--Filtering by partial strings
MATCH (p:Person)-[:ACTED_IN]->()
WHERE p.name STARTS WITH 'Michael'
RETURN p.name

MATCH (p:Person)-[:ACTED_IN]->()
WHERE toLower(p.name) STARTS WITH 'michael'
RETURN p.name

--Filtering by patterns in the graph
--people who wrote a movie but did not direct
MATCH (p:Person)-[:WROTE]->(m:Movie)
WHERE NOT exists( (p)-[:DIRECTED]->(m) )
RETURN p.name, m.title

--Filtering using lists
MATCH (p:Person)
WHERE p.born IN [1965, 1970, 1975]
RETURN p.name, p.born

MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE  'Neo' IN r.roles AND m.title='The Matrix'
RETURN p.name, r.roles

--What properties does a node or relationship have
MATCH (p:Person)
RETURN p.name, keys(p)

--What properties exist in the graph?
--return all the property keys defined in the graph.
CALL db.propertyKeys()


---TEST ----
--What actors in the movie As Good as It Gets were born after 1960?
MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.title = 'As Good as It Gets' AND p.born > 1960
RETURN p.name, p.born

```

## Writing Data to Neo4j
We use the MERGE keyword to create a pattern in the database. After the MERGE keyword, we specify the pattern that we want to create.
When you use MERGE to create a node, you must specify at least one property that will be the unique primary key for the node.
```
--create a node to represent Michael Caine
MERGE (p:Person {name: 'Michael Caine'})

--Verify if node is created
MATCH (p:Person {name: 'Michael Caine'})
RETURN p

--Executing multiple Cypher clauses
MERGE (p:Person {name: 'Katie Holmes'})
MERGE (m:Movie {title: 'The Dark Knight'})
RETURN p, m

```
The benefit of using CREATE is that it does not look up the primary key before adding the node. You can use CREATE if you are sure your data is clean and you want greater speed during import. 

TEST
```
--Create a new Person node for Daniel Kaluuya
MERGE (p:Person {name: 'Daniel Kaluuya'})

--Check if node is created
MATCH (p:Person {name: 'Daniel Kaluuya})
RETURN p
```

## Creating a relationship between two nodes
```
--Find existing Person and Movie and create a relationship
MATCH (p:Person {name: 'Michael Caine'})
MATCH (m:Movie {title: 'The Dark Knight'})
MERGE (p)-[:ACTED_IN]->(m)

--Confirm relatioship is created
MATCH (p:Person {name: 'Michael Caine'})-[:ACTED_IN]-(m:Movie {title: 'The Dark Knight'})
RETURN p, m

--Creating nodes and relationships using multiple clauses
MERGE (p:Person {name: 'Chadwick Boseman'})
MERGE (m:Movie {title: 'Black Panther'})
MERGE (p)-[:ACTED_IN]-(m)
--NOTE: By default, if you do not specify the direction when you create the relationship, it will always be assumed left-to-right.

--Check the created graph and relationship is created left to right direction
MATCH (p:Person {name: 'Chadwick Boseman'})-[:ACTED_IN]-(m:Movie {title: 'Black Panther'})
RETURN p, m

--Create nodes and a relationship in single clause
MERGE (p:Person {name: 'Emily Blunt'})-[:ACTED_IN]->(m:Movie {title: 'A Quiet Place'})
RETURN p, m
--NOTE: We can execute this Cypher code multiple times and it will not create any new nodes or relationships.

------TEST------
--Ways to create a relationship from a to b
MERGE (a)-[:LIKES]→(b)
MERGE (a)-[:LIKES]-(b)
```

## Updating Properties
```
--Set a property for a relationship inline as part of the MERGE clause
MATCH (p:Person {name: 'Michael Caine'})
MERGE (m:Movie {title: 'Batman Begins'})
MERGE (p)-[:ACTED_IN {roles: ['Alfred Penny']}]->(m)
RETURN p,m
  
--SET keyword for a reference to a node or relationship
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name = 'Michael Caine' AND m.title = 'The Dark Knight'
SET r.roles = ['Alfred Penny']
RETURN p, r, m

---Setting multiple properties
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name = 'Michael Caine' AND m.title = 'The Dark Knight'
SET r.roles = ['Alfred Penny'], m.released = 2008
RETURN p, r, m

--Updating properties
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name = 'Michael Caine' AND m.title = 'The Dark Knight'
SET r.roles = ['Mr. Alfred Penny']
RETURN p, r, m

--Removing properties from relationship
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE p.name = 'Michael Caine' AND m.title = 'The Dark Knight'
REMOVE r.roles
RETURN p, r, m

--Removing properties from node
MATCH (p:Person)
WHERE p.name = 'Gene Hackman'
SET p.born = null
RETURN p

----TEST----
--Remove tagline properties from all movie nodes
MATCH (m:Movie)
REMOVE m.tagline
RETURN  m

--Modify this Cypher to use the SET clause to add the following properties to the Movie node: tagline: Gripping, scary, witty and timely! released: 2017
MATCH (m:Movie {title: 'Get Out'})
SET m.tagline = 'Gripping, scary, witty and timely!'
SET m.released = 2017
RETURN m.title, m.tagline, m.released

```
You can remove or delete a property from a node or relationship by using the **REMOVE** keyword, or setting the property to **null**.
**NOTE: You should never remove the property that is used as the primary key for a node.**

## Merge Processing
MERGE operations work by first trying to find a pattern in the graph. If the pattern is found then the data already exists and is not created. If the pattern is not found, then the data can be created.
- Customizing MERGE behavior
When we run below code 1st time createdAt property gets adeed and when we run it next time updatedAt properties get created and all next run updatedAt gets updated
```
MERGE (p:Person {name: 'Raj Kumar'})
// Only set the `createdAt` property if the node is created during this query
ON CREATE SET p.createdAt = datetime()
// Only set the `updatedAt` property if the node was created previously
ON MATCH SET p.updatedAt = datetime()
// Set the `born` property regardless
SET p.born = 2006
RETURN p

--TEST : How can we update this code to include her birth year of 1911?

MERGE (p:Person {name: 'Lucille Ball'})
????
SET p.born = 1911
RETURN p
ANS: Using ON MATCH

--TEST: user timestamp on node creation and updation time
MERGE (m:Movie {title: 'Rocketman'})
ON CREATE SET m.createdAt = datetime()
ON MATCH SET m.updatedAt = datetime()
SET m.tagline = "The Only Way to Tell His Story is to live His Fantasy.",
    m.released = 2019
RETURN m

```
## Deleting data
```
--Create and delete the node
MERGE (p:Person {name: 'Raj'})
RETURN p

MATCH (p:Person {name: 'Raj'})
DELETE p

--Delete node with relationship will give error to delete command
--Create node and relationship
MATCH (m:Movie {title: 'The Matrix'})
MERGE (p:Person {name: 'Raj'})
MERGE (p)-[r:ACTED_IN]->(m)
RETURN m, p, r
--YOU EILL GET ERROR
MATCH (m:Movie {title: 'The Matrix'})-[r:ACTED_IN]->(p:Person {name: 'Raj'})
DELETE p

--USE "DETACH DELETE" TO delete the node with the relationship
MATCH (m:Movie {title: 'The Matrix'})-[r:ACTED_IN]->(p:Person {name: 'Raj'})
DETACH DELETE p

------WARNING------
Code below will delete the entire graph DB
MATCH (n)
DETACH DELETE n
-----------------

--Create and delete a relationship
MATCH (m:Movie {title: 'The Matrix'})
MERGE (p:Person {name: 'Raj'})
MERGE (p)-[r:ACTED_IN]->(m)
RETURN m, p, r

MATCH (m:Movie {title: 'The Matrix'})-[r:ACTED_IN]->(p:Person {name: 'Raj'})
DELETE r
RETURN p, m

--Deleting labels
--CREATE a node and a lable
MERGE (p:Person {name: 'Raj'})
SET p:developer
RETURN P

MATCH (p:Person {name: 'Raj'})
REMOVE p:Developer
RETURN p


---------CHECK ALL THE LABLES EXISTS IN GRAPH-----
CALL db.labels()

```
TEST

```
--DELETE A person 
--Check a person in the graph who has any relationship with any node
MATCH (e:Person {name: "Emil Eifrem"})-[]->(n)
RETURN e, n

MATCH (e:Person {name: "Emil Eifrem"})-[]->(n)
DETACH DELETE e
```




