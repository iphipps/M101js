//show score of 50 essay of type but return student only and not _id
db.scores.find({score: 50, type: "essay"}, {"student": true, "_id" : false})

//find score equal or less than 98 but greater than 50 in type essay.  only return student and not _id
db.scores.find({score: {$gt : 50, $lte: 98}, type: "essay"}, {"student": true, "_id" : false})

//find username between F and Q.  thos letters must be capitalized even though "ian" is in lower case and a match
db.people.find({username : {$lt: "Q", $gt : "F"}})

db.data.find({"Wind Direction" : {$lt: 360, $gt : 180}}, {"State": true, "Temperature": true, "_id": false}).sort({"Temperature": 1})
 between 180 and 360

//only return profressions is it exsits
db.people.find({profession : {$exists : true }})

//$type has to do wit bson spec
db.people.find({name : {$type : 2 }})//bson spec 2 is string

libpcre regex can be used

db.people.find({name : {$regex : "a" }})//Charlie Dave and Edgar. returns results that are true

db.people.find({name : {$regex : "e$" }})//alice charle and dave
//regex tend not to be optimizable
//   ^ can be optimized if no wild cards (e.g.  ^A ) and not ^A*B

//quiz
//find names with a q in it if they have an email
db.users.find({name : {$regex : "q" }, email : {$exists: true}})

//or
//find names ending in E  OR || if the age exists
db.people.find({$or : [{name: {$regex: "e$"}}, {age: {$exists: true }} ] });
//find scores that are less than 50 OR scores greater than 90
db.score.find({$or : [{score: {$lt: 50}}, {score: {$gt: 90 }} ] });

//and
//and doesn't make a ton of sense logically
//e.g the below example can be better written db.people.find({name: {$regex: "e$"}, age: {$exists: true }}}
db.people.find({$and : [{name: {$regex: "e$"}}, {age: {$exists: true }} ] });

db.score.find({$and : [{score: {$lt: 50}}, {score: {$gt: 90 }} ] });

//not optimizable, use something like db.people.find({username : {$lt: "Q", $gt : "F"}})

//$in and $all
db.accounts.find({favorites: {$all: ["pretzels", "beer"]}}) // must have all

db.accounts.find({name: {$in: ["Howard", "John"]}}) // must have one of


//dot notation  for going deeper
db.users.find({email: {work: "richard@10gen.com", personal: "krueter@examplescom"}})//must match the order
db.users.find({"email.work":"richard@10gen.com"})//can use this.  little like fixed path depth perception

db.catalog.find({price : {$gt: 10000}, "reviews.rating": {$gte: 5}})


//cur

cur = db.people.find(); null;
cur.sort({name: -1}).limit(3).skip(2); null;
whie (cur.hasNext()) printjson(cur.next());
db.scores.find({type: "exam"}).sort({score: -1}).skip(50).limit(20)

//counting results


db.scores.count({ type:"essay", score:{$gt:90}});

//wholesale updating of doc
/*
updates are methods on a collection (at least two args, WHERE and DOCUMENT)
//update can do the following:  wholesale replacement, manipulation and UPSERTS and MultiUpdate of documentts
*/
db.people.update({name: "Smith"}, {name: "Thompson", salary: 5000})
//smith becomes Thomson and either adds or changes the salary.   the other values in the orignal document are removed, all that remains consistent is _id
//dangerous use because you might delete.
//if you're merging data this could be useful 


//USing $set Command
//for updating just fields and not whole documents
//could use update above and just replace but instead
db.people.update({name: "Alice"}), {$set: {age: 30}});
//we can also increment
db.people.update({name: "Alice"}), {$inc: {age: 1}});
//if value doesn't exist.  $inc makes the value.  e.g. age: 1

db.users.update({_id: "myrnarackham"}, {$set: {country: "RU"}});

//USING $unset Command
//for removing field.
db.people.update({name: "Jones"}, {$unset: {profession: 1}});

