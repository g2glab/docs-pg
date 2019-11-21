# Loading to databases

## Neo4j

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
