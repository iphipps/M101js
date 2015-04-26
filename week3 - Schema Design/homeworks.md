HOMEWORK: HOMEWORK 3.1

Download the students.json file from the Download Handout link and import it into your local Mongo instance with this command:
$ mongoimport -d school -c students < students.json
This dataset holds the same type of data as last week's grade collection, but it's modeled differently. You might want to start by inspecting it in the Mongo shell.

Write a program in the language of your choice that will remove the lowest homework score for each student. Since there is a single document for each student containing an array of scores, you will need to update the scores array and remove the homework.

Remember, just remove a homework score. Don't remove a quiz or an exam!

Hint/spoiler: With the new schema, this problem is a lot harder and that is sort of the point. One way is to find the lowest homework in code and then update the scores array with the low homework pruned.

To confirm you are on the right track, here are some queries to run after you process the data with the correct answer shown:

Let us count the number of students we have:

    use school
    db.students.count() 
200
Let's see what Tamika Schildgen's record looks like:
    db.students.find( { _id : 137 } ).pretty( )
    {
    	"_id" : 137,
    	"name" : "Tamika Schildgen",
    	"scores" : [
    		{
    			"type" : "exam",
    			"score" : 4.433956226109692
    		},
    		{
    			"type" : "quiz",
    			"score" : 65.50313785402548
    		},
    		{
    			"type" : "homework",
    			"score" : 89.5950384993947
    		}
    	]
    }
To verify that you have completed this task correctly, provide the identity (in the form of their _id) of the student with the highest average in the class with following query that uses the aggregation framework. The answer will appear in the _id field of the resulting document.

    db.students.aggregate( { '$unwind' : '$scores' } , { '$group' : { '_id' : '$_id' , 'average' : { $avg : '$scores.score' } } } , { '$sort' : { 'average' : -1 } } , { '$limit' : 1 } )


    ANSWER
    13

##HOMEWORK: HOMEWORK 3.2

If you have any difficulty using MongoProc, here are 2 video lectures showing how to set it up. 

Download MongoProc 

Making your blog accept posts
In this homework you will be enhancing the blog project to insert entries into the posts collection. After this, the blog will have the basic functionality. It will allow you to add blog posts with a title, body and tags and have it be added to the posts collection properly.

We have provided the code that creates users and allows you to login (the assignment from last week). Download and unpack the files for this homework from the Download Handout link.

We have removed parts of the code that uses the Node.js driver to query MongoDB from posts.js and marked the area where you need to work for hw3.2 with "hw3.2 TODO".

In a terminal: 

Linux/Mac:
    cd blog/ 
    grep -rn "hw3.2 TODO" *
Windows:
cd blog/ 
find /n "hw3.2 TODO" *
You should not need to touch any other code. The database call that you are going to add will insert a new post into the posts collection. Here is an example of valid blog post:

    db.posts.find().pretty()
    {
	    "_id" : ObjectId("513d396da0ee6e58987bae74"),
	    "title" : "Martians to use MongoDB",
	    "author" : "andrew",
	    "body" : "Representatives from the planet Mars announced today that the planet would adopt MongoDB as a planetary standard. Head Martian Flipblip said that MongoDB was the perfect tool to store the diversity of     life that exists on Mars.",
	    "permalink" : "martians_to_use_mongodb",
	    "tags" : [
		    "martians",
		    "seti",
		    "nosql",
		    "worlddomination"
	    ],
	    "comments" : [ ],
	    "date" : ISODate("2013-03-11T01:54:53.692Z")
    }
As a reminder, to run your blog, go into the blog directory and type:
    npm install
    node app.js
To play with the blog you can navigate to the following URLs:
http://localhost:8082/
http://localhost:8082/signup
http://localhost:8082/login
http://localhost:8082/newpost
Ok, now it’s time to validate you got it all working. 

From the top of this page, there was one additional program that should have been downloaded: mongoProc. 

With it, you can test your code and look at the Feedback section. When it says "user creation successful" and "user login successful", you can Turn in your assignment. 

You will see a message below about your number of submissions, but you must submit this assignment using MongoProc.


    ANSWER to be placed in appropriate spot
    posts.insert(post, function (err, result) {
        "use strict";
    
        if (err) return callback(err, null);
    
        console.log("Inserted new post");
        callback(err, permalink);
    });

##HOMEWORK: HOMEWORK 3.3 (MONGOPROC VERSION)

If you have any difficulty using MongoProc, here are 2 video lectures showing how to set it up. 

Download MongoProc 

Making your blog accept comments
In this homework you will add code to your blog so that it accepts comments. Download and unpack the files for this homework from the Download Handout link. You will be using the same code as you downloaded for homework 3.2, along with the changes you made.

We have removed parts of the code that uses the Node.js driver to query MongoDB from posts.js and marked the area where you need to work for HW 3.3 with "hw3.3 TODO".

In a terminal: 

Linux/Mac:
cd blog/ 
grep -rn "hw3.3 TODO" *
Windows:
cd blog/ 
find /n "hw3.3 TODO" *
You should not need to touch any other code. The database call that you are going to add will add a new comment to a given post.

This assignment has fairly little code, but it's a little more subtle than the previous assignment because you are going to be manipulating an array within the Mongo document. For the sake of clarity, here is a document out of the posts collection from a working project that also has comments.

    {
    	"_id" : ObjectId("513d396da0ee6e58987bae74"),
    	"author" : "andrew",
    	"body" : "Representatives from the planet Mars announced today that the planet would adopt MongoDB as     a planetary standard. Head Martian Flipblip said that MongoDB was the perfect tool to store the     diversity of life that exists on Mars.",
    	"comments" : [
    	{
    	"author" : "Larry Ellison",
    	"body" : "While I am deeply disappointed that Mars won't be standardizing on a relational database, I     understand their desire to adopt a more modern technology for the red planet.",
    	"email" : "larry@oracle.com"
    },
    {
    	"author" : "Salvatore Sanfilippo",
    	"body" : "This make no sense to me. Redis would have worked fine."
    }
    ],
    "date" : ISODate("2013-03-11T01:54:53.692Z"),
    "permalink" : "martians_to_use_mongodb",
    "tags" : [
    "martians",
    "seti",
    "nosql",
    "worlddomination"
    ],
    "title" : "Martians to use MongoDB"
    }
As a reminder, to run your blog, go into the blog directory and type:
npm install
node app.js
Note that you add comments in this blog from the blog post detail page, which appears at

http://localhost:8082/post/post_slug
where post_slug is the permalink. For the sake of eliminating doubt, the permalink for the example blog post above is
http://localhost:8082/post/martians_to_use_mongodb
Ok, now it’s time to validate you got it all working. 

From the top of this page, there was one additional program that should have been downloaded: mongoProc. 

With it, you can test your code and look at the Feedback section. When it says "user creation successful" and "user login successful", you can Turn in your assignment. 

You will see a message below about your number of submissions, but you must submit this assignment using MongoProc.

    ANSWER to be placed in the appropriate spot
    posts.update({'permalink': permalink}, {'$push': {'comments': comment}}, function(err, numModified) {
        "use strict";
    
        if (err) return callback(err, null);
    
        callback(err, numModified);
    });