//USING $push, $pop, $pull, $pushAll, $pullAll, $addToSet

db.arrays.find()
//{"_id": 0, "a": [1,2,3,4]}
db.arrays.update({_id:0}, {$set: {"a.2": 5}});
//{"_id": 0, "a": [1,2,5,4]}
db.arrays.update({_id:0}, {$push: {a: 6}});
//{"_id": 0, "a": [1,2,5,4,6]}
db.arrays.update({_id:0}, {$pop: {a: 1}});//removes right most
//{"_id": 0, "a": [1,2,5,4]}
db.arrays.update({_id:0}, {$pop: {a: -1}});//removes left most
//{"_id": 0, "a": [2,5,4]}
db.arrays.update({_id:0}, {$pushAll: {a: [7,8,9]}});//adds even if values exist (possible duplicates) 
//{"_id": 0, "a": [2,5,4, 7, 8, 9]}
db.arrays.update({_id:0}, {$pull: {a: 5}});//removes matches
//{"_id": 0, "a": [2,4, 7, 8, 9]}
db.arrays.update({_id:0}, {$pullAll: {a: [8,9, 4]}});//removes all matches
//{"_id": 0, "a": [2, 7]}
db.arrays.update({_id:0}, {$addToSet: {a: 5}});//adds to set if doesn't exist
//{"_id": 0, "a": [2, 7, 5]}
//QUIZ
{ _id : "Mike", interests : [ "chess", "botany" ] }
db.friends.update( { _id : "Mike" }, { $push : { interests : "skydiving" } } );
db.friends.update( { _id : "Mike" }, { $pop : { interests : -1 } } );
db.friends.update( { _id : "Mike" }, { $addToSet : { interests : "skydiving" } } );
db.friends.update( { _id : "Mike" }, { $pushAll: { interests : [ "skydiving" , "skiing" ] } } );

{ _id : "Mike", interests : [ "botany", "skydiving", "skydiving", "skiing" ] }

//UPSERTS



db.people.update({name: "Alice"}), {$set: {age: 30}});
//what if Alice doesn't exist? then we set upsert to true.  if it doesn't exist it gets added.  if it does exist then it gets modified.
db.people.update({name: "Alice"}), {$set: {age: 30}}, {upsert: true});
//underspecfiying is an potential issue

//MultiUpdate
//by default, update only affects a single document, the first find unless you use multi
db.people.update({ }, {$set, {title: "Dr"}}, {multi: true});
//different from sql
//single thread for update.  however ever write thread is carefully multi-task.  multi-update can affect 3 documents the pause to allow other operations.  "yielding"
//these are not isolated transactions.  they might yield then continue.  

//Quiz
db.scores.update({score:{$lt:70}}, {$inc: {score: 20}}, {multi: true});



//REMOVING DATA
db.people.remove(name: "Alice");//sort of like find // with no args it doesnt work Mongo 2.6+

db.people.remove({})//does work in a one by one sort of way
db.people.drop() // works faster to remove the whole collection.  meta data remains in remove but is deleted with drop
//drop then recreate indexes that must exist is a good method

//remove is like multi update and may yield

//QUIZ
db.scores.remove({score:{$lt:60}})

/**
NODE JS DRIVER
**/
//find, findOne, and cursors

//getting data in from json file
//mongoimport -d dbName -c collectionName filename.json
mongoimport -d course -c grades grades.json

mongo

use course

db.grades.find()

//app specific
var MongoClient = require('mongodb').MongoClient;
//connect to db but can also look for replica set or sharded cluster 
MongoClient.connect('mongodb://localhost:27012/course', function(err, db){
	if(err) throw err;
	var query = {'grade' : 100};
	db.collection('grades').findOne(query, function(err, doc){
		if(err) throw err;
		console.dir(doc);
		db.close():
	});
});
//end app specific

node app.js //will log one collection to client



