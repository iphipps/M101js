##1.1
Use mongorestore to restore the dump into your running mongod. Do this by opening a terminal window (mac) or cmd window (windows) and navigating to the directory so that the dump directory is directly beneath you. Now type:



######mongorestore dump

    use m101
    db.hw1_1.findOne()

//Answer// Hello from MongoDB!

##1.2
Run app to find the console log

    cd hw1-2

    mongorestore dump

    npm install mongodb

    node app.js

//Answer//I like Kittens

##1.3
If you have all the dependencies installed correctly, this will print the message 'Express server started on port 8080'. Navigate to 'localhost:8080' in a browser and write the text that is displayed on that page in the text box below.


//Answer//Hello, Agent 007.