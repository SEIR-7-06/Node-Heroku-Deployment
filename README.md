## Getting set up on Heroku with Node + Mongoose

### Before you do anything
1) Stop and commit. Make sure your app is under version control with `git`.  If you're not sure whether your project is under version control yet, you definitely haven't been commiting often enough! But run `git status` to check if your project directory is a repo and `git init` to make it into one if necessary. __Stop and commit your changes.__

2) Sign up for an account with heroku: https://www.heroku.com/

3) Install the heroku cli
https://devcenter.heroku.com/articles/heroku-cli#download-and-install

Once installed, you can use the heroku command from your command shell.
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

5) In your `server.js` file, modify `app.listen` to use `process.env.PORT` (this will be set, dynamically, by Heroku):

```javascript
    app.listen(process.env.PORT || 3000)
```

6) As of 11/10/20, Heroku, the application hosting service we deploy our projects to, will no longer offer a free option for hosting a MongoDB instance. Please refer to this [guide](https://git.generalassemb.ly/la-seir-9-8/atlas-hosted-mongodb), to deploy your MongoDB instance to Atlas.

7) Update your database connection to point to Heroku's database. Open `models/index.js` and add the following to the `mongoose.connect` method:

```javascript
    mongoose.connect( process.env.MONGODB_URI || "YOUR CURRENT LOCALHOST DB CONNECTION STRING HERE" );
```

##### Deprecated mLab Mongodb Deploy Steps
~~6) Tell heroku to use the mongolab addon. In your terminal, run:~~

~~heroku addons:create mongolab~~

~~At this point, the command line will probably ask you to enter a credit card number. Follow the prompt.~~

~~Heroku is a "freemium" service. Be careful! They will charge you if you exceed their data limits -- but our projects are tiny so we don't expect to get a lot of traffic!~~

~~7) **Patience...**  If you had to enter in a credit card, you can run `heroku addons` to check/confirm that mongolab was addded. __You may need to wait a few minutes for mogolab to become active.__~~

Congrats! Your application knows what port to run on, and what database to connect to - you're almost all set up to work in "production" on Heroku's servers!

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

9) Double check your `package.json` to make sure that all your depenedencies are present. If something is missing install it.

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

<!-- 
### Add a start script
10) Add a `start` script for your application in your `package.json`:

```javascript
...
  "scripts": {
    "start": "node index.js",
    "postinstall": "bower install"   // only if you're using Bower
   }
...
```

This is assuming your main application file is called `index.js`. If your main file is called something else, adjust the script to use your file name.

### Add a Procfile
11) Create a `Procfile` so that Heroku knows how to run your application:
    - Make sure you are in your main project directory (the same directory as `index.js`). Then run:  
``` bash
    touch Procfile
    echo "web: npm start" >> Procfile
```
-->

### Deploy!

10) Hold your horses! We've made a lot of changes -- let's STOP AND COMMIT!
``` bash
    git add . -A
    git commit -m "ready for heroku deploy attempt #1"
```

11) Now we can deploy:
``` bash
    git push heroku master
```

12) Set any ENV Variables that you're app is expecting. (Check your `.env`, e.g., MONGODB_URI).

**View current config var values**
```bash
heroku config
#GITHUB_USERNAME: joesmith
#OTHER_VAR:    production

heroku config:get GITHUB_USERNAME
#joesmith
```

**Set a config var**
```bash
heroku config:set GITHUB_USERNAME=joesmith
#Adding config vars and restarting myapp... done, v12
#GITHUB_USERNAME: joesmith
```

**Remove a config var**
```bash
heroku config:unset GITHUB_USERNAME
#Unsetting GITHUB_USERNAME and restarting myapp... done, v13
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
