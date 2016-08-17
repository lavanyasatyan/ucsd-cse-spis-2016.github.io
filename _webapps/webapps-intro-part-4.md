---
topic: "Webapps Intro, Part 4"
desc: ""
---

# Deploying an existing Flask App on Heroku

If you already have a Flask app running locally by just running it in IDLE, or at the command line (e.g. python hello.py), and you want to convert it to run on Heroku, you need to do three things.  Each is very simple.

* It needs to be in a git repository.
* You need a Procfile—this is usually just one line of code (see below.)
* You need a requirements.txt file.  This is generated automatically with one unix command (see below.)
* You use the command heroku create to set up a remote repository on heroku where you can deploy your application
* You use git push heroku master to deploy your application.
* You can see your application on the web, or use heroku logs to see the logs (if there are errors.)

(Note to instructors: The heroku create and heroku logs commands are provided by the "Heroku toolbelt", 
and it was installed especially for SPIS 2015.  It requires installation of Ruby, then it is a simple download.)

First, to deploy on Heroku, we need to create two extra files.
We need a file called Procfile in our git repo.  This file tells Heroku what to do with our github repo when we push it to github.  It should contain the following:
web: gunicorn hello:app --log-file=-
The part of this line that reads  hello:app assumes that the main python code for your web app is in hello.py, and that the variable app is the one that appears in the line of code app = Flask(__name__).

If that is not the case, you may need to adjust either hello or app as needed.

Now that we have that file, you will want to do these commands to commit this file to our github repo.
git add Procfile
git commit -m "Added Procfile needed by Heroku"
git push origin master
We also need a file called  requirements.txt which is a list of the Python modules that are needed for our Heroku flask application.   

This file will list all of the Python modules that we may have installed using 
pip install --user blah, including flask, and anything else that flask might have required.

Note that before you do the next step, you should do 
pip install --user gunicorn if you haven't already—while that isn't necessarily needed for running Flask applications locally, it is needed for Heroku.

We create the file requirements.txt with this command:


pip freeze > requirements.txt

We can see whether it worked by typing:

cat requirements.txt

It should look something like this (the exact list may differ, and the numbers may differ.  It is important that gunicorn is in the list though.)

[spis15t7@ieng6-240]:511$ cat requirements.txt 
Flask==0.10.1
itsdangerous==0.24
Jinja2==2.8
MarkupSafe==0.23
Werkzeug==0.10.4
wheel==0.24.0
gunicorn=19.3.0
[spis15t7@ieng6-240]:512$ 
So now lets push that to github as well:
git add requirements.txt
git commit -m "Added list of Python modules needed by Heroku"
git push origin master
Now do:

heroku create and notice the name of the application created 
(It will take the form word-word-number, e.g. flying-tomato-4321)

git push heroku master

To see your app, visit https://word-word-number.herokuapp.com, 
e.g. https://flying-tomato-4321.herokuapp.com

If there are errors, check them by typing heroku logs
A side note about that "itsdangerous" thing 
(you can skip reading this section if you want) 

When I first saw that name show up in the modules we were downloading, I was a little taken aback.
If you are worried about having something called "itsdangerous" in your account, this paragraph is to reassure you that its not dangerous. 

I read the documentation for the itsdangerous module and realized that that the only thing dangerous here was the name.   The name refers to the fact that sometimes data has to be passed from a "trusted environment" to an "untrusted environment" or vice-versa, and when that happens, you want to "sign" the data—that is, do some cryptography with it—to ensure that it isn't modified enroute.  There isn't anything "dangerous" about the software itself.  On the contrary—not using it would be dangerous.