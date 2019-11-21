# Loading to databases

## Amazon Neptune

Create sample data.

    $ vi data.pg

`data.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes  since:2013
    p1 -- p2 :friend since:2011

Create Neptune style nodes/edges files.

    $ alias pg2aws='docker run --rm -v $PWD:/work g2glab/pg:x.x.x pg2aws'
    $ pg2aws data.pg

Load the data.

    $ curl -X POST -H 'Content-Type: application/json' http://xxx.us-west-2.neptune.amazonaws.com:xxxx/loader -d'
    { "source" : "s3://xxx/",
      "format" : "csv",
      "iamRoleArn" : "arn:aws:iam::xxxx:role/NeptuneLoadFromS3",
      "region" : "us-west-2",
      "failOnError" : "FALSE"
    }'
