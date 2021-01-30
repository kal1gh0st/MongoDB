# MongoDB

PyQt5 ebook
Tkinter ebook
SQLite Python
wxPython ebook
Windows API ebook
Java Swing ebook
Java games ebook
MySQL Java ebook
MongoDB JavaScript tutorial
last modified July 7, 2020

MongoDB JavaScript tutorial shows how to do create programs that work with MongoDB in JavaScript. This tutorial uses the native mongodb driver. (There are also other solutions such as Mongoose or Monk.)


MongoDB
MongoDB is a NoSQL cross-platform document-oriented database. It is one of the most popular databases available. MongoDB is developed by MongoDB Inc. and is published as free and open-source software.

A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents. MongoDB stores documents in collections. Collections are analogous to tables in relational databases and documents to rows.

Installing MongoDB server
The following command can be used to install MongoDB on a Debian-based Linux.

$ sudo apt-get install mongodb
The command installs the necessary packages that come with MongoDB.

$ sudo service mongodb status
mongodb start/running, process 975
With the sudo service mongodb status command we check the status of the mongodb server.

$ sudo service mongodb start
mongodb start/running, process 6448
The mongodb server is started with the sudo service mongodb start command.

MongoDB driver install
We set up the project.

$ node -v
v11.5.0
We use Node.js version 11.5.0.

$ npm i mongodb
We install the mongodb native JavaScript driver. The npm is a Node.js package manager. The MongoDB Node.js driver provides both callback based as well as Promised based interaction.

MongoDB create database
The mongo tool is an interactive JavaScript shell interface to MongoDB, which provides an interface for systems administrators as well as a way for developers to test queries and operations directly with the database.

$ mongo testdb
MongoDB shell version v4.0.7
...
> db
testdb
> db.cars.insert({name: "Audi", price: 52642})
> db.cars.insert({name: "Mercedes", price: 57127})
> db.cars.insert({name: "Skoda", price: 9000})
> db.cars.insert({name: "Volvo", price: 29000})
> db.cars.insert({name: "Bentley", price: 350000})
> db.cars.insert({name: "Citroen", price: 21000})
> db.cars.insert({name: "Hummer", price: 41400})
> db.cars.insert({name: "Volkswagen", price: 21600})
We create a testdb database and insert eight documents in the cars collection.

MongoDB promise
Promise is an object used for deferred and asynchronous computations. It represents an operation that has not completed yet, but is expected in the future.

asyncFunc()
  .then(value => { /* success */ })
  .catch(error => { /* failure */ })
  .finally( => { /* cleanup */};
The then() method always returns a Promise, which enables us to chain method calls.

Note: the MongoClient's connect returns a promise if no callback is passed.
We can also use async/await syntax to work with promises.

MongoDB JS driver
In the first example, we print the version of the Node.js driver.

driver_version.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    console.log(client.topology.clientInfo);

    client.close();
});
In the example, we connect to the server and find out the client information.

const mongo = require('mongodb');
We use the mongodb module.

const client = mongo.MongoClient;
MongoClient is used to connect to the MongoDB server.

