# Reference

# Property Graph Exchange Format

[https://arxiv.org/abs/1907.03936](https://arxiv.org/abs/1907.03936)

## Abstract

Recently, a variety of database implementations adopting the property graph model have emerged. However, interoperable management of graph data on these implementations is challenging due to the differences in data models and formats. Here, we redefine the property graph model incorporating the differences in the existing models and propose interoperable serialization formats for property graphs. The model is independent of specific implementations and provides a basis of interoperable management of property graph data. The proposed serialization is not only general but also intuitive, thus it is useful for creating and maintaining graph data. To demonstrate the practical use of our model and serialization, we implemented converters from our serialization into existing formats, which can then be loaded into various graph databases. This work provides a basis of an interoperable platform for creating, exchanging, and utilizing property graph data.

## Introduction

Increasing amounts of scientific and social data are described and analyzed in the form of graphs. In the context of graph analysis, the **property graph model** is becoming popular; various graph database engines, including Neo4j, Oracle Labs PGX, and Amazon Neptune, adopt this model. These graph database engines support powerful algorithms for traversing or analyzing graphs. In contrast to the standardized RDF, however, property graphs lack a standardized data model.

Here, we considered the general requirements for representing property graphs and designed two serialization formats as flat text and JSON. These formats can be converted into specific formats for each of the databases mentioned above. The serialization formats independent of certain database implementations will increase the interoperability of graph databases and will make it easier for users to import accumulated graph data.

## Model Definition

Here, we define the property graph model independent of specific graph database implementations. For the purpose of interoperability, we incorporate differences in property graph models, taking into consideration multiple labels or property values for nodes and edges, as well as mixed graphs with both of directed and undirected edges. The property graph model we redefine here requires the following characteristics:

* A property graph contains nodes and edges.
* Each of the nodes and edges can have zero or more labels.
* Each of the nodes and edges can have properties (key-value pairs).
* Each property can have multiple values.
* Each edge can be directed or undirected.

More formally, we define the property graph model as follows.

**Definition 1.  Property Graph Model**

(to be updated..)

## Serialization

According to our definition of the property graph model, we propose serialization in flat text and JSON. The flat text format (PG) is better for human readability and line-oriented processing, while the JSON format (JSON-PG) is best used for server-client communication.

The flat text PG format has the following characteristics, and an example is given in **Figure 1**.

* Each line describes a node or an edge.
* All elements in each line are separated by space or tab.
* In each of the node lines, the first column contains the node ID.
* In each of the edge lines, the first three columns contain the source node ID, the direction, and the destination node ID.
* Each line can contain an arbitrary number of labels.
* Each line can contain an arbitrary number of properties (key-value pairs).

**Figure 1.  Example of PG**

    # NODES
    101  :Person  name:Alice  age:15  country:"United States"
    102  :Person  :Student  name:Bob  country:Japan  country:Germany
    
    # EDGES
    101 -- 102  :sameSchool  :sameClass  since:2012
    102 -> 101  :likes  since:2015

More formally, we describe the PG format in the EBNF notation as follows.

**Definition 2.  PG Format**

EBNF notation of the PG format.

    PG         ::= (Node | Edge)+
    Node       ::= NODE_ID Labels Properties NEWLINE
    Edge       ::= NODE_ID Direction NODE_ID Labels Properties NEWLINE
    Labels     ::= Label*
    Properties ::= Properties*
    Label      ::= ':' STRING
    Property   ::= STRING ':' Value
    Value      ::= STRING | NUMBER
    Direction  ::= '--' | '->'

Next, we describe the JSON-PG format which follows the JSON syntax in addition to the above definition of the property graph model. The JSON-PG format has the following characteristics, and an example of the format is shown in **Figure 2**. It is to be noted that, whereas the set of labels or property values are represented as arrays in JSON, those elements are supposed to have no specific order according to the the property graph model.

* Nodes and edges are listed under `nodes` and `edges` elements, respectively.
* Edge direction is defined with the boolean element `undirected`. By default it is false (directed).
* Labels are listed under the `labels` element.
* Properties (key-value pairs) are listed under the `properties` element.

**Figure 1.  Example of PG-JSON**

    {
      "nodes":[
        {
         "id":101,
         "labels":["Person"],
         "properties":{"name":["Alice"], "age":[15], "country":["United States"]}
        },
        {
         "id":102,
         "labels":["Person", "Student"],
         "properties":{"name":["Bob"], "country":["Japan", "Germany"]}
        }
      ],
      "edges":[
        {
         "from":101,
         "to":102,
         "undirected":true,
         "labels":["sameSchool", "sameClass"],
         "properties":{"since":[2012]}
        },
        {
         "from":102,
         "to":101,
         "labels":["likes"],
         "properties":{"since":[2015]}
        }
      ]
    }

Furthermore, we have implemented command-line tools to convert between PG and JSON-PG, as well as to transform them into formats for well-known graph databases such as Neo4j, Oracle Labs PGX, and Amazon Neptune. The practical use cases of our tools demonstrate that the proposed data model and formats have the capability to describe property graph data begin used in existing graph databases (see [https://github.com/g2glab/pg](https://github.com/g2glab/pg)).

## Conclusion

In this work, we redefined the property graph model independent of specific graph database implementations and also proposed serialization formats based on the data model. Further, we implemented practical tools to convert our formats into existing ones. Our model and serialization will increase the interoperability of existing graph databases and make it easier for users to create, exchange, and utilize property graph data.

## For more information

* Project Home - [https://github.com/g2glab](https://github.com/g2glab)
* For Citation - [https://arxiv.org/abs/1907.03936](https://arxiv.org/abs/1907.03936)

