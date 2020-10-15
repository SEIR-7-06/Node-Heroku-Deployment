## Getting set up on Heroku with Node + Mongoose

### Before you do anything
1) Stop and commit. Make sure your app is under version control with `git`.  If you're not sure whether your project is under version control yet, you definitely haven't been commiting often enough! But run `git status` to check if your project directory is a repo and `git init` to make it into one if necessary. __Stop and commit your changes.__

In order to deploy your applications you will be deploying your MongoDB database to a service called [Atlas](https://www.mongodb.com/cloud/atlas)
  - To Setup your DB on Atlas, please refer to this [guide](https://git.generalassemb.ly/la-seir-9-8/atlas-hosted-mongodb)
  - We recommend following the above guide first to set up Atlas before continuing on to deploy Heroku.

1) We'll be deploying our Node.js code with a service called Heroku that allows hosting for fullstack applications. Once you've setup your DB with Atlas via the README linked above, sign up for an account with heroku: https://www.heroku.com/

2) Install the heroku cli
https://devcenter.heroku.com/articles/heroku-cli#download-and-install

3) Once installed, you can use the heroku command from your command shell.
Log in using the email address and password you used when creating your Heroku account:

```bash
heroku login
# Enter your Heroku credentials.
# Email: zeke@example.com
# Password:
# ...
```

Authenticating is required to allow both the heroku and git commands to operate.

**(NOTE YOUR PROJECT MUST BE A GIT REPO TO CONTINUE.)**.

### Preparing for Heroku Deploy

4) Add a new remote to your project that points to Heroku's servers:

```bash
    heroku create
```
Check to see the new remote created in your repo
```bash
    git remote -v
```

5) At this point you should be able to log into Heroku, see your dashboard, and see the application that you created when you ran `heroku create`.

Click on the Application, it might have a goofy name like "aqueous-chamber-05556". From there go to the settings tab and scroll down to the section on Config Vars. Click "Reveal Config Vars". Here is were you are going to add the environment variables that you added to your `.env` file when you were dev'ing locally.

- For the first Key add `MONGODB_URI`. For the value field you'll want to grab your connection string for your Atlas Cluster that you created earlier. See [Atlas](https://www.mongodb.com/cloud/atlas) setup docs for how to get your connection string.
- For the second Key add `SESSION_SECRET` just as we have it in our `.env` file and pass the same value in the value field.

6) Next lets move back to the command line and our code editor and make sure we are ready to deploy.

STOP AND COMMIT!

### Setup `.gitignore` (Recommended)
From inside your project directroy, run:
```bash
touch .gitignore
```

Next, open `.gitignore` in your text editor, and add the following line:
```
node_modules
```

Finally, run the following command to untrack your `node_modules` folder (it's not *your* code, so we don't want to track it!):
```bash
git rm -r --cached node_modules
```

STOP AND COMMIT!

### Confirm your Dependencies

7) Double check your `package.json` to make sure that all your depenedencies are present. If something is missing install it.

For example, here are a bunch of common dependencies (*DO NOT COPY*):  
``` javascript
    {
      "dependencies": {
        "body-parser": "^1.13.3",
        "bower": "^1.5.2",
        "express": "^4.13.3",
        "express-session": "^1.11.3",
        "method-override": "^2.3.5",
        "mongoose": "^4.1.5"
      }
    }
```

If your `package.json` is missing any dependencies, you will need to both `install` and `--save` the package. For example, if you notice that the `mongoose` package is missing from your `package.json` you would need to run:

```bash
    npm install --save mongoose
```

### Add a start script
10) Add a `start` script for your application in your `package.json`:

```javascript
...
  "scripts": {
    "start": "node server.js",
    "postinstall": "bower install"   // only if you're using Bower
   }
...
```

This is assuming your main application file is called `server.js`. If your main file is called something else, adjust the script to use your file name.

### Deploy!

8) Hold your horses! We've made a lot of changes -- let's STOP AND COMMIT!
``` bash
    git add . -A
    git commit -m "ready for heroku deploy attempt #1"
```

9) Now we can deploy:
``` bash
    git push heroku master
```

13) If you missed a step just ask for help. Otherwise you should be able to visit your application by saying the following:

```bash
    heroku open
```

14) If you have a seed task, you can run it now (assuming everything else is working):

``` bash
heroku run bash
node seed.js
```

## Debugging Tips

Here are some helpful commands for debugging your application on Heroku:

#### `heroku logs`
This command lists your most recent application server logs. Helpful for figuring out why your application may be crashing and burning.

#### `heroku run bash`
This command allows you to run terminal _on Heroku's servers_. This is a handy way for us to poke around and run commands on our application (like seeding the database, and checking that everything was installed correctly).
