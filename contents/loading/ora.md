## Oracle

Create sample data.

    $ vi graph.pg

`graph.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes  since:2013
    p1 -- p2 :friend since:2011

Create Oracle style CSV files and config file.

    $ alias pg2csv='docker run --rm -v $PWD:/work g2glab/pg:0.4 pg2csv'
    $ pg2csv graph.pg -d ora

This command creates 3 files:

- `graph.ora.nodes.csv`
- `graph.ora.edges.csv`
- `graph.ora.json` - Loading configuration file

Run stand-alone PGX using JShell and load the data.

    $ /opt/oracle/graph/bin/opg-jshell
    pgx> G = session.readGraphWithProperties("graph.ora.json")
