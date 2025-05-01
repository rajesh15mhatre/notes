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