const url = 'mongodb://localhost:27017';
This is the URL to the database. The 27017 is the default port on which the MongoDB server listens.

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {
A connection to the database is created with connect().

$ node driver_version.js
{ driver: { name: 'nodejs', version: '3.2.2' },
    os:
    { type: 'Windows_NT',
        name: 'win32',
        architecture: 'x64',
        version: '10.0.17134' },
    platform: 'Node.js v11.5.0, LE' }
The driver version is 3.2.2.

MongoDB list database collections
The listCollections() method lists available collections in a database.

list_collections.js
const mongo = require('mongodb');

const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    db.listCollections().toArray().then((docs) => {

        console.log('Available collections:');
        docs.forEach((doc, idx, array) => { console.log(doc.name) });

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example connects to the testdb database and retrieves all its collections.

db.listCollections().toArray().then((docs) => {

    console.log('Available collections:');
    docs.forEach((doc, idx, array) => { console.log(doc.name) });
...
The listCollection() method finds all the collections in the testdb database; they are printed to the console.

Note: that we should be careful about using toArray() method because it can cause a lot of memory usage.
}).catch((err) => {

    console.log(err);
}).finally(() => {

    client.close();
});
In the catch block we catch any potential exceptions and we close the connection to the database in the finally block.

Note: our applications are console programs; therefore, we close the connection at the end of the program. In web applications, the connections should be reused.
$ node list_collections.js
Available collections:
continents
cars
cities
In our database, we have these three collections.

MongoDB database statistics
The dbstats() method gets statistics of a database.

dbstats.js
const mongo = require('mongodb');

const MongoClient = mongo.MongoClient;
const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    db.stats((err, stats) => {

        if (err) throw err;

        console.log(stats);

        client.close();
    })
});
The example connects to the testdb database and shows its statistics.

$ node dbstats.js
{ db: 'testdb',
  collections: 3,
  views: 0,
  objects: 18,
  avgObjSize: 57.888888888888886,
  dataSize: 1042,
  storageSize: 69632,
  numExtents: 0,
  indexes: 3,
  indexSize: 69632,
  fsUsedSize: 136856346624,
  fsTotalSize: 254721126400,
  ok: 1 }
This is a sample output.

MongoDB find
The find() function creates a cursor for a query that can be used to iterate over results from MongoDB.

find_all.js
const mongo = require('mongodb');

const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    db.collection('cars').find({}).toArray().then((docs) => {

        console.log(docs);

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
In the example, we retrieve all docs from cars collection.

db.collection('cars').find({}).toArray().then((docs) => {
Passing an empty query returns all documents.

$ node find_all.js
[ { _id: 5cfcfc3438f62aaa09b52175, name: 'Audi', price: 52642 },
  { _id: 5cfcfc3a38f62aaa09b52176, name: 'Mercedes', price: 57127 },
  { _id: 5cfcfc3f38f62aaa09b52177, name: 'Skoda', price: 9000 },
  { _id: 5cfcfc4338f62aaa09b52178, name: 'Volvo', price: 29000 },
  { _id: 5cfcfc4838f62aaa09b52179, name: 'Bentley', price: 350000 },
  { _id: 5cfcfc4b38f62aaa09b5217a, name: 'Citroen', price: 21000 },
  { _id: 5cfcfc4f38f62aaa09b5217b, name: 'Hummer', price: 41400 },
  { _id: 5cfcfc5438f62aaa09b5217c,
    name: 'Volkswagen',
    price: 21600 } ]
This is the output.

MongoDB count documents
The count() function returns the number of matching documents in the collection.

count_documents.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    db.collection('cars').find({}).count().then((n) => {

        console.log(`There are ${n} documents`);

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example counts the number of documents in the cars collection.

$ node count_documents.js
There are 8 documents
There are eight documents in the cars collection now.

MongoDB findOne
The findOne() method returns one document that satisfies the specified query criteria. If multiple documents satisfy the query, this method returns the first document according to the natural order which reflects the order of documents on the disk.

find_one.js
const MongoClient = require('mongodb').MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let collection = db.collection('cars');
    let query = { name: 'Volkswagen' }

    collection.findOne(query).then(doc => {

        console.log(doc);

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example reads one document from the cars collection.

let query = { name: 'Volkswagen' }
The query contains the name of the car—Volkswagen.

collection.findOne(query).then(doc => {
The query is passed to the findOne() method.

$ node find_one.js
{ _id: 8, name: 'Volkswagen', price: 21600 }
This is the output of the example.

MongoDB async/await example
With async/await we can easily work with promises in a synchronous manner.

async_await.js
const MongoClient = require('mongodb').MongoClient;

const url = 'mongodb://localhost:27017';

async function findCar() {

    const client = await MongoClient.connect(url, { useNewUrlParser: true })
        .catch(err => { console.log(err); });

    if (!client) {
        return;
    }

    try {

        const db = client.db("testdb");

        let collection = db.collection('cars');

        let query = { name: 'Volkswagen' }

        let res = await collection.findOne(query);

        console.log(res);

    } catch (err) {

        console.log(err);
    } finally {

        client.close();
    }
}

findCar();
The example reads one document using async/await.

async function findCar() {
The function has the async keyword.

let res = await collection.findOne(query);
With await, we wait for the result of the findOne() function.

MongoDB query operators
It is possible to filter data using MongoDB query operators such as $gt, $lt, or $ne.

read_gt.js
const mongo = require('mongodb');

const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let query = { price: { $gt: 30000 } };

    db.collection('cars').find(query).toArray().then((docs) => {

        console.log(docs);

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example prints all documents whose car prices' are greater than 30,000.

let query = { price: { $gts: 30000 } };
The $gt operator is used to get cars whose prices are greater than 30,000.

$ node read_gt.js
[ { _id: 5d03e40536943362cffc84a7, name: 'Audi', price: 52642 },
  { _id: 5d03e40a36943362cffc84a8, name: 'Mercedes', price: 57127 },
  { _id: 5d03e41936943362cffc84ab, name: 'Bentley', price: 350000 },
  { _id: 5d03e42236943362cffc84ad, name: 'Hummer', price: 41400 } ]
This is the output of the example. Only cars more expensive than 30,000 are included.

The $and logical operator can be used to combine multiple expressions.

read_gt_lt.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

   if (err) throw err;

   const db = client.db("testdb");

   let query = { $and: [{ price: { $gt: 20000 } }, { price: { $lt: 50000 } }] };

   db.collection('cars').find(query).toArray().then((docs) => {

      console.log(docs);
   }).catch((err) => {

      console.log(err);
   }).finally(() => {

      client.close();
   });
});
In the example, we retrieve cars whose prices fall between 20,000 and 50,000.

let query = { $and: [{ price: { $gt: 20000 } }, { price: { $lt: 50000 } }] };
The $and operator combines $gt and $lt to get the results.

$ node read_gt_lt.js
[ { _id: 5d03e41336943362cffc84aa, name: 'Volvo', price: 29000 },
  { _id: 5d03e41e36943362cffc84ac, name: 'Citroen', price: 21000 },
  { _id: 5d03e42236943362cffc84ad, name: 'Hummer', price: 41400 },
  { _id: 5d03e42636943362cffc84ae,
    name: 'Volkswagen',
    price: 21600 } ]
This is the output of the example.

MongoDB projections
Projections determine which fields are passed from the database.

projections.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

   if (err) throw err;

   const db = client.db("testdb");

   db.collection('cars').find({}).project({_id: 0}).toArray().then((docs) => {

      console.log(docs);
   }).catch((err) => {

      console.log(err);
   }).finally(() => {

      client.close();
   });
});
The example excludes the _id field from the output.

db.collection('cars').find({}).project({_id: 0}).toArray().then((docs) => {
The project() method sets a projection for the query; it excludes the _id field.

$ node projections.js
[ { name: 'Audi', price: 52642 },
  { name: 'Mercedes', price: 57127 },
  { name: 'Skoda', price: 9000 },
  { name: 'Volvo', price: 29000 },
  { name: 'Bentley', price: 350000 },
  { name: 'Citroen', price: 21000 },
  { name: 'Hummer', price: 41400 },
  { name: 'Volkswagen', price: 21600 } ]
This is the output for the example.

MongoDB limit data output
The limit() method specifies the number of documents to be returned and the skip() method the number of documents to skip.

skip_limit.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

   if (err) throw err;

   const db = client.db("testdb");

   db.collection('cars').find({}).skip(2).limit(5).toArray().then((docs) => {

      console.log(docs);
   }).catch((err) => {

      console.log(err);
   }).finally(() => {

      client.close();
   });
});
The example reads from the cars collection, skips the first two documents, and limits the output to five documents.

db.collection('cars').find({}).skip(2).limit(5).toArray().then((docs) => {
The skip() method skips the first two documents and the limit() method limits the output to five documents.

$ node skip_limit.js
[ { _id: 5d03e40f36943362cffc84a9, name: 'Skoda', price: 9000 },
  { _id: 5d03e41336943362cffc84aa, name: 'Volvo', price: 29000 },
  { _id: 5d03e41936943362cffc84ab, name: 'Bentley', price: 350000 },
  { _id: 5d03e41e36943362cffc84ac, name: 'Citroen', price: 21000 },
  { _id: 5d03e42236943362cffc84ad, name: 'Hummer', price: 41400 } ]
This is the output of the example.

MongoDB aggregations
Aggregations calculate aggregate values for the data in a collection.

sum_all_cars.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

   if (err) throw err;

   const db = client.db("testdb");

   let myagr = [{$group: {_id: 1, all: { $sum: "$price" } }}];

   db.collection('cars').aggregate(myagr).toArray().then((sum) => {

      console.log(sum);
   }).catch((err) => {

      console.log(err);
   }).finally(() => {

      client.close();
   });
});
The example calculates the prices of all cars in the collection.

let myagr = [{$group: {_id: 1, all: { $sum: "$price" } }}];
The $sum operator calculates and returns the sum of numeric values. The $group operator groups input documents by a specified identifier expression and applies the accumulator expression(s), if specified, to each group.

db.collection('cars').aggregate(myagr).toArray().then((sum) => {
The aggregate() function applies the aggregation operation on the cars collection.

$ node sum_all_cars.js
[ { _id: 1, all: 581769 } ]
The sum of all prices is 581,769.

We can use the $match operator to select specific cars to aggregate.

sum_two_cars.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let myagr = [
        { $match: { $or: [{ name: "Audi" }, { name: "Volvo" }] } },
        { $group: { _id: 1, sum2cars: { $sum: "$price" } } }
    ];

    db.collection('cars').aggregate(myagr).toArray().then((sum) => {

        console.log(sum);
    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example calculates the sum of prices of Audi and Volvo cars.

let myagr = [
    { $match: { $or: [{ name: "Audi" }, { name: "Volvo" }] } },
    { $group: { _id: 1, sum2cars: { $sum: "$price" } } }
];
The expression uses $match, $or, $group, and $sum operators to do the task.

$ node sum_two_cars.js
[ { _id: 1, sum2cars: 81642 } ]
The sum of the two cars' prices is 81,642.

MongoDB insertOne
The insertOne() method inserts a single document into a collection.

insert_one.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;
const ObjectID = mongo.ObjectID;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let doc = {_id: new ObjectID(), name: "Toyota", price: 37600 };

    db.collection('cars').insertOne(doc).then((doc) => {

        console.log('Car inserted')
        console.log(doc);
    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example inserts one car into the cars collection.

let doc = {_id: new ObjectID(), name: "Toyota", price: 37600 };
This is a document to be inserted. A new Id is generated with ObjectID.

db.collection('cars').insertOne(doc).then((doc) => {
The insertOne() function inserts the document into the collection.

> db.cars.find({name:'Toyota'})
{ "_id" : ObjectId("5d03d4321f9c262a50e671ee"), "name" : "Toyota", "price" : 37600 }
We confirm the insertion with the mongo tool.

MongoDB insertMany
The insertMany() functions inserts multiple documents into a collection.

insert_many.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;
const ObjectID = mongo.ObjectID;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let collection = db.collection('continents');

    let continents = [
        { _id: new ObjectID(), name: "Africa" }, { _id: new ObjectID(), name: "America" },
        { _id: new ObjectID(), name: "Europe" }, { _id: new ObjectID(), name: "Asia" },
        { _id: new ObjectID(), name: "Australia" }, { _id: new ObjectID(), name: "Antarctica" }
    ];

    collection.insertMany(continents).then(result => {

        console.log("documents inserted into the collection");
    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example creates a continents collection and inserts six documents into it.

let collection = db.collection('continents');
The collection() method retrieves a collection; if the collection does not exist, it is created.

let continents = [
    { _id: new ObjectID(), name: "Africa" }, { _id: new ObjectID(), name: "America" },
    { _id: new ObjectID(), name: "Europe" }, { _id: new ObjectID(), name: "Asia" },
    { _id: new ObjectID(), name: "Australia" }, { _id: new ObjectID(), name: "Antarctica" }
];
This is an array of six records to be inserted into the new collection. The ObjectID() creates a new ObjectID, which is a unique value used to identify documents instead of integers.

collection.insertMany(continents).then(result => {

    console.log("documents inserted into the collection");
}).catch((err) => {

    console.log(err);
}).finally(() => {

    client.close();
});
The insertMany() method inserts the array of documents into the continents collection.

> db.continents.find()
{ "_id" : ObjectId("5cfcf97732fc4913748c9669"), "name" : "Africa" }
{ "_id" : ObjectId("5cfcf97732fc4913748c966a"), "name" : "America" }
{ "_id" : ObjectId("5cfcf97732fc4913748c966b"), "name" : "Europe" }
{ "_id" : ObjectId("5cfcf97732fc4913748c966c"), "name" : "Asia" }
{ "_id" : ObjectId("5cfcf97732fc4913748c966d"), "name" : "Australia" }
{ "_id" : ObjectId("5cfcf97732fc4913748c966e"), "name" : "Antarctica" }
The continents collection has been successfully created.

MongoDB deleteOne
The deleteOne() method is used to delete a document.

delete_one.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let query = { name: "Volkswagen" };

    db.collection('cars').deleteOne(query).then((result) => {

        console.log('Car deleted');
        console.log(result);
    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example deletes a document.

let query = { name: "Volkswagen" };

db.collection('cars').deleteOne(query).then((result) => {
...
The deleteOne() deletes the document of Volkswagen.

MongoDB updateOne
The updateOne() function is used to update a document.

update_one.js
const mongo = require('mongodb');
const MongoClient = mongo.MongoClient;

const url = 'mongodb://localhost:27017';

MongoClient.connect(url, { useNewUrlParser: true }, (err, client) => {

    if (err) throw err;

    const db = client.db("testdb");

    let filQuery = { name: "Audi" };
    let updateQuery = { $set: { "price": 52000 }};

    db.collection('cars').updateOne(filQuery, updateQuery).then(result => {

        console.log('Car updated');
        console.log(result);

    }).catch((err) => {

        console.log(err);
    }).finally(() => {

        client.close();
    });
});
The example updats a prices of a car.

let filQuery = { name: "Audi" };
let updateQuery = { $set: { "price": 52000 }};

db.collection('cars').updateOne(filQuery, updateQuery).then(result => {
The price of Audi is changed to 52,000 with the updateOne() method. The $set operator is used to change the price.

> db.cars.find({name:'Audi'})
{ "_id" : ObjectId("5cfcfc3438f62aaa09b52175"), "name" : "Audi", "price" : 52000 }
