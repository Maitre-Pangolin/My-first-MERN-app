## 2021-08-09

---

## First Session
  Following : https://www.youtube.com/watch?v=ngc9gnGgUdA

  First day, app structure creation

  import {} from '', can be used instead of require('') in server side by adding  "type":"module" in package.json

  Error on dependencies installation on client side : react-file-base64 **ERESOLVE**.  
  This error is due to peer-dependencies & react version not matching. NPM v7 try to install a react version (due to peer dependency) older than the one currently installed and throw and error , this can be prevented using *--legacy-peer-deps*. Let's see if this raises conflicts during dev ... [Source stack-overflow](https://stackoverflow.com/questions/64701308/graphql-eresolve-unable-to-resolve-dependency-tree-when-building-my-docker-conta/64744566#64744566)

  body-parser was installed server-side, I think it is supposed to be deprecated, let's circle back later.  
  It is indeed deprecated, Express v4.16.0 onward  includes the middleware express.json() , express.urlencoded()

  MongoDB Atlas will be used for DB , connection credentials typed raw ... Should used environnement variable (maybe later in the course)

  Warning when running server for first-time (with db connection) *body-parser deprecated undefined extended: provide extended option file:\C:\Learning\MERN\memories_project\server\index.js:8:17*  
  Uninstalled body-parser and removed import, message still here.  
  Turns out express built-in middleware for urlencoded (json too) is based on body-parser and needed the extended property to be set. [Express Doc](http://expressjs.com/en/4x/api.html#express.urlencoded).

First route creation for posts , validation that server works properly. Good stoping point for lunch.

---

## Second Session

Implement server side folder structure for scalability, Routes / Controllers / Models folders.

Created a  Mongoose model for posts and basic get / post routes and controllers for posts

[Status code summary](https://www.restapitutorial.com/httpstatuscodes.html)

---

Moving to front-end for post submit

Using material-ui/core , first time using this lib, a bit obscure for me right now ...
Unfamiliar with style in js , and the material ui style workflow, tutorial will not go in details, styles are copy-paste. Need to read more about this.
A bit disappointed by all those tuto skipping styling , saying it doesn't matter and its unimportant. At some point learning how to design is require if you want to produce something that is not too ugly ...  Thinking the UI/UX will drive to some extent the design of the client code , I might be wrong ...  
I guess style sheets / js files should be provided by UI/Ux /Designer, separation of concerns, more efficient, so valid approach if you're not looking to create everything by yourself.  

Connecting client to server, api client folder created, will use **Axios**, wanted to use this for a while 😀.  

hardcoded url used to access back-end , this feels like a bad development/prod practice, maybe using a proxy props in package.json would be better ?

Will start Redux integration, this is my first use of Redux altough I get the general concept. Groceries shopping on the way.

---

## Third Session

Redux initialization not really explained , needs to follow dedicated course on ui.dev  
 `const store = createStore(reducers,compose(applyMiddleware(thunk)))`
no explanations as of now.

Provider is used as a wrapper around the app , it gives access to the store (created in highest level index.js) throughout the whole app , the store is created using the reducers , still unclear how the reducer/actions thingy works,  a reducer take a state and an action as input, based on the action type it does different stuff ... ❓

Action creator are function that return an action, action are an object having a type and a payload , redux thunk allow for asynchronous action , async / dispatch notation, the Action creator use dispatch(action) instead of return (action).

Axios Response Object has several props, including status, data, statusText,headers,config,request.

The getPosts action is using axios through our api structure to retrieve all the post from the DB through our server route, the action goes to the post reducer to be treated by the logic matching the action type, in this case for a FETCH_ALL , the reducer immediatly return the payload from the action , that is the full posts array.

Fixed a bunch of deprecated syntax related to material-ui and a few style typos.

Tutorial shows that we get blocked by CORS policy as I've suspected before we will set-up a proxy in the client package.json but I suspect that something was not properly implemented server-side as we specifically used cors package to prevent this issue.

As suspected the issue server-side was that  our route was defined before all the three middleware, the use(cors()) was therefore not applied.

In form , we spread an existing object in state to modify only one property using setState

> **mem.dev** Flashcard creation , looks interesting.  

Form creation, using material-ui components, using react-file-base allowing to load files

First run with form submit, data is lost somewhere , an entry is created on the db but it's empty as if no model is used.
Data lost before reaching the server empty object in post routes
Wrong implementation on my side of the createPost actions. Forgot to pass the data in the function.   

Weird that we can actually posts something on MongoDb without the fields, think about enforcing model , (server-side).

END OF FIRST VIDEO - Stop for today

## 2021-08-10

---

## First Session