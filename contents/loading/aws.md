## Amazon Neptune

Create sample data.

    $ vi data.pg

`data.pg`

    p1 :person name:Bob
    p2 :person name:Alice
    p1 -> p2 :likes  since:2013
    p1 -- p2 :friend since:2011

Create Neptune style nodes/edges files.

    $ alias pg2aws='docker run --rm -v $PWD:/work g2glab/pg:latest pg2aws'
    $ pg2aws data.pg

Load the data.

    $ curl -X POST \
    -H 'Content-Type: application/json' \
    http://<Neptune-Endpoint>.us-west-2.neptune.amazonaws.com:8182/loader -d'
    { "source" : "s3://<S3-Bucket-Name>/",
      "format" : "csv",
      "iamRoleArn" : "arn:aws:iam::<AWS-account-ID>:role/NeptuneLoadFromS3",
      "region" : "us-west-2",
      "failOnError" : "FALSE"
    }'

After loading the data into Neptune you will receive a load-ID that can be used for checking your load status.

    curl -G \
    ‘http://<Neptune-Endpoint>.us-west-2.neptune.amazonaws.com:8182/loader/<load-ID>’

Output:

    {
    “status” : “200 OK”,
    “payload” : {
        “feedCount” : [
            {
                “LOAD_NOT_STARTED” : 1
            },
            {
                “LOAD_FAILED” : 1
            }
        ],
        “overallStatus” : {
            “fullUri” : “s3://<S3-bucket-name>”,
            “runNumber” : 1,
            “retryNumber” : 2,
            “status” : “LOAD_CANCELLED_DUE_TO_ERRORS”,
            “totalTimeSpent” : 3,
            “startTime” : 1574269317,
            “totalRecords” : 10656,
            “totalDuplicates” : 0,
            “parsingErrors” : 8,
            “datatypeMismatchErrors” : 0,
            “insertErrors” : 0
        }
    }
