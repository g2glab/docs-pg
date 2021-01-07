# Neo4j

## Convert PG to CSV

Create a sample graph.

    $ vi graph.pg

`graph.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes since:2013
    p1 -- p2 :friend since:2011

Create Neo4j style CSV files.

    $ alias pg2csv='docker run --rm -v $PWD:/work g2glab/pg:0.4 pg2csv'
    $ pg2csv graph.pg -d neo4j

This command creates 3 files:

- `graph.neo.nodes.csv`
- `graph.neo.edges.csv`
- `graph.neo.cypher` - LOAD CSV Cypher command

## Load with Cypher command (LOAD CSV)

Run LOAD CSV command using Neo4j Shell.

    $ $NEO4J_DIR/bin/neo4j-shell -c < graph.neo.cypher

## Bulk load

Remove the existing Neo4j database files.

    $ rm -r $NEO4J_DIR/data/databases/graph.db

Import the graph from the CSV files.

    $ $NEO4J_DIR/bin/neo4j-import \
      --into $NEO4J_DIR/data/databases/graph.db \
      --nodes graph.neo.nodes.csv \
      --relationships graph.neo.edges.csv

Start Neo4j console and access its browser ( http://localhost:7474/browser/ ).

    $ $NEO4J_DIR/bin/neo4j console

## Bulk load (in Docker environment)

Create Neo4j container.

```sh
$ docker run -d \
  -p 7474:7474 \
  -p 7687:7687 \
  --name neo4j \
  --env NEO4J_AUTH=none \
  neo4j
```

Copy Neo4j files to the container.

e.g.

```sh
$ docker cp graph.neo.nodes.csv neo4j:/var/lib/neo4j/import/nodes.csv
$ docker cp graph.neo.edges.csv neo4j:/var/lib/neo4j/import/edges.csv
```

Import data and reload Neo4j server.

```sh
docker exec neo4j bash -c \
"rm -rf data/databases/graph.db/ && neo4j-admin import \
  --database=graph.db \
  --nodes=import/nodes.csv \
  --relationships=import/edges.csv \
  --delimiter='\t' \
  && chown -R root:root /data" \
  && docker restart neo4j
```

Access to http://localhost:7474 on your web browser to see the imported data.
