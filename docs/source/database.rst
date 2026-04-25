Database
========

the database handles the storing of long term information from user accounts to questions and feedback, the database is written in sqlite.

ERD 
---


Here you can see an overview of the design for the database we have 4 tables which each store information, an accounts table to store account data for users, a questions table to store questions, topics to group the questions by and a feedback table to store all the users answers for any quizzes the user has participated in. 

ACCOUNTS TABLE 
--------------
 

This table holds the accounts, accounts have an ID, username, password and an experience counter.  

ACC_username is constrained as unique, this was done so that we could search for a user by their name allowing for an easier time finding the a user in the database. 

ACC_password has a number of constraints which collectively make it so that the password needs letters and numbers and capitals to be valid. 

ACC_exp is currently serving no purpose, it was added originally to match the initial scope of the edulearn project so that accounts could have progression but now it serves no purpose. 

FEEDBACK/QUIZ TABLE 
-------------------

The feedback table intersects between the accounts and questions table, it exists to store the results of a users quiz for the purposes of feedback. 

The table uses a composite key with FEED_ID, QUE_ID and ACC_ID. ACC_ID points to which user the feedback regards, the FEED_ID differentiates each quiz/feedback object and the QUE_ID refers to which question the entry regards. 

FEED_answer status stores if the question was answered correctly while FEED_user_answer_index stores which option the user chose in the quiz, these are stored so that we can reconstruct the quiz later so that the user can see what they got right/wrong. 

QUESTIONS TABLE 
---------------

The questions table stores the sum of all the questions in the database, each question is grouped by topic, hence the TOP_ID. 

QUE_question stores the question text. 

QUE_ans_1-4 stores each of the 4 answers texts. 

QUE_ans_correct stores which answer is correct, it is constrained to only accept an index of 0-3 matching the 4 possible answers. 

TOPIC TABLE 
-----------


As mentioned above each question is grouped by a topic (maths, physics, etc) in the topic table we have an ID for the primary key and a unique TOP_name which denotes the topic in question. 
