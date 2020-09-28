# Deploy on Heroku

Start by signing up on [Heroku](https://www.heroku.com)

Select _Create a new app_ and in the next screen, choose the name of your app.

It will be the name used in the URL to access your app, so choose wisely!

Also, select _Europe_ as a region.

Go to your _Account settings_ page and add a credit card. It will not be charged, as long as you don't pick any paid add-on.

Go back to your app's overview page and select _Configure Add-ons_, type _MongoDB_ and select _mLab MongoDB_,
with the free Sandbox option.

Now, click on the _Deploy_ tab and _Connect to GitHub_.

Select your repo from Github (make sure that your package.json is at the root of your repository) and enable _Automatic Deploys_.

For the first time, select the manual deploy to deploy your master branch.

If there is an error, select _More_ > _View Logs_.

## Troubleshooting

To limit errors, you can make sure that every variable that you have in your .env is also saved in Heroku, in the _Config Vars_ section of the _Settings_ tab.

### Mongo connection URI

```
2019-07-04T20:56:06.674574+00:00 app[web.1]:     at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1106:14)
2019-07-04T20:56:06.674575+00:00 app[web.1]:   errno: 'ECONNREFUSED',
2019-07-04T20:56:06.674576+00:00 app[web.1]:   code: 'ECONNREFUSED',
2019-07-04T20:56:06.674577+00:00 app[web.1]:   syscall: 'connect',
2019-07-04T20:56:06.674578+00:00 app[web.1]:   address: '127.0.0.1',
2019-07-04T20:56:06.674580+00:00 app[web.1]:   port: 27017 }
```

This means you are trying to connect to MongoDB via localhost. You need to make sure you are using the correct url.

When you are running your app in development it is localhost, in production it should be the address that you can get by clicking on the _Settings_ tab and then _Reveal Config Vars_.

You can see there is already a MONGODB_URI variable configured, so just add this to your mongoose connection link wherever you need it in your project:

```
mongoose
  .connect(process.env.MONGODB_URI || "mongodb://localhost/movie-project" ...)
```

(without the ... of course...)

Commit the changes and push to the master branch, this should deploy again.


## Connect to a GUI

To visualize your database on Compass or [Studio3T](https://studio3t.com/ironhack) start by copying the MONGODB_URI config variable from the *Settings* tab.

### Compass

You should get a prompt asking if you want to use the URI in your clipboard. Accept and then change the following field: *authentication database* which is set as `admin` and use the value in the *username* field instead.

### Studio3T

Select *Connect*, *New Connection* and *From URI* on the main tab. Just paste the link and you're set up! Next time, just select the connection.


## React deploy


As for the first time, you should initialize your git repo so that your server code is at the root level.

This should be the structure you have at the root level: 

![root-level structure](https://i.imgur.com/Xihs4uP.png)

Start by running `npm run build` inside your `client` folder, this will create the static files that we'll need to serve.

![npm run build](https://i.imgur.com/VWsclun.png)

This is what the `client` folder structure should look like: 

![client structure](https://i.imgur.com/3J9U4gQ.png)

Notice that the build folder is ignored by create-react-app:

![create-react-app .gitignore](https://i.imgur.com/RA3UmIT.png)


To add it to our commit, we will need to run `git add -f ` (force):

![git add -f](https://i.imgur.com/34seZ0K.png)

(and commit 😉)

🚨 We will need to run `npm run build` and to add and commit the changes (also to the `/build` folder) every time we write new code on the client side 

### Changes to server/app.js

Don't forget to link to `process.env.MONGODB_URI` for the mongoose connection.

Replace the line: 
```
// app.use(express.static(path.join(__dirname, "public")));
```
with
```
app.use(express.static(path.join(__dirname, "/client/build")));
```

And finally, add this at the very bottom of your app.js, after any other route:
```
app.use((req, res) => {
  // If no routes match, send them the React HTML.
  res.sendFile(__dirname + "/client/build/index.html");
});
```

### Heroku 

On heroku, nothing changes! You can start by adding the mLab add on, and ensuring that you have copied all your .env variables

![config vars](https://i.imgur.com/HrBLoeZ.png)

Then, in the deploy tab, check that you have connected to your github repo and automatic deploys, this way heroku will deploy on every push on the default branch.

And... that's it! Happy deployment 😉