# code-review-tutorial
In this exercise, I reviewed the code files in the repo above and provide a walkthrough tutorial here for a teachback on how this codebase functions:

## server.js

This is a file in the root folder and is the script you run to start up the server, so any issues here will mess up the flow of your app. Thus this file is a good place to start. 

Lines 1-6 list out our dependicies and use the 'require' function. Line 2, requires express framework on top of node and makes the coding the server processes a lot easy.  The next two requires modules are new to me:  

```
var session = require("express-session");
var passport = require("./config/passport");
```

Looking up the documentation on express-session I discovered it is a module that assigns every user a unique session that can store the user state. This is related to the next require for 'passport' on line 6, which is an authentication libary that allows for users to sign in. 

Lines 9-10 set up our port for our server, either hosted remotely using the "process.env.PORT' or falling back to a local deployment on port 8080. 

Lines 13-20 create the express app and configure middleware needed for authentication. This is typically starter code and reused for each work project. The `app.use(express.static("public"));` provides a public folder for general access if hosted in a cloud provider such as Heroku.

Lines 18-20 are related to the new authentication Passport module and keep track of the users's login status. 

Lines 23-24 require our our html and server routes stored in the "routes" folder. 

Lines 27-31 is starter code that syncs the database and displays a message to the user when they successfully start the server. In the console you run node server.js and hopefully you see the console log meessage on line 29 run in the terminal. 

**Decision point**

Looking at the server.js file overall there are a few places we could go next. Line 6 points us to the config folder to look at the passport file `var passport = require("./config/passport");` and lines 23-24 point us to the routes folder and a third pathway is line 10 which establishes a var db that points to the "models" folder. This tutorial will eventually get to all the files but here I'll start out by looking at the models folder.

## Models folder

Inside this folder we have two files index.js and user.js

*index.js*

Lines 3-5 require moduls file server ("fs"), path (for reading file paths) and establishing sequelize with will perform the function of our ORM and make writing SQL queries much easier. 

Line 7 establishes a var basename that points to `path.basename(module.filename);` which is a method that returns the filename part of a file path. We use it later on line 26 to check that .js file exists in the directory. 

Line 8-10 are shortcut varaibles for pointing to starter code for connecting to our database and extracting the details from the config.json file. The config.json file will have details such as databse name, username, and database password etc. Line 10 creates an empty object that we will use to create models off this starter template and then export for other files to use as writte down in line 43

Lines 14-19 Determines which database to use depending on whether we are deployed in the cloud or running a local instance. Denpending on the true/false of the conditional statement the `var sequelize = new Sequelize` will create a model either in the remote database or the local databse using the details form the config.json file. 

Line 22-31 use the file service module, which employes a filter to check that there is a .js file in the directory. For each file, sequelize will create a new model. 

*user.js*

This is a sequelize template for a User model. 

Line 2 we require the bcryptjs library for password hashing.

Line 4-25 we export our User model and create two colums (email and password) on lines 7 and 17. 

Line 9 `type: DataTypes.STRING` forces the datatype for email column (in line 7) to be a string
Line 10 `allowNull: false` prevents the column for email from being empty or null
Line 11 `unique: true` ensures that each email in the column is unique and not reused
Line 12-15 is a validator of the information the user entered `isEmail` check to make sure the user input matches the general email formate of name@provider.com.

line 23-25 Creates a custom method for User model that will check passwords from the user to what we have stored in the database.

Line 28-30 Hooks are automatic methods that run during various phases of the User Model lifecycle, in this case, before a User is created, we will automatically hash their password.

Line 31 Returns the user object for other files to use

## Routes folder

Within this folder we have two files api-routes.js and html-routes.js

*api-routes.js*
