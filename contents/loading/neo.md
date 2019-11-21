# Neo4j

## With Docker

Create Neo4j container.

```
$ docker run -d \
  -p 7474:7474 \
  -p 7687:7687 \
  --name neo4j \
  --env NEO4J_AUTH=none \
  neo4j
```

Copy Neo4j files to the container.

  $ docker cp <path/to/edges/file> neo4j:/var/lib/neo4j/import/neo.edges
  $ docker cp <path/to/nodes/file> neo4j:/var/lib/neo4j/import/neo.nodes

e.g.

  $ docker cp output/musician/neo/*.edges neo4j:/var/lib/neo4j/import/neo.edges
  $ docker cp output/musician/neo/*.nodes neo4j:/var/lib/neo4j/import/neo.nodes

Import data and reload Neo4j server.

```
docker exec neo4j bash -c \
"rm -rf data/databases/graph.db/ && neo4j-admin import \
  --database=graph.db \
  --nodes=import/neo.nodes \
  --relationships=import/neo.edges \
  --delimiter='\t' \
  && chown -R root:root /data" \
  && docker restart neo4j
```

Access to http://localhost:7474 on your web browser to see the imported data.

## Without Docker

Create sample data.

    $ vi data.pg

`data.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes  since:2013
    p1 -- p2 :friend since:2011

Create Neo4j style nodes/edges files.

    $ alias pg2neo='docker run --rm -v $PWD:/work g2glab/pg:x.x.x pg2neo'
    $ pg2neo data.pg

Remove existing Neo4j database files.

    $ rm -r $NEO4J_DIR/data/databases/graph.db

Import data from nodes/edges files.

    $ $NEO4J_DIR/bin/neo4j-import \
      --into $NEO4J_DIR/data/databases/graph.db \
      --nodes output/data.neo.nodes \
      --relationships output/data.neo.edges

Start Neo4j console and access its browser ( http://localhost:7474/browser/ ).

    $ $NEO4J_DIR/bin/neo4j console
