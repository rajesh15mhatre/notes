# Creating nodes

Create nodes as per instance model:
![image](https://github.com/user-attachments/assets/84853eb9-f14e-4269-968a-035ae19487f4)

```
- Delete all the exsting graph
MATCH (n) DETACH DELETE n;

- Create new nodes as per instance model 
MERGE (:Movie {title: 'Apollo 13', tmdbId: 568, released: '1995-06-30', imdbRating: 7.6, genres: ['Drama', 'Adventure', 'IMAX']})
MERGE (:Person {name: 'Tom Hanks', tmdbId: 31, born: '1956-07-09'})
MERGE (:Person {name: 'Meg Ryan', tmdbId: 5344, born: '1961-11-19'})
MERGE (:Person {name: 'Danny DeVito', tmdbId: 518, born: '1944-11-17'})
MERGE (:Person {name: 'Jack Nicholson', tmdbId: 514, born: '1937-04-22'})
MERGE (:Movie {title: 'Sleepless in Seattle', tmdbId: 858, released: '1993-06-25', imdbRating: 6.8, genres: ['Comedy', 'Drama', 'Romance']})
MERGE (:Movie {title: 'Hoffa', tmdbId: 10410, released: '1992-12-25', imdbRating: 6.6, genres: ['Crime', 'Drama']})

- Check all nodes
MATCH (n) RETURN n
```

Once we created person node we need more node to idetify movie rater and directoer and actor 
```
-- Create user node
MERGE (s:User {userId: 534})
SET s.name = "Sandy Jones"

MERGE (c:User {userId: 105})
SET c.name = "Clinton Spencer"

```
