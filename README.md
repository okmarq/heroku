# A how-to steps to deploy a laravel 8 application to heroku with a database

This tutorial will enable you deploy your laravel 8 application with a MySQL database to github and heroku, saving you the time to deal with subsequent errors you might encounter if you were to follow ther usual tutorials. as they skip over some little however important detail. that decides the success of your deployment.

I will assume you already have the laravel installer and composer installed on your machine as well as a web server like xampp, then you should also have a github account and git installed, with it comes the bash cli which i recommend you use as your cli. and finally have a heroku account then install the heroku cli. they are the prerequisites for this given tutorial.

1	in your command line, run "laravel new heroku --jet" and follow the command prompt.

	$ laravel new heroku --jet

	a	choose your desired templating engine e.g livewire or inertia
	b	choose if you will be using a team or no team

2	if you chose inertia then npm install && npm run dev will be done automatically for you so you can skip it else you run the command below one after the other has been completed.

	$ npm install
	$ npm run dev

3 next create a database with a name in the phpmyadmin panel that corresponds to your app name in my case it will be "heroku" then run the command below, this will migrate all your base database information. without a database laravel applications wont run which is one of the main reasons laravel applications are difficult to deploy and setup on heroku

	$ php artisan migrate

by now you should see your laravel application active on your local development.
now it is time to deploy your project on github then subsequenty on heroku.

4 the first step is to initialize your project environment with the command below

	$ git init

5 next add all your files using the following command "git add ." or "git add file.ext" if an individual file {read best practices on how to use git commands}

	$ git add .

6 you need to commit your changes next with the command below

	$ git commit -m "first commit"

7 add a branch to your local project

	$ git branch -M main

8 if you havent already created a repository on GitHub then now would be the best time [actually the last time] to do so to move forward, when youre done you copy the repo link then run the command like below [note: i use ssh so your link might be different. my apologies, i wont be covering the differences from here]

	$ git remote add origin git@github.com:okmarq/heroku.git

9 then upload your code to github with the comman below

	$ git push -u origin main

at this point we are ready to initialize our heroku environment for deployment.

10 the first step here is to login to heroku via your cli with the command below, follow the given instructions in the command prompt

	$ heroku login

11 next you create a heroku app with your project name like below or leave it blank for heroku to give you a randomly generated project name [your project name should be unique or it will be rejected]

	$ heroku create heroku

12 your next step is to your code to heroku where the build pack will be automatically detected and installed for you. in our case php

	$ git push heroku main

opening the app on heroku at this stage will give us a forbidden output as we dont have a procfile.
13 in this case you add a Procfile with no extension with the following line for an apache server "web: vendor/bin/heroku-php-apache2 public/" stage, commit and push the changes to heroku. now you should see a "HTTP 500 Internal Server Error".
you should take note of the "public" in the procfile, heroku might give you "web" which is just a generic indication, this will lead you to see an application error which from here on out, any error other will now begin to look painful to fix which is the very reason for this tutorial itself so follow it closely.

14 add a database from your heroku dashboard. specifically, use a JawsDB MySQL addon. the reason for this is so you can have easy access to all the separate parameters we will be needing to connect to the database, the Host, Username, Password, Port and Database.

15 your next step is to use your cli to add the following parameters to your heroku config vars. run the following commands below one after the other has completed

	$ heroku config:set APP_NAME=apoku

	$ heroku config:set APP_KEY=base64:138V/prcrPWnpMdyl9kPmRPug4s133odCrWuFl/KOas=

	$ heroku config:set APP_DEBUG=true

	$ heroku config:set APP_URL=http://apoku.test

	$ heroku config:set DB_CONNECTION=mysql

	$ heroku config:set DB_HOST=j21q532mu148i8ms.cbetxkdyhwsb.us-east-1.rds.amazonaws.com

	$ heroku config:set DB_PORT=3306

	$ heroku config:set DB_DATABASE=kvqb74eik8x9hi68

	$ heroku config:set DB_USERNAME=hksxxqiwg639dry3

	$ heroku config:set DB_PASSWORD=oczq10yi11woi5e9

you're almost there.
16 your next step is to migrate your database. while i'll be using the cli to do this step, you can do this directly from your heroku dashboard.

	$ heroku run bash

upon running the above command, a bash interface will be activated after which you will run the following command in it.

	$php artisan migrate

very nice, running a bash interface from a bash cli. cool huh?
well, congratulations on making it this far, you should pat yourself on the back, your application should up and running right now, assuming you followed the instructions. if you still have an error and you are using a laravel application, then read through again, to ensure you used "public" and not "web" in your Procfile.