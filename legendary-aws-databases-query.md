<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Query Data with DynamoDB

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-databases-query)

**Author:** saqibh49@gmail.com  
**Email:** saqibh49@gmail.com

---

## Query Data with DynamoDB

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-databases-query_733d9399)

---

## Introducing Today's Project!

### What is Amazon DynamoDB?

Amazon DynamoDB is a non-relational, NoSQL database in AWS, and it’s useful because it’s super fast, scales automatically, and lets you store and retrieve data without managing servers.

### How I used Amazon DynamoDB in this project

In today's project, I used Amazon DynamoDB to query tables, edit items using CLI and console, and use transactions to add items to 2 different tables at the same time. 

### One thing I didn't expect in this project was...

One thing I didn't expect in this project is how versatile CLI can be. Up until this point I felt like CLI wouldn't be for me even in a production environment, but I see now why data engineers prefer it. 

### This project took me...

This project took me abut an hour. 

---

## Querying DynamoDB Tables

A partition key is used to filter the items when querying a table. 

A sort key is a way of filtering items in a table even more after already sorting with a partition key. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-databases-query_d105b0b0)

---

## Limits of Using DynamoDB

I ran into an error when I queried for an attribute, not using my partition key or sort key.  This was because there isn't a way for me filter by the attribute PostedBy. 

Insights we could extract from our Comment table includes creating a way for us to filter the table by the user as well as the post and date/time. Insights we can't easily extract from the Comment table includes who posted each comment under the PostedBy attribute. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-databases-query_cb3e260c)

---

## Running Queries with CLI

A query I ran in CloudShell was: 

aws dynamodb get-item \
    --table-name ContentCatalog \
    --key '{"Id":{"N":"202"}}' \
    --projection-expression "Title, ContentType, Services" \
    --return-consumed-capacity TOTAL

 This query will return the title, content type, and services of the item in the table as well as the capacity used in the query. 

Query options I could add to my query are **`--key`**, which tells DynamoDB exactly which item to look up, **`--projection-expression`**, which limits the response to only the attributes I want back, and **`--return-consumed-capacity`**, which shows how much read capacity the request used.

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-databases-query_733d9399)

---

## Transactions

A transaction is a way of querying items in 2 tables at the same time using the command line. 

I ran a transaction using the following command:

aws dynamodb transact-write-items --client-request-token TRANSACTION1 --transact-items '[
    {
        "Put": {
            "TableName" : "Comment",
            "Item" : {
                "Id" : {"S": "Events/Do a Project Together - NextWork Study Session"},
                "CommentDateTime" : {"S": "2024-9-27T17:47:30Z"},
                "Comment" : {"S": "Excited to attend!"},
                "PostedBy" : {"S": "User Connor"}
            }
        }
    },
    {
        "Update": {
            "TableName" : "Forum",
            "Key" : {"Name" : {"S": "Events"}},
            "UpdateExpression": "ADD Comments :inc",
            "ExpressionAttributeValues" : { ":inc": {"N" : "1"} }
        }
    }
]'


 This transaction did two things - added a new comment to the Comments table and updated the number of comments for the events forum in the Forum table. 

![Image](http://learn.nextwork.org/grateful_white_lucky_bat/uploads/aws-databases-query_2f65f83e)

---

---
