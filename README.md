# Heroku Deployment

## Instruction

### Steps

1. Installing Heroku's CLI
1. Setting up the environment
2. Establishing Heroku pipelines
3. Pushing the App to Heroku
4. Deploying from Heroku
 
<hr>

>You will need to be familiar with setting up a standard git repository on Github.com. A tutorial can be found here in how to quickly setup a brand new git repository. 
>
>[Github Tutorial](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)


#### Installing Heroku's CLI

Head too ```https://devcenter.heroku.com/articles/heroku-cli``` to this address and follow the instructions for your OS.

Once you have it downloaded. You will need to head to heroku.com and create a new account. Once you have your credentials (Just user name and password, you can authenticate yourself on your computer so your heroku commands will interact with the heroku servers. Here is an example...

```shell
heroku login
Enter your Heroku credentials.
Email: dean@myawesomesitename.com
Password (typing will be hidden):
Authentication successful.
```

#### Setting up the environment

While in the folder of your project, after you have Heroku's CLI installed, throw his command.

```shell
heroku create
```

You will see something to the effect of..

```shell
Creating happy-dancing-871... done, stack is cedar-14
http://happy-dancing-871.herokuapp.com/ | https://git.heroku.com/happy-dancing-871.git
Git remote heroku added
```

This will tell you where the app is going to be finally deployed at on heroku as well as where the heroku git repository is. These names are randomly generated! This will also create the ability to ```git push heroku``` so you can put your code into the site directly. We will be linking our github account to the project however, so we won't need that!

We will also need to add a Procfile to our repository in the base directory. This will allow us to tell Heroku what to do when the project is established.

For node, our Procfile would look like...

```
web: node NAME_OF_SERVER_START_FILE.js
```

For Php, our Procfile would look like...

```
web: vendor/bin/heroku-php-apache2 PROJECT_ROOT_FOLDER/ 
```


We will also need to setup a composer.json or package.json to let it know what needs to be installed for this project, depending on Node or PHP!

##### composer.json

```json
{
  "require-dev": {
    "heroku/heroku-buildpack-php": "*"
  }
}
```

This will just install heroku's apache instance so you can run your project! Your php project will require a line to be added to its index file to make full use of these dependencies with a single include line.

```php
require('../vendor/autoload.php');
```


##### package.json

```json
{
  "name": "node-project-name",
  "version": "0.2.5",
  "description": "A Node.js app using Express 4",
  "engines": {
    "node": "5.9.1"
  },
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies":{
    "express": "4.13.3"
  },
    "keywords": [
    "node",
    "heroku",
    "express"
  ],
  "license": "MIT"
}
```

>The only thing this one HAS to have is a scripts section with the start condition. This is what Heroku will use to actually launch your appilcation. The rest will be standard package.json additions from using npm within that folder.

#### Establishing Heroku Pipelines


1) Head over to Heroku and log in on their front page. This will take you to their dashboard. You will see your application you created already on the server. This application will be blank so far because we haven't actually pushed anything to Heroku yet.

You will see a "new" dropdown on the right-hand side. Click that and click create new pipeline.
![Imgur](http://i.imgur.com/KYff7RF.png)

2) Type in the name of your pipe-line. They must contain only lower case letters. Once you have completed that, go ahead and connect this pipe-line to the repository. We created earlier. 

![Imgur](http://i.imgur.com/SKINUWQ.png)

3) Go back to your "personal Apps" now and click on the newly created application. Find the "deploy" tab and bring it up. Search then on that page for the ability to  "Add this app to a pipeline". Select the proper pipeline you created above in order to assign that application to the pipeline.

![Imgur](http://i.imgur.com/RTtgrWM.png)

4) This will then take you to the pipe-line page. From here you can move the server you've created around. Because we don't have multiple server created yet. It will set itself up as a production server. We can go back into the local branch of our terminal and execute ```heroku create``` again should we desire to add another server using that same configuration to our heroku applications. From there we can continue to add servers or arrange them in the pipeline. Its recommended to have atleast a staging server and a production server, though you can also add a development server as well!

5) Finally we need to link our staging server to a branch on the github repo. I recommend linking master to it or creating a new special branch such as release. Whatever you decide to link to it. The server will automatically pull down that new build each time you push that branch up to github. You can do this from the pipe-line page....

![Imgur](http://i.imgur.com/nr6EmPo.png)



   or the individual application pages from your dashboard.



![Imgur](http://i.imgur.com/TLjIiox.png)

#### Pushing the App to Heroku


Now that we have linked our staging server to the github repo we want to push forward too. We simply need to make a push directly to the master branch for our server to hook into git. Once this occurs the server will automatically pull down the new build, setup the server once more for your code and attempt to run it.

```shell
git push origin master
```


#### Deploying from Heroku

Once you have gotten a chance to examine your code on your staging server and are happy with its outcome. You need only to click "Promote to production..." This will automatically push it to your production server which will go down for a moment while it rerenders everything with your changes.

![Imgur](http://i.imgur.com/rSXCK7F.png)
