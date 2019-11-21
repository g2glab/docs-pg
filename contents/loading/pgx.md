## Oracle Labs PGX

Create sample data.

    $ vi data.pg

`data.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes  since:2013
    p1 -- p2 :friend since:2011

Create PGX style nodes/edges files.

    $ alias pg2pgx='docker run --rm -v $PWD:/work g2glab/pg:x.x.x pg2pgx'
    $ pg2pgx data.pg

Run PGX console and load the data.

    $ $PGX_DIR/bin/pgx
    pgx> G = session.readGraphWithProperties("data.pgx.json")
