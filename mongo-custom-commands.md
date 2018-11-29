---
title: 'Custom commands for MongoDB | Microsoft Docs'
description: Learn about the custom commands implemented by Cosmos DB Mongo api.
services: cosmos-db
author: sivethe
manager: kfile

ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: overview
ms.date: 11/28/2018
ms.author: sivethe
experimental: true
experiment_id: "662dc5fd-886f-4a"
---
# Custom commands for MongoDB

Azure Cosmos DB is Microsoft's globally distributed multi-model database service. You can communicate with the database's MongoDB API through any of the open source MongoDB client [drivers](https://docs.mongodb.org/ecosystem/drivers). The MongoDB API enables the use of existing client drivers by adhering to the MongoDB [wire protocol](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol).

By using the Azure Cosmos DB MongoDB API, you can enjoy the benefits of the MongoDB APIs you're used to, with all of the enterprise capabilities Azure Cosmos DB provides: [global distribution](distribute-data-globally.md), [automatic sharding](partition-data.md), availability and latency guarantees, automatic indexing of every field, encryption at rest, backups, and much more.

## MongoDB Protocol Support

The Azure Cosmos DB MongoDB API is compatible with MongoDB Server version **3.2** by default. To expose Cosmos DB specific functionality , we have added the below custom commands

- [Create Database](#create-database)
- [Update Database](#update-database)
- [Get Database](#get-database)
- [Create Collection](#create-collection)
- [Update Collection](#update-collection)
- [Get Collection](#get-collection)


### Create Database

The Create Database custom command creates a new MongoDB database.  The database name is used from the databaes context against which the command is executed.

The format of the Create Database command is as follows
```
{
  customAction: "CreateDatabase",
  offerThroughput: <int> 
}
```
where

| Field           | Type   | Description                                              |
|-----------------|--------|----------------------------------------------------------|
| customAction    | string | Name of the custom command. Must be "CreateDatabase"     |
| offerThroughput | int    | [Optional] Provisioned Throughput to set on the database |


#### Output

The response contains a document with the following fields

| Field  | Type   | Description                                                                                                |
|--------|--------|------------------------------------------------------------------------------------------------------------|
| ok     | int    | Status of response. 1 == success.  0 == failure                                                            |
| code   | int    |  [Optional] Only returned when the command failed (i.e. ok == 0). Contains the MongoDB error code.         |
| errMsg | string |  [Optional] Only returned when the command failed (i.e. ok == 0). Contains a user friendly error message.  |



#### Examples

**Create a database**

To create a database with name "test", use the following command  

```Mongo Shell
    use test
    db.runCommand({customAction: "CreateDatabase"});
``` 

**Create a database with throughput**

To create a database with name "test" and provisioned throughput of 1000 RUs, use the following command

```Mongo Shell
    use test
    db.runCommand({customAction: "CreateDatabase", offerThroughput: 1000 });
```

### Update Database

The Update Database custom command updates the properties associated with the specified database.  Currently, the only property supported is "offerThroughput"
```
{
  customAction: "UpdateDatabase",
  offerThroughput: <int> 
}
```
where

| Field           | Type   | Description                                              |
|-----------------|--------|----------------------------------------------------------|
| customAction    | string | Name of the custom command. Must be "UpdateDatabase"     |
| offerThroughput | int    | Provisioned Throughput to set on the database            |

#### Default Output

Returns a default custom command response.  Check [Default output of custom command](#default-output-of-custom-command)

#### Examples

**Update the provisioned throughput associated with a database**

To update the provisioned throughput of a database with name "test" to 1000 RUs, use the following command

```Mongo Shell
    use test
    db.runCommand({customAction: "UpdateDatabase", offerThroughput: 1000 });
```

### Default output of custom command

If not specified, a custom custom response contains a document with the following fields

| Field  | Type   | Description                                                                                                |
|--------|--------|------------------------------------------------------------------------------------------------------------|
| ok     | int    | Status of response. 1 == success.  0 == failure                                                            |
| code   | int    |  [Optional] Only returned when the command failed (i.e. ok == 0). Contains the MongoDB error code.         |
| errMsg | string |  [Optional] Only returned when the command failed (i.e. ok == 0). Contains a user friendly error message.  |