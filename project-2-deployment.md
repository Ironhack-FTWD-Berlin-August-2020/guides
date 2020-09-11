# Deploy on Heroku

Start by signing up on [Heroku](https://www.heroku.com)

Select _Create a new app_ and in the next screen, choose the name of your app.

It will be the name used in the URL to access your app, so choose wisely!

Also, select _Europe_ as a region.

### Adding a MongoDB database to your app - see alternative solution that uses MongoDB Atlas at the bottom (for this you don't have to use a credit card)

Go to your _Account settings_ page and add a credit card. It will not be charged, as long as you don't pick any paid add-on.

Go back to your app's overview page and select _Configure Add-ons_, type _MongoDB_ and select _mLab MongoDB_,
with the free Sandbox option.

You need to make sure that you have all the variables that are in your .env are also saved in Heroku, in the _Config Vars_ section of the _Settings_ tab.

So click on the _Settings_ tab and then on the _RevealConfigVars_ tab. There you enter the configuration variables.



When you are running your app in development it is localhost, in production it should be the address that you can get by clicking on the _Settings_ tab and then _Reveal Config Vars_.

You can see there is already a MONGODB_URI variable configured, so just add this to your mongoose connection link wherever you need it in your project:

The MONGODB_URI variable on heroku will look something like this: 
```
mongodb://heroku_s5t34q0n:1hspkmkdnoddpj4tchb01na9ai@ds0345969.mlab.com:29969/heroku_s5t34q0n 
```

And in your project make this change to the connection in app.js:

```js
// app.js
//
mongoose
  .connect(process.env.MONGODB_URI || 'mongodb://localhost/<Name of your project>' ...)
//
```

(without the ... of course...)
### Possible Problem with the Mongo connection URI

```
2019-07-04T20:56:06.674574+00:00 app[web.1]:     at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1106:14)
2019-07-04T20:56:06.674575+00:00 app[web.1]:   errno: 'ECONNREFUSED',
2019-07-04T20:56:06.674576+00:00 app[web.1]:   code: 'ECONNREFUSED',
2019-07-04T20:56:06.674577+00:00 app[web.1]:   syscall: 'connect',
2019-07-04T20:56:06.674578+00:00 app[web.1]:   address: '127.0.0.1',
2019-07-04T20:56:06.674580+00:00 app[web.1]:   port: 27017 }
```

This means you are trying to connect to MongoDB via localhost. You need to make sure you are using the correct url.

Commit the changes and push to the master branch, this should deploy again.

### Deploy from GitHub
- Now, click on the _Deploy_ tab and _Connect to GitHub_.

- Select your repo from Github (make sure that your package.json is at the root of your repository) and enable _Automatic Deploys_.

- For the first time, select the manual deploy to deploy your master branch.

- If there is an error, select _More_ > _View Logs_.


## Connect to a GUI with Compass 

Start by copying the MONGODB_URI config variable from the *Settings* tab and then open Compass. You should get a prompt asking if you want to use the URI in your clipboard. Accept and then change the following field: *authentication database* which is set as `admin` and use the value in the *username* field instead.


### Alternative without entering credit card details

##  MongoDB Atlas configuration - (without credit card)

<br>



https://www.mongodb.com/atlas-signup-from-mlab



<br>

### **Sign Up & Create a Free Cluster** 

1. Sign Up for Mongo Atlas - enter the email, password, first name, last name
2. Select  >>>  **Starter Clusters**
3. In the drop-down **Cloud Provider & Region** select: 
   -  Europe Region with "**FREE TIER AVAILABLE**" flag
4. In the drop-down "**Cluster Tier**" make sure to select:
   - **M2 Sandbox**
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

     
6. Proceed as above by adding the connection string as a config var in heroku and update the mongo.connection in your project's app.js
<br>

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

```bash
heroku logs -t

```

## Executing scripts on the server - e.g. running a seed file


### [heroku run bash](https://devcenter.heroku.com/articles/heroku-cli-commands#heroku-run)

We can open the terminal instance on the Heroku container ([dyno](https://www.heroku.com/dynos)) in order to run custom scripts or see the files included in the instance. 

```bash
# Open the terminal in the app dyno in Heroku
heroku run bash

# We may then run the seed file
~$~$ node bin/seed.js

```

<br>