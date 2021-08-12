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

> Weird that we can actually posts something on MongoDb without the fields, think about enforcing model , (server-side).

END OF FIRST VIDEO - Stop for today

## 2021-08-10

---

## First Session

Project finally on Github after publishing my MongoDB credentials twice like the genius I am. Dotenv was added to the project to pass credentials as env variable (but I still managed to publish my .env because I don't know how to write a path properly).

[**Project Part 2**] (https://www.youtube.com/watch?v=aibtHnbeuio)

Modified Posts components to display all the posts, passing post from redux state (useSelector) as props to Post components

Modifying Post component error in importing material-ui icons. Turns out it's another package, not installed in the first video I guess it will be done later in this one.

> Improvement idea , deal with user input too large a file

>Random thought , need to learn / read / watch about caching .

Card component created , a few comments : 
* tags are not separated , always one element , need to be splitted by space and or commas and trimmed.
* Message displayed instead of title , message not considered when fixed.
  

> Using [SVG Background](https://www.svgbackgrounds.com/) a website to generate or use SVG background

Stopping point for this session maybe one later altough we're going to Le chasse-pinte microbrew so it might be it for today.

## 2021-08-10

---

## First Session

Implemented back-end logic for updating posts

Now working on front-end update logic, posts information and id should be shared between Post and Form components, first approahc in pure React (no Redux) , we will use the higher order components to share the information in this case App. This will be refactored to use Redux. 

This force us to use props drilling , sharing state between components force us to go back to the higher components that includes the two former, and to drill props from the higher components to the one were we want to share state. Viable for small scale app with shallow tree structure, become very messy for bigger project.

Updated the following to include our update functionnality : Forms (comp), Posts (comp), Post (comp) , post api (add patch) , redux actions, redux reducers.

useEffect in Form to change the form data when the shared (top level in App) currentID is modified.

> should add a default image when none is provided by user, currently throwing warning

Post Date creation seems to bug, just created a post , and app says it was created 18 hours ago. 🔥

Try to restart server and client, server crash due to mongoose import in controller file. Fixed, was imported as a named import instead of a default : *import {Mongoose} from ''mongoose ==> *import Mongoose from 'mongoose'* 🔥

After restart post date seems better altough after creation the posts already says it was created 2 minutes ago. Might be something wrong with the moment library or how we're using it in this project.

> Investigate date offset 🕔

Post modification functionnality seems to work properly , in the video he needs to manually refresh his page to see post modification, works immediatly in my case might be due to React updates. Let's see..

Just realized that handleSubmit() is called by the button with *type='submit'* without the need to define a onClick=handleSubmit, implicit reference
Implemented clear method on form.

Big debug session in the video because his post are not updating, weird fix on App useEffect and on his mongoose import in controllers. Not working. (It's working fine on my side) 

Turns out he add a typo, he added a bunch off stuff like the useEffect on App and added the _id to the post object on the action, I don't think it is necessary as we provide (id,post) post without_id to the patch request. Wonder of the object merging works in mongoose/MongoDB , need to check in Mongo/Mongoose class.
Rectification he did this in the controller section (back-end side).

Modified a useless destructuration renaming he made us write in controllers, works the same, cleaner.

> [All wonders of destructuration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
> 
```javascript
let a ='foo'
let b = 'bar'
[a,b]=[b,a]
//Wonderfull variable exchange
```

Stopping point for this session prior to delete functionnality implementation. Going to try out outdoor gear prior to Lac-St Jean 4 days biking trip and eat.

---

## Second Session

Delete functionnality implemented succesfully.

Adding a like functionnality, the new implemented version feels not really optimized as we are doing two req to the db :

``` javascript
export const likePost = async (req,res) =>{
  const { id } = req.params;
  if (!mongoose.Types.ObjectId.isValid(id))
  return res.status(404).send("No post with that id");
  const post = await PostMessage.findById(id)
  const updatedPost = await PostMessage.findByIdAndUpdate(id,{likeCount: post.likeCount+1},{new:true})
  res.json(updatedPost)
}
```

I think there is a way to directly increment the field using a $inc operator in mongo/mongoose calls but I'm not sure of the syntax. 

> Verify mongo call

Bug after implementing Like functionnality 🔥. Getting a 404 . Typo in route , i forgot a ':' before the route id params in my routes.js file. Still bugging . Forgot to include the controller in the route ... 🤦‍♂️
![test](https://media.giphy.com/media/d3YHKs8wwYfce0PS/giphy.gif)

Added a placeholder image (extra, not in video).

Finally we are fixing the cards message/title and most importantly the fricking hashtag list that was bugging me from the beggining 

![thanks](https://c.tenor.com/qptJAAqoow8AAAAC/thanks-thank-you.gif)

GIF integration in Markdown is a dangerous power ...


Wanted to add white space on like button, turns out it was a mess all html entities like &nbsp do not render on my side, cannot find an good answer why it's not working (working on the video) lot of different work around on stack overflow but no real explanation.Decided to use whiteSpace:'pre' styling and to ass space in jsx {'  '}

Session cut short by secondary screen battery dying on me 😢. => Hike and beer on the way, more tomorrow.



