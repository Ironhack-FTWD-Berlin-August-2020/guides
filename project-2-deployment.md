# Deploy on Heroku

Start by signing up on [Heroku](https://www.heroku.com)

Select _Create a new app_ and in the next screen, choose the name of your app.

It will be the name used in the URL to access your app, so choose wisely!

Also, select _Europe_ as a region.




##  MongoDB Atlas configuration 

<br>



https://www.mongodb.com/atlas-signup-from-mlab



<br>

### **Sign Up & Create a Free Cluster** 

1. Sign Up for Mongo Atlas - enter the email, password, first name, last name
2. Select  >>>  **Shared Clusters**
3. In the drop-down **Cloud Provider & Region** select: 
   -  Europe Region with "**FREE TIER AVAILABLE**" flag
4. In the drop-down "**Cluster Tier**" make sure to select:
   - **M0 Sandbox**
5. Click the button **Create Cluster**



<br>


### Setup MongoDB Atlas Cluster:


1. In the sidebar select **SECURITY  ->  Database Access**

   <br>

2. Click on **<u>+ ADD NEW USER</u>** to create a new user

   - Select **Read and write to any database** - User Privileges
   - Set the **Username** and **Password**
   - Create New User

   <br>

3. In the sidebar select **SECURITY  ->  Network Access**

   <br>

4. Click on **<u>+ ADD IP ADDRESS</u>**

   - Click on **ALLOW ACCESS FROM ANYWHERE**
   - Confirm

   <br>

5. Connect To Your Cluster: Click on the gray button **CONNECT**

   - Select **Connect Your Application** option

   - Choose Your driver version:  **DRIVER**: Node.js

   - **Copy** the **Connection String Only**

### Add the Environment variables to heroku and update the monogoose connection in app.js  

You need to make sure that you have all the variables that are in your .env are also saved in Heroku, in the _Config Vars_ section of the _Settings_ tab.

So click on the _Settings_ tab and then on the _RevealConfigVars_ tab. There you enter the configuration variables.

When you are running your app in development it is localhost, in production it should be the address that you can get by clicking on the _Settings_ tab and then _Reveal Config Vars_.

You can see there is already a MONGODB_URI variable configured, so just add this to your mongoose connection link wherever you need it in your project:

The MONGODB_URI variable on heroku will look something like this:

You will have to replace the database name and the password
```
mongodb+srv://fullstacksteve:<password>@ironhack.5tfbhz.mongodb.net/<dbname>?retryWrites=true&w=majority
```

And in your project make this change to the connection in app.js:

Commit the changes and push to the master branch, this should deploy again.

```js
// app.js
//
mongoose
  .connect(process.env.MONGODB_URI || 'mongodb://localhost/<Name of your project>' ...)
//
```

(without the ... of course...)


<br>


### Deploy from GitHub
- Now, click on the _Deploy_ tab and _Connect to GitHub_.

- Select your repo from Github (make sure that your package.json is at the root of your repository) and enable _Automatic Deploys_.

- For the first time, select the manual deploy to deploy your master branch.

- If there is an error, select _More_ > _View Logs_.



## Connect to a GUI with Compass 

Start by copying the MONGODB_URI config variable from the *Settings* tab and then open Compass. You should get a prompt asking if you want to use the URI in your clipboard. Accept and then change the following field: *authentication database* which is set as `admin` and use the value in the *username* field instead.




### Optional: The Heroku Command Line Interface (CLI)

#### If you want to deploy directly to heroku via git or run a script on the server you can also use the Heroku CLI


## Install Heroku CLI


#### [LINK: Docs - Install the Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#npm)


### If unable to install the CLI via the Standalone installation (using a file) :

```bash
npm install -g heroku


heroku --version

# heroku/7.0.0 (darwin-x64) node-v8.0.0


heroku login
```


<br>



### Deploying with the CLI



While in the server root directory, in the terminal run :



```bash
# Commit the most recent work and merge it to the master branch
git add .
git commit -m 'Update xyz.js with new middleware'
git push origin <branch-name>


# Checkout to the master branch
git checkout master


# Add heroku remote
heroku git:remote -a <name-of-the-newly-created-app>


# Check the remotes available
git remote -v


# Make some changes to the code and deploy them to Heroku using Git.
git add .

git commit -m 'Make it better'

git push heroku master
```





<br>

## Heroku commands


#### To fetch your app’s most recent logs, use the `heroku logs` command:

```bash
heroku logs
```

The `logs` command retrieves 100 log lines by default. 

#### You can specify the number of log lines to retrieve (up to a maximum of 1,500 lines) by using the `--num` (or `-n`) option.

```bash
heroku logs -n 200

```


### [Real-time tail](https://devcenter.heroku.com/articles/logging#real-time-tail)

#### Real-time tail displays recent logs and leaves the session open for real-time logs to stream in. 

The name of your app can be found on heroku under settings

```bash
heroku logs --tail --app <name of your app>

```

## Executing scripts on the server - e.g. running a seed file


### [heroku run bash](https://devcenter.heroku.com/articles/heroku-cli-commands#heroku-run)

We can open the terminal instance on the Heroku container ([dyno](https://www.heroku.com/dynos)) in order to run custom scripts or see the files included in the instance. 

```bash
# Open the terminal in the app dyno in Heroku
heroku run bash

# We may then run the seed file
node bin/seed.js

```

<br>