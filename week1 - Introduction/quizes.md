Quiz: What is MongoDB?

Which of the following statements are true about MongoDB?

[x]: MongoDB is schemaless.
[ ]: MongoDB supports SQL.
[x]: MongoDB is document oriented.
[ ]: MongoDB supports Joins.

Quiz: MongoDB Relative to Relational 

Which features did MongoDB omit in order to retain scalability?

[x]: Joins
[x]: Transactions across multiple collections
[ ]: Secondary Indexes
[ ]: Indexes


Quiz: Quick Introduction to the Mongo Shell 

Which of the following expressions are valid JSON documents for MongoDB?

Remember, MongoDB doesn`t require quotation marks around keys, as they must always be strings.


[(X)]: { a : 1, b : 2, c : 3 }
[( )]: { a,1; b, 4, c, 6}
[( )]: { a : 1; b : 1; c : 4 }
[( )]: ( A, 1; b : 2; c, 4 }


QUIZ: INTRODUCTION TO JSON

Which of the following are JSON documents that the MongoDB shell will accept?


[X]: { a : 1, b : 2, c : 3 }
[X]: { a : 1, b : 2, c : [ 1, 2, 3, 4, 5 ] }
[X]: { a : 1, b : { }, c : [ { a : 1, b : 2 }, 5, 6 ] }
[X]: { }



QUIZ: JSON REVISITED

Write the JSON for a simple document containing a single key "fruit" that has as its value an array containing three strings: "apple", "pear", and "peach"

Note that this isn't a question about the mongo shell; you'll need to put quotation marks around your key.

{
    fruit: ["apple", "pear", "peach" ]
}

QUIZ: JSON SUBDOCUMENTS

Write a JSON document with a single key, "address" that has as it value another document with the keys “street_address”, “city”, “state”, “zipcode”, with the following values: “street_address” is "23 Elm Drive", “city” is "Palo Alto", “state” is "California", “zipcode” is "94305"

{
    address: {
        street_address: "23 Elm Drive",
        city: "Palo Alto",
        state: "California",
        zipcode: "94305"
    }
}

QUIZ: BLOG IN RELATIONAL TABLES

let’s assume that our blog can be modeled with the following relational tables:
authors:
	author_id,
	name,
	email,
	password

posts:
	post_id,
	author_id
	title,
	body,	
	publication_date

comments:
	comment_id,
	name, 
	email,
	comment_text

post_comments:
	post_id,
	comment_id


tags
	tag_id
	name

post_tags
	post_id
	tag_id
In order to display a blog post with its comments and tags, how many tables will need to be accessed?

[( )]: 2
[( )]: 3
[( )]: 5
[(X)]: 6



QUIZ: BLOG IN DOCUMENTS

Given the document schema that we proposed for the blog, how many collections would we need to access to display the blog home page?

[( )]: 0
[(X)]: 1
[( )]: 2
[( )]: 4

QUIZ: INTRODUCTION TO SCHEMA DESIGN

In which scenario is it impossible to embed data within a document (you must put the data in it a separate collection)?


[( )]: The data would be duplicated across multiple objects within a collection.
[( )]: You need an index on the data element.
[(X)]: The embedded data could exceed the 16MB document limit within MongoDB.
[( )]: The data is not isomorphic.