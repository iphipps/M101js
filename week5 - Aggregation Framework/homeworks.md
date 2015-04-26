### 5.1
#####Use the aggregation framework in the web shell to calculate the author with the greatest number of comments. 


    db.posts.aggregate( [ 
    	{ $unwind: "$comments" }, 
    	{ $group: { _id:"$comments.author", num_comments: { $sum:1 } } }, 
    	{ $sort: { num_comments: -1 } }, { $limit: 1 } 
    ] )
//Gisela Levin

###5.2
#####Please calculate the average population of cities in California (abbreviation CA) and New York (NY) (taken together) with populations over 25,000. 
For this problem, assume that a city name that appears in more than one state represents two separate cities. 


    mongoimport -d test -c zips --drop small_zips.json
    
    db.zips.aggregate( [ 
    	{ $match: { state: { $in: ['CA','NY'] } } }, 
    	{ $group: { _id: { state: "$state", city: "$city" }, pop: { $sum: "$pop" } } }, 
    	{ $match: { pop: { $gt: 25000 } } }, 
    	{ $group: { _id: null, avg_pop: { $avg: "$pop" } } } 
    ] )
//44805

###5.3
#####Your task is to calculate the class with the best average student performance. This involves calculating an average for each student in each class of all non-quiz assessments and then averaging those numbers to get a class average. To be clear, each student's average includes only exams and homework grades. Don't include their quiz scores in the calculation. 
What is the class_id which has the highest average student performance? 

    db.grades.aggregate( [ 
    	{ $unwind: "$scores" }, 
    	{ $match: { "scores.type": { $nin: ["quiz"] } } }, 
    	{ $group: { 
    			_id: { class:"$class_id", student:"$student_id" }, 
    			avg_student_score: { $avg: "$scores.score" } 
    		} 
    	}, 
    	{ $group: { 
    			_id: "$_id.class", 
    			class_avg: { $avg: "$avg_student_score" } 
    		} 
    	}, 
    	{ $sort: { class_avg: -1 } }, { $limit: 1 } 
    ] )

//1

###5.4
#####Using the aggregation framework, calculate the sum total of people who are living in a zip code where the city starts with a digit. Choose the answer below.



    mongoimport -d test -c zips --drop five-four.json


    db.zips.aggregate( [ 
    	{ 
    		$project: { 
    				_id: 0, 
    				zipcode: "$_id", 
    				first_char: { $substr: [ "$city", 0, 1 ] }, 
    				pop: "$pop" 
    			} 
    	}, 
    	{ 
    		$match: { first_char: { $gte: "0", $lte: "9" } } 
    	}, 
    	{ 
    		$group: { _id: null, total_pop: { $sum: "$pop" } } 
    	} 
    ] )
//298015
