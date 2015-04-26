##HOMEWORK: HOMEWORK 2.1

In this problem, you will be using an old weather dataset. Download weather_data.csv from the Download Handout link. This is a comma separated value file that you can import into MongoDB as follows:

    mongoimport --type csv --headerline weather_data.csv -d weather -c data

You can verify that you've imported the data correctly by running the following commands in the mongo shell:

    use weather
    db.data.find().count()
    2963

Reading clockwise from true north, the wind direction is measured by degrees around the compass up to 360 degrees. 

90 is East

180 is South

270 is West

360 is North

Your assignment is to figure out the "State" that recorded the lowest "Temperature" when the wind was coming from the west ("Wind Direction" between 180 and 360). Please enter the name of the state that meets this requirement. Do not include the surrounding quotes in providing your answer.

    New Mexico





##HOMEWORK: HOMEWORK 2.2

Download MongoProcLinux users not using the Ubuntu 12.04 binaries need to have python 2.7 installed and the pymongo module installed. They can also choose to to point mongoProc to any 2.7 compatible python implementation (like PyPy) in user_settings by changing the python field.
Write a program that finds the document with the highest recorded temperature for each state, and adds a "month_high" field for that document, setting its value to true. Use the weather dataset that you imported in HW 2.1. 

This is a sample document from the weather data collection:
    use weather
switched to db weather
    db.data.findOne()
{
    "_id" : ObjectId("520bea012ab230549e749cff"),
    "Day" : 1,
    "Time" : 54,
    "State" : "Vermont",
    "Airport" : "BTV",
    "Temperature" : 39,
    "Humidity" : 57,
    "Wind Speed" : 6,
    "Wind Direction" : 170,
    "Station Pressure" : 29.6,
    "Sea Level Pressure" : 150
}
Assuming this document has the highest "Temperature" for the "State" of "Vermont" in our collection, the document should look like this after you run your program:
db.data.findOne({ "_id" : ObjectId("520bea012ab230549e749cff") })
{
    "_id" : ObjectId("520bea012ab230549e749cff"),
    "Day" : 1,
    "Time" : 54,
    "State" : "Vermont",
    "Airport" : "BTV",
    "Temperature" : 39,
    "Humidity" : 57,
    "Wind Speed" : 6,
    "Wind Direction" : 170,
    "Station Pressure" : 29.6,
    "Sea Level Pressure" : 150,
    "month_high" : true
}
Note that this is only an example and not the actual document that you would be updating. Note also that our collection only has one month of data for each "State", which is why we are asking you to set "month_high".

Hint: If you select all the weather documents, you can sort first by state, then by temperature. Then you can iterate through the documents and know that whenever the state changes you have reached the highest temperature for that state. 

From the top of this page, there was one additional program that should have been downloaded: mongoProc. 

With it, you can test your code and look at the Feedback section. When it says "Correct amount of data documents.", "Correct amount of month_high documents." and "Correct month_high documents.", you can Turn in your assignment. 

You will see a message below about your number of submissions, but you must submit this assignment using MongoProc.

    ANSWER
    db.data.find({}, {"State": true, "Temperature": true}).sort({"State": 1},{"Temperature": -1})


##HOMEWORK: HOMEWORK 2.3

If you have any difficulty using MongoProc, here are 2 video lectures showing how to set it up. 

Download MongoProc 

Blog User Sign-up and Login 

Download hw2-3.zip from the Download Handout link and uncompress it. 

You should see four files in the 'blog' directory: app.js, users.js, posts.js and sessions.js. There is also a 'views' directory which contains the templates for the project and a 'routes' directory which contains our express routes. 

If everything is working properly, you should be able to start the blog by typing: 
    npm install
    node app.js

Note that this requires Node.js to be correctly installed on your computer.
After you run the blog, you should see the message: 
Express server listening on port 8082

If you goto http://localhost:8082 you should see the front page of the blog. Here are some URLs that must work when you are done. 

http://localhost:8082/signup
http://localhost:8082/login
http://localhost:8082/logout

When you login or sign-up, the blog will redirect to http://localhost:8082/welcome and that must work properly, welcoming the user by username. 

We have removed parts of the code that uses the Node.js driver to query MongoDB from users.js and marked the area where you need to work with "TODO: hw2.3". You should not need to touch any other code. The database calls that you are going to add will add a new user upon sign-up and validate a login by retrieving the right user document. 

The blog stores its data in the blog database in two collections, users and sessions. Here are two example docs for a username ‘sverch’ with password ‘salty’. You can insert these if you like, but you don’t need to. 
    use blog
switched to db blog
    db.users.find()
{ "_id" : "sverch", "password" : "$2a$10$wl4bNB/5CqwWx4bB66PoQ.lmYvxUHigM1ehljyWQBupen3uCcldoW" }
    db.sessions.find()
{ "username" : "sverch", "_id" : "8d25917b27e4dc170d32491c6247aabba7598533" }
    
Once you have the the project working, the following steps should work: 
go to http://localhost:8082/signup
create a user

It should redirect you to the welcome page and say: welcome username, where username is the user you signed up with. Now: 
    Goto http://localhost:8082/logout
    Now login http://localhost:8082/login

Ok, now it’s time to validate you got it all working. 

From the top of this page, there was one additional program that should have been downloaded: mongoProc. 

With it, you can test your code and look at the Feedback section. When it says "user creation successful" and "user login successful", you can Turn in your assignment. 

You will see a message below about your number of submissions, but you must submit this assignment using MongoProc.



    ANSWER
    users.findOne({ '_id' : username }, validateUserDoc);