//app specific
var MongoClient = require('mongodb').MongoClient;
//connect to db but can also look for replica set or sharded cluster 
MongoClient.connect('mongodb://localhost:27012/course', function(err, db){
	if(err) throw err;
	var query = {'grade' : 100};
	db.collection('grades').find(query).toArray(function(err, doc){
		if(err) throw err;
		console.dir(doc);
		db.close():
	});
});
//end app specific
node app.js //will log all as array to client


//app specific
var MongoClient = require('mongodb').MongoClient;
//connect to db but can also look for replica set or sharded cluster 
MongoClient.connect('mongodb://localhost:27012/course', function(err, db){
	if(err) throw err;
	var query = {'grade' : 100};
	var cursor = db.collection('grades').find(query);
	cursor.each(function(err, doc){
		if(err) throw err;
		
		if(doc == null){//end of cursor

			return db.close():
		}
		console.dir(doc.student + ' got a good grade!');
	});
});
//end app specific
//in the above the cursor is an object.  thequery doesn't happen until inside the each function.


//one more thing.
//making the cursor doesn't query the database
//on calling methods like .each() or .toArray is when cursor queries the db
//the response is not the entire result set...
//imagine a large db.  mongodb only returns in batch sizes, runs out and makes another request for another batch
//each does the callback at each each.  toarray only does the callback after building the whole array


//Node.js Drive: Using Field Projection

//app specific
var MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/course', function(err, db) {
    if(err) throw err;

    var query = { 'grade' : 100 };

    var projection = { 'student' : 1, '_id' : 0 };

    db.collection('grades').find(query, projection).toArray(function(err, docs) {
        if(err) throw err;

        docs.forEach(function (doc) {
            console.dir(doc);
            console.dir(doc.student + " got a good grade!");
        });

        db.close();
    });
});
//end app specific












//homework 2.2
db.data.find({}, {"State": true, "Temperature": true}).sort({"State": 1},{"Temperature": -1})
db.people.update({name: "Alice"}), {$inc: {age: 1}});


var students = db.grades.find( { 'type' : 'homework' }, { 'student_id' : 1, 'score' : 1, '_id' : 0}).sort({ 'student_id' : 1, 'score' : 1 })

// Create a variable to track student_id so we can detect when it changes
var id = "";

// Loop through our query results. Each document in the query is passed into a function as 'student'
students.forEach(function (student) { 
    if (id !== student.student_id) { 
        db.grades.remove(student)
        id = student.student_id; 
    }
});

db.students.find( { 'scores.type' : 'homework' }, { 'name' : 1, 'scores' : 1}).sort({ 'name' : 1, 'scores' : 1 })

// Create a variable to track student_id so we can detect when it changes
var id = "";

// Loop through our query results. Each document in the query is passed into a function as 'student'
students.forEach(function (student) { 
    if (id !== student._id) { 
    	if(student.scores.type === )
        db.students.remove(student)
        id = student._id; 
    }
});



{
	"name" : "Jenise Mcguffie",
	"scores" : [
		{
			"type" : "exam",
			"score" : 83.68438201130127
		},
		{
			"type" : "quiz",
			"score" : 73.79931763764928
		},
		{
			"type" : "homework",
			"score" : 89.57200947426745
		},
		{
			"type" : "homework",
			"score" : 45.23301509928876
		}
	]
}


db.students.aggregate(
   [
     {
       $group:
         {
           _id: "$item",
           minQuantity: { $min: "$quantity" }
         }
     }
   ]
)

//hw 3.1
x=db.students.aggregate( [{ "$unwind":"$scores" },{"$match":{"scores.type":"homework"}},{ "$group":{"_id":"$_id","minscore":{"$min":"$scores.score" }}} ] )

x.forEach(function(doc) {
   db.students.update( { "_id": doc._id }, 
                       { "$pull": { "scores" : { 
                                          "score":doc.minscore, 
                                          "type":"homework"
                       } } } 
   );
});