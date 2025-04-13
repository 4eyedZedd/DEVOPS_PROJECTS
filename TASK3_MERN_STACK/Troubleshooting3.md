
## Challenge with connecting to Mongoose DB 

I experienced challenges with connecting to the mongoose DB I created for a coupld of reasons;

- I copied the connectin string from the setup prompts; `DB = 'mongodb+srv://User01:<db_password>@emma-mongo.tkxsfmb.mongodb.net/?retryWrites=true&w=majority&appName=Emma-Mongo'` and in trying to replace <db_password> with the correct password, I kept typing the actual password into the anchor brackets like this <actual_password> instead of using '...:actual_password@...'. I was getting 'bad authentication error' when trying to run index file to connect to the database. 

- I discovered and corrected this this error, however I ran into sonnection string error and this was because I did not defind the parameter.  ![alt text](<Images/Mongoose DB connection string error.png>).

- I defined the parameter correctly in single quotes and the connection worked.