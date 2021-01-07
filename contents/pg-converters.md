# PG Tools

## Installation

If **Docker** is installed on your machine, run the following:

    $ alias pg2dot='docker run --rm -v $PWD:/work g2glab/pg:0.4 pg2dot'
    $ pg2dot --version
    0.4.0

Otherwise, install **Git** and **Node**, then run the following with the latest version number:
  
    $ git clone -b v0.4.0 https://github.com/g2glab/pg.git
    $ cd pg
    $ npm install
    $ npm link
    $ pg2dot --version
    0.4.0

## Quick start

Create some sample data:

    $ vi graph.pg

`graph.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes since:2013
    p1 -- p2 :friend since:2011

Run the `pg2dot` command as an example:

    $ pg2dot graph.pg
    "graph.dot" has been created.

`graph.dot`

    digraph "graph" {
      "p1" [label="person\lp1\l" name="Bob"]
      "p2" [label="person\lp2\l" name="Alice"]
      "p1" -> "p2" [label="likes\l" since="2013"]
      "p1" -> "p2" [label="friend\l" since="2011" dir=none]
    }

You can now generate a PNG image from the converted file using graphviz.

    $ dot -T png graph.dot -o graph.png

`data.png`

![data](https://user-images.githubusercontent.com/4862919/54224265-658d3380-44b6-11e9-8f24-9a0ffef9c40d.png)

## Commands

PG to graphviz dot:

    $ pg2dot graph.pg [-p output_path_prefix]

    e.g.
    $ pg2dot graph.pg -p output/graph

PG to CSV format (for loading to graph databases):

    $ pg2csv graph.pg [-p output_path_prefix] [-d dbms]    
      dbms = neo | ora | aws

PG to JSON-PG:

    $ pg2json graph.pg [-p output_path_prefix]

JSON-PG to PG:

    $ json2pg graph.json [-p output_path_prefix]
