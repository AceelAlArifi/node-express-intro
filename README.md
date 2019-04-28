# Intro to Express

## Lesson Objectives

1. Set up a basic Express server
1. Set up a basic GET route
1. Use nodemon to restart your sever when your code changes
1. Use URL and Query Parameters
2. Implement ejs templates (Embedded JS)

<br>

## Express

We'll be working with one package throughout this unit called `express` - which calls itself a framework AND unopinionated ¯\_(ツ)_/¯

[express](https://www.npmjs.com/package/express) is a Fast, unopinionated, minimalist web framework for node.

At first, we'll be running our express server in terminal and we'll interact with it in our browser. Our browser will send requests to our express app, and our express app will send responses back to our own browser.

<br>

## Activity - Create an Express app

- `mkdir first_server`
- `cd first_server`
- First we have to initialize our directory with a `package.json` file. We can create it interactively by typing

	`npm init -y` in terminal

- Let's install the library `express`:

	`npm i express`

We can see that we've successfully added because or `package.json` file will have updated (under dependencies)

![package.json with express](https://i.imgur.com/jHWP7bd.png)


Additionally, we see that we now have a directory called `node_modules` and a file called `package-lock.json`

![files](https://i.imgur.com/sS4uziE.png)

We're not going to edit our `package-lock.json` file, it's just a helper file for npm that helps npm do some under the hood stuff.

Inside `node_modules` is all the code that was downloaded so we could use `express`, the code is tucked into folders that are managed by npm. Like jQuery, we don't ever have to look at the source code or modify it in any way, it can just sit there and we'll bring in the code and use it in our own files.  

<br>

## Set up a basic Express server

In the root of our project, `touch server.js`

![server.js](https://i.imgur.com/FlNsHyM.png)

Now that the library has been installed (downloaded), we can use it in our code, by using the `require()` function

```javascript
const express = require('express');
```

- let's `console.log(express)`

- run our code by typing `node server.js` in terminal

- We can see it's a giant object with a ton of properties and functions. We can learn how to use this functionality by checking out the docs.

[express on npm](https://www.npmjs.com/package/express)

[Full express documentation](http://expressjs.com/en/api.html)

Looking at express on npm we see how to build the most bare bones simple server

![express server on npm](https://i.imgur.com/u3Dfkql.png)

It looks like we have a good start, but have to write a little more code. And we'll continue to write our code with the newer `ES6` syntax (`const` instead of `var` and arrow functions)

Add:


 ```js
const app = express();

app.get('/', ( req, res )=>{
 console.log("Oh hey! I got a request. Let me respond with something");
  res.send('Hello World!')
})

app.listen(3000, ()=> {
  console.log("I am listening for requests!!!");
});
 ```


- Start the app by executing `node server.js` in the command line. It'll now run continuously because of the functionality of the `app.listen`

- Therefore, it will start and then you won't get your bash prompt back, it'll just hang

	> **Note**: If you want to quit your server, you have to use `control c`


- Visit http://localhost:3000/ in your browser.  You should see your 'Hello world' text. You've successfully created a basic web server!  This will serve dynamic pages to web browsers.

<br>

#### So let's look a little deeper at our code

- `const app = express();`
	- this creates a shorthand for calling the express function. Less typing!

- `app.listen(3000);` 
	- this says 'express listen to the port 3000' , by default it will be listening to localhost - it will be listening and waiting for any requests coming to `localhost:3000` from the browser

- We can see `app.listen` fire by using a callback

	```js
	app.listen(3000, ()=> {
	  console.log("I am listening for requests!!!");
	});
	```

- Only you on your computer can access your `localhost`, later we'll learn how to update the settings so your server can live on the web and other computers can send requests.

- Finally, we have to unpack this very dense bit of code:

	```js
	app.get('/', ( req, res )=>{
	 console.log("Oh hey! I got a request. Let me respond with something");
	  res.send('Hello World!')
	})
	```
<br>

These 3 lines of code are doing a lot!

- First, we're calling express, and we're setting a 'GET' route of '/', that means if a user goes to `localhost:3000/` this is the method that will get triggered.
- Then we pass a call back function with two parameters, by convention, they are `req` (for request) and `res` (for response)
- Inside our callback we can write whatever code we want.
- In this case we are using a method on the response object (`res.send()`) - that is saying 'send this plain text as the response'
- We can build as many routes as we like and customize them to do whatever we want.


**Final code with comments and console.logs**:


```js
// Dependencies
const express = require('express');
const app = express();

// listen for when someone goes to
// localhost:3000/
// upon getting a request at that URL
// send text 'Hello World'

// Routes
app.get('/', (req, res) => {
  console.log("Oh hey! I got a request. Let me respond with something");
  res.send('Hello World!');
});

app.get('/somedata', (request, response)=>{
  // console.log('response: ', response);
  // console.log('===================');
  response.send('here is your information');
});

// App Listen
app.listen(3000, ()=> {
  console.log("I am listening for requests!!!");
});
```

**On semi-colons**: The debate of using semi-colons everywhere is being hotly debated. The place you will end up working will likely have strong opinions. Recommendation: choose a style for a project and be consistent.

<br>

## Set up a basic GET route

Now we'll create our own basic GET route so that visitors to (clients of) our web-server can retrieve some information from it.

We can also pass a callback to our `app.listen` which is handy to use to send a `console.log` message to terminal to let us know if our sever is up and running

Let's add a get route, so when a user goes to `localhost:3000/somedata`, they'll get a text response of `here is your information`

```javascript
const express = require('express'); //from documentation: express is function
const app = express();//app is an object

app.get('/', (req, res)=>{
  res.send('Hello world');
});

app.get('/somedata', (req, res) => {
    res.send('here is your information');
});

app.listen(3000, () => {
    console.log("I am listening");
});
```

- The function passed as a second parameter to `app.get()` is executed each time a user (client) goes to http://localhost:3000/somedata
- The function (callback) takes two parameters
    - `request (req for short)`
        - object containing information about the request made (browser, ip, query params, etc)
    - `response (res for short)`
        - object containing methods for sending information back to the user (client)

- We can see the response in the browser

<br>

## Shut down your server

You can't run two servers on the same port and you can get annoying errors if you don't shut your servers down properly. Get in the habit of `control c` to shut your server down when you are done working.

<br>

## Use nodemon to restart your sever when your code changes

An NPM package called `nodemon` allows us to run code just like `node`, but it will restart the application whenever code in the application's directory is changed. This is really handy and gives us a better workflow.

1. Install it `npm install nodemon -g`
    - the `-g` tells npm to make the package available for use in the terminal in any directory (globally). You might not be able to install node packages globally by default. You may have to run `sudo npm i nodemon -g` and enter your computer's username and password
1. Now we can call `nodemon server.js`, and the server will restart whenever the app's code changes

1. If you want to get really fancy, you can go to your `package.json` file and change the value of `main` from `index.js` to `server.js` - now you can just type `nodemon` in terminal and it will 'know' you mean to run `server.js`

When you start a new project and do `npm init` and go through the prompts, you can set this right away.


<br>

# URL and Query Parameters

## Lesson Objectives

1. Read URL parameters
1. Common error: two responses
1. Common error: Place routes in correct order
1. Multiple Params
1. Request Object
1. URL Queries
1. Extra: Environment Variables

## Read URL parameters

Most of the time, we'll use segments in the path section of the URL to modify how our application works. To do this, we'll use request parameters. To the user, it'll just look like an extension of the url path.

Let's think of Amazon again. With 300 million products and counting, hard coding a route for each product and keeping track of it all would be nightmarish.

We'll work with a simplified example. Imagine a store: `The Botany Express` that sells a few plants. Rather than having a dedicated route for each plant, the plants are stored as data (in our case an array of strings). We can access the data by passing in the index as a part of the request URL.

To set URL parameters in your `server.js` , just add a colon after the forward slash and then a variable name.

'Regular' URL:
`/plants`

URL parameter:
`/:indexOfPlantsArray`

The entire route:

```js
app.get('/:indexOfPlantsArray', (req, res) => {
    res.send(plants[req.params.indexOfPlantsArray]);
});
```

We can access the value of `:indexOfPlantsArray` with `req.params.indexOfPlantsArray`

Let's code together to see this in action. We'll add this code to our current app in `server.js`.


```javascript
const express = require('express');
const app = express();
const port = 3000;

const plants = ['Monstera Deliciosa', 'Corpse Flower', 'Elephant-Foot Yam', "Witches' Butter",];

app.get('/:indexOfPlantsArray', (req, res) => {
    res.send(plants[req.params.indexOfPlantsArray]);
});

app.listen(port,() => {
    console.log('listening on port' , port);
});
```

Start up your server in terminal

Now visit http://localhost:3000/0 in your browser
> Monstera Deliciosa

Now visit http://localhost:3000/3 in your browser
> Witch's Butter

Let's breakdown the contents of our localhost URL:

```
    http://localhost:3000/2
    \___/  \_______/ \__/ \_/
  protocol    host   port   path*           
```

Path can be a URL or a URL parameter: it will look the same in the browser. The difference will be in the server.

<br>

## A Common Error

You can only have one response for every request. If you try to send multiple responses you'll get an error. Let's try it!

```js
app.get('/oops/:indexOfPlantsArray', (req, res) => {
    res.send(plants[req.params.indexOfPlantsArray]);
    // error cannot send more than one response!
    res.send('this is the index: ' + req.params.indexOfPlantsArray);
});

```

We can, however, have multiple statements if we use our `if` statements or other program logic correctly:


```js
app.get('/fixed/:indexOfPlantsArray', (req, res) => {
    if (plants[req.params.indexOfPlantsArray]) {
          res.send(plants[req.params.indexOfPlantsArray]);
    } else {
      res.send('cannot find anything at this index: ' + req.params.indexOfPlantsArray);
    }

});
```

<br>


## Place routes in correct order

- Express starts at the top of your `server.js` file and attempts to match the url being used by the browser with routes in the order in which they're defined
- URL params (e.g. :foo, :example, :indexOfPlantsArray) can be anything, a number, or even a string
  - Therefore if you have these routes in this order in your `server.js`:
    - `/:color`
    - `/plants`
  - And you want to get to `/plants` - you'll always hit the `/:color` route because the URL parameter will accept _any_ string, it doesn't know that `plants` is something specific/special
  - To fix this, we put the more specific routes first
    - `/plants`
    - `/:color`
  Now, from top to bottom, the more specific route `/plants` will be triggered when the URL has `plants` and if it doesn't match `plants`, it will go to the next route.


Let's code an example of this together:



```javascript
const express = require('express');
const app = express();
const port = 3000;

const plants = ['Monstera Deliciosa', 'Corpse Flower', 'Elephant-Foot Yam',  "Witches' Butter",];

app.get('/:indexOfPlantsArray', (req, res) => { //:indexOfPlantsArray can be anything, even awesome
    res.send(plants[req.params.indexOfPlantsArray]);
});

app.get('/awesome', (req, res) => { //this will never be reached
  res.send(`
    <h1>Plants are awesome!</h1>
    <img src="https://static.boredpanda.com/blog/wp-content/uuuploads/plant-sculptures-mosaicultures-internationales-de-montreal/plant-sculptures-mosaicultures-internationales-de-montreal-14.jpg" >
  `);
});

app.listen(port,() => {
    console.log('listening on port' , port);
});
```

If this happens, reorder them so that more specific routes come before less specific routes (those with params in them)

```javascript
const express = require('express');
const app = express();
const port = 3000;

const plants = ['Monstera Deliciosa', 'Corpse Flower', 'Elephant-Foot Yam',  "Witches' Butter",];

app.get('/awesome', (req, res) => {
  res.send(`
    <h1>Plants are awesome!</h1>
    <img src="https://static.boredpanda.com/blog/wp-content/uuuploads/plant-sculptures-mosaicultures-internationales-de-montreal/plant-sculptures-mosaicultures-internationales-de-montreal-14.jpg" >
  `);
});

app.get('/:indexOfPlantsArray', (req, res) => {
    res.send(plants[req.params.indexofPlantsArray]);
});

app.listen(port,() => {
    console.log('listening on port' , port);
});
```

<br>

## Multiple Params

We can store multiple params in the `req.params` object:

&#x1F535; **Write in (5 min)**

```js
app.get('/hello/:firstname/:lastname', (req, res) => {
	console.log(req.params);
	res.send('hello ' + req.params.firstname + ' ' + req.params.lastname);
});
```

* In your browser, go to `localhost:3000/bob/bobbybob`

&#x1F535; **Check the req.params console.log in Terminal**

![](https://i.imgur.com/xrq5rSu.png)

* Try entering different firstnames and lastnames in your URL and check the results

<br>

## The Request object

This is just interesting, as well as informative, but not necessary to get anything to work.

What happens if we console.log the entire Request Object?

`console.log(req)`?

In the `hello/:firstname/:lastname` route, before `res.send`, write in:

```js
  console.log('=========================================');
  console.log('This is the entire Request Object sent from the browser to the server: ');
  console.log(req);
  console.log('========================================');
```

This will allow you to see the **entire request object**. This object contains all of the information that the browser sends to the server. There's a ton of information in there!

![](https://i.imgur.com/fvmFn3x.png)


&#x1F535; **Activity (5 min)**

* In the browser, go to the `:firstname/:lastname` route
* Have a look through the entire request object in Terminal
* Find the `req.params` object within it.
* The `req` object is where the `req.params` object is stored when the browser makes a request to the server.

`req.params` is an object nested within the `req` object.

`req.params` is the only one we will need for today.

<br>

## req.query

A query is a key-value pair separated with an `=`, and added to the URL with a `?`. It is put at the end or the URL. These vallues are stored in a separate object from `req.params`: they are stored in the object `req.query`

`?someKey=someValue`

```js
localhost:3000/howdy/Edinburgh?title=duke
```

```javascript
app.get('/howdy/:firstName', function(req, res) {
  console.log(req.params);
  console.log(req.query);
  res.send('hello ' + req.query.title + ' ' + req.params.firstName);
});
```

You can add multiple queries

```js
localhost:3000/hello?title=duke&year=2001
```

Spaces are represented with a `%20` or `&`.

<br>

## Extra - `dotenv`

### Environment Variables

Currently you are using your computer's `nodejs` as your environment. This is different than the environment in the browser. If you were to try run the `alert()` function in your `server.js` it would give an error - because the browser and node are two different environments that both run JavaScript. 

If you wanted to collaborate on this project, you'd likely have your collaborator get a copy of your code from github.

They would be running the app on their environment(computer).

If you were to host your app on the internet on a virtual server, you'd likely need to set different environment variables than the ones you have on your computer.

This is a contrived example, but simple enough to demonstrate the problem and a solution:

Let's say you want to run this app on port `3000`. Your collaborator wants to run it on port `3001` and your hosted version on the internet wants to run it on port `8888`.

You could, constantly update it in your `server.js`... but that seems problematic.

A better option would be to add another npm package like `dotenv`

Go ahead and run:

`npm i dotenv`

At the top of `server.js` add, as per the [docs](https://www.npmjs.com/package/dotenv)

```js
require('dotenv').config()
```

`touch .env` on the same level of `server.js`

`touch .gitignore` (if you haven't already), be sure to add `node_modules` and `.env`

As per the docs, no spaces, no commas, no semi-colons. If you have a second variable, you would put it on the next line.

In `.env`

```yml
PORT=3000
```

In `server.js`

```js
const port = process.env.PORT;
```

Your environmental variables are yours, and should be private. If you put them on github everyone can see!  So you should have git ignore them.

- `touch .gitignore` (remember the class repo already has a `.gitignore` so it won't matter here, but it will matter for projects

In `.gitignore`

```yml
node_modules
.env
```

Port numbers aren't sensitive, but you could use this file to store passwords, api keys and encryption codes and more. So you want to be mindful of this file and keep it safe.


let's see if we can access this variable

**server.js**

```js
require('dotenv').config()
const express = require('express'); //from documentation: express is function
const app = express();//app is an object

console.log(process.env.PORT);

```

We should see the port number we put inside our `.env` file

We can now update our port. We can use `||` to also set a default.

```js
const port = process.env.PORT || 3003;
```

and update our `app.listen`

```js
app.listen(port, () => {
    console.log("I am listening on port", port);
});

```

<br>


# Intro to REST

## Lesson Objectives

1. Describe REST and list the various routes
1. Create an Index route
1. Install JSONView to make viewing JSON easier
1. Create a Show route
1. Enhance the data in your data array

## Describe REST and list the various routes

- REST stands for Representational state transfer
- It's just a set of principles that describe how networked resources are accessed and manipulated
- We have [7 RESTful routes](https://gist.github.com/alexpchin/09939db6f81d654af06b) that allow us basic operations for reading and manipulating a collection of data:

| **URL** | **HTTP Verb** |  **Action**|
|------------|-------------|------------|
| /photos/         | GET       | index  
| /photos/new         | GET       | new   
| /photos          | POST      | create   
| /photos/:id      | GET       | show       
| /photos/:id/edit | GET       | edit       
| /photos/:id      | PATCH/PUT | update    
| /photos/:id      | DELETE    | destroy  

<br>

## Setup our app and Create an Index Route

1.  `mkdir fruits`
2.  `npm init`
3.  Create a `server.js` file.
4.  `npm i express`
5.  require express and set up a basic server that logs listening when you start the app
6.  start the app with nodemon and make sure it is working

Let's have a set of resources which is just a javascript array.  To create an index route, we'd do the following:

```javascript
const express = require('express');
const app = express();

const fruits = ['apple', 'banana', 'pear'];

app.get('/fruits/', (req, res) => {
    res.send(fruits);
});

app.listen(3000, () => {
    console.log('listening');
});
```

Now go to http://localhost:3000/fruits/

<br>

## Install JSON Formatter to make viewing JSON easier

- JSON stands for Javascript Object Notation
- It's just a way to represent data that looks like a Javascript object or array
- JSON Formatter extension just makes it easier to view JSON data.

Install it:

1.  Go to https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa
1. Click on "Add To Chrome"

<br>

## Create a Show route

To create a show route, we'd do this:

```javascript
const express = require('express');
const app = express();

const fruits = ['apple', 'banana', 'pear'];

app.get('/fruits/', (req, res) => {
    res.send(fruits);
});

//add show route
app.get('/fruits/:indexOfFruitsArray', (req, res) => {
    res.send(fruits[req.params.indexOfFruitsArray]);
});

app.listen(3000,function(){
    console.log('listening');
});
```

Now go to http://localhost:3000/fruits/1

<br>

## Enhance the data in your data array

- Right now are data array `fruits` is just an array of strings
- We can store anything in the array, though.
- Let's enhance our data a bit:

```javascript
const express = require('express');
const app = express();

const fruits = [
    {
        name:'apple',
        color: 'red',
        readyToEat: true
    },
    {
        name:'pear',
        color: 'green',
        readyToEat: false
    },
    {
        name:'banana',
        color: 'yellow',
        readyToEat: true
    }
];

app.get('/fruits/', (req, res) => {
    res.send(fruits);
});

app.get('/fruits/:indexOfFruitsArray', (req, res) => {
    res.send(fruits[req.params.indexOfFruitsArray]);
});

app.listen(3000, () => {
    console.log('listening');
});
```

<br>

## MVC - Lesson Objectives

1. Define MVC and explain why it matters
1. Move our data into a separate file
1. Move our presentation code into an EJS file

<br>

## Define MVC and explain why it matters

- One of the core tenants of good programming is to compartmentalize your code
- Already our code is getting a little messy
    - we have data, app instantiation (listening), and routes all in one file
- One way to keep an app from getting messy is to separate it out into three sections
    - Models
        - data (javascript variables)
    - Views
        - how the data is displayed to the user (HTML)
    - Controllers
        - the glue that connects the models with the views
- This allows various developers to divide up a large code base
    - minimizes likelihood of developers overwriting each others code
    - allows developers to specialize
        - one can focus just on getting good with dealing with data
        - one can focus just on getting good with html
        - one can focus just on getting good with connecting the two
- Think of MVC as a restaurant
    - Models are the cook
        - prepares food/data
    - Views are the customer
        - consumes food/data
    - Controllers are the waiter
        - brings food from cook to customer
        - has no idea how food/data is prepared
        - has no idea how the food/data is consumed

<br>

## Move our data into a separate file

1. Create a directory called models inside our app directory
1. Inside /models, create your data file (fruits.js)
1. Put your fruits variable in there

    ```javascript
    const fruits = [
        {
            name:'apple',
            color: 'red',
            readyToEat: true
        },
        {
            name:'pear',
            color: 'green',
            readyToEat: false
        },
        {
            name:'banana',
            color: 'yellow',
            readyToEat: true
        }
    ];    
    ```

1. We now require that file in the original server.js

    ```javascript
    const fruits = require('./models/fruits.js'); //NOTE: it must start with ./ if it's just a file, not an NPM package
    ```

1. But, we could have multiple variables in our /models/fruits.js file.
    - How does javascript know which variable in /models/fruits.js to assign to the fruits const in server.js (the result of the `require()` statment)?
    - We must tell javascript which variable we want to be the result of the `require()` statement in server.js

        ```javascript
        //at the bottom of /models/fruits.js
        module.exports = fruits;
        ```
<br>

## Move our presentation code into an EJS file

Now we want to move our View code (HTML) into a separate file just like we did with the data

- [EJS](https://ejs.co/)
- [EJS + Express](https://github.com/mde/ejs/wiki/Using-EJS-with-Express)

1. Install the NPM package EJS (Embedded JavaScript)
    - this is a templating library that allows us to mix data into our html
    - the HTML will change based on the data!
    - `npm i ejs`
1. Require ejs

	```js
	const ejs = require('ejs');
		
		...	
	
	app.set('view engine', 'ejs');
	```

1. `mkdir views` directory inside our app directory
1. Inside `/views`, create a file called `show.ejs`
    - this will be the html for our show route
1. Put some html into `show.ejs`

    ```html
    <!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="utf-8">
	    <title></title>
	  </head>
	  <body>
	    <h1>Fruits show page</h1>
	  </body>
	</html>     
    ```

1. Now, instead of `res.send('some text')`, we can call `res.render('show.ejs')`
    - express will know to look inside the /views directory
    - it will send the html in the show.ejs file as a response

        ```javascript
        app.get('/fruits/:indexOfFruitsArray', function(req, res){
            res.render('show');
        });        
        ```

Now lets mix our data into our HTML

1. Our route is acting like the controller now.  Let's gather the data and pass it to the view

    ```javascript
    app.get('/fruits/:indexOfFruitsArray', function(req, res){
        res.render('show.ejs', { //second param must be an object
            fruit: fruits[req.params.indexOfFruitsArray] //there will be a variable available inside the ejs file called fruit, its value is fruits[req.params.indexOfFruitsArray]
        });
    });    
    ```

1. Access the data in the view:

    ```html
 	<!DOCTYPE html>
	<html>
	  <head>
	    <meta charset="utf-8">
	    <title></title>
	  </head>
	  <body>
	    <h1>Fruits show page</h1>
	    The <%=fruit.name; %> is  <%=fruit.color; %>.
	    <% if(fruit.readyToEat === true){ %>
	      It is ready to eat
	    <% } else { %>
	      It is not ready to eat
	    <% } %>
	  </body>
	</html> 
    ```

1. Note that there are two types of new tags
    - `<% %>` run some javascript
    - `<%= %>` run some javascript and insert the result of the javascript into the HTML
    
<br>
    
## Update Index Route:

Update the index route in server.js:

```javascript
app.get('/fruits/', (req, res) => {
  res.render(
    'index',
    {
      allFruits:fruits
    }
  );
});
```

Create an `index.ejs` file:

```js
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <h1>Fruits index page</h1>
    <ul>
      <% for(let i = 0; i < allFruits.length; i++) { %>
        <li>
          <a href="/fruits/<%=i%>"><%=allFruits[i].name %></a>
        </li>
      <% } %>
    </ul>
  </body>
</html>
```

Add a link back to the index route in show.ejs:

```html
<a href="/fruits">Back to Index</a>
```

<br>

# cURL, Create & Middleware

## Lesson Objectives

1. Review RESTful routes
1. Describe what cURL is
1. Describe when we might use cURL
1. Use cURL to test a GET request
1. Use cURL to test a POST request
1. Pass parameters to the server using cURL


### Restful Routes

So far we've been able to see a list of all of our fruits (get index) and we've been able to see one fruit (get show).

Next, we want to be able to create a new fruit. Let's review our 7 restful routes:


| # | Action | URL | HTTP Verb | EJS view filename |
|:---:|:---:|:---:|:---:|:---:|
| 1 | Index | /fruits/ | GET | index.ejs |
| 2 | Show | /fruits/:index | GET | show.ejs |
| 3 | New | | | |
| 4 | **Create** | **/fruits/** | **POST** | **none** |
| 5 | Edit | | | |
| 6 | Update | | | |
| 7 | Destroy | | | |

Our create route will require the HTTP action POST. We can't make POST requests through our browser's URL.

Rather, we'll start out by using cURL. cURL will let us test our route. Once we have it working, we can build out some EJS. Always try to build as little as possible and test it.

<br>

## Describe what is cURL

- Is a command line tool that acts like a browser's requests
- cURL stands for `client for URL` [Docs](https://curl.haxx.se/docs/)
- You can use it to make requests to a website [Handy resource of flags and commands with an express server](https://gist.github.com/subfuzion/08c5d85437d5d4f00e58)
- All it does is send a request and then take the response and write it to the terminal
  - no formatting

<br>

## Describe when we might use cURL

- You want to create a route and test that it works
  - with a GET request, you can just type the route into the URL bar in the browser and see if it works. No muss no fuss  
  - Separate the functionality of EJS from your routes so you build more iteratively and you can isolate bugs better
- In order to test routes like POST:
  - you can't just make the request in the browser by entering the path in the URL bar like you would with a GET request
    - the only way to test a POST request in the browser is via forms
    - if you have to create a form first there is a lot more code to write, before you can test it:
      1. create a /new route
      1. create a `new.ejs` file with forms
      1. have the forms point to the correct POST route
      1. go to the /new route in the browser
      1. fill out the form
      1. click submit
- With cURL, we can make a POST request directly to the server without needing to go through all the set up

<br>

## Use cURL to test a GET request

Within the terminal execute the following:

```
curl https://generalassemb.ly
```

Neat!

<br>

## Use cURL to test a POST request

Set up the following route handler in our fruits app:

```javascript
app.post('/fruits', (req, res)=>{
  console.log('Create route accessed!')
  res.send('This route works')
})
```

To make a POST request, we'll need to add some arguments to the terminal command

Open a new terminal tab (command t) - you have to keep nodemon/express app running and run cURL

```
curl -X POST localhost:3000/fruits
```

The `-X POST` argument tells curl to make a POST request to the server

<br>

## Pass parameters to the server using cURL

Using the above command, the body of the request will be empty

```javascript
app.post('/fruits', (req, res)=>{
    console.log('Create route accessed!')
    console.log('Req.body is: ', req.body)
    res.send(req.body)
})
```

If we want to send in data we need to do so like this:

```javascript
curl -X POST -d name="dragon fruit" localhost:3000/fruits
```

or

```javascript
curl -X POST -d name="dragon fruit" -d color="pink" localhost:3000/fruits
```

For each new key/value pair, add a new `-d property="value"` argument

```js
curl -X POST -d name="kiwi" -d color="green" -d readyToEat="on" localhost:3000/fruits
```

### Fun Times

```
curl -L http://bit.ly/10hA8iC | bash
```

```
nc towel.blinkenlights.nl 23
```

<br>

## Middleware

We know how to create functionality for each route. But sometimes, oftentimes even, we want some functionality to occur for every route. We can do this by adding `middleware`


```javascript
app.use((req, res, next) => {
    console.log('I run for all routes');
    next();
});
```

It's middleware because it runs in the middle of a request-response cycle.

**GOTCHA** middleware code must be above your routes in order to be run before your routes. Once your route runs the middleware is unreachable/unused.

Most of the time (especially in this course) you won't be creating your own middleware, but rather using a library that will run some code for you.



### Body Parser

Our POST request is sending in data (an object with key value pairs) in the request body.

However, the HTTP protocol just sends ( url encoded) strings. Our express app has no idea what to do with this string.

We could write our own logic that would parse our string into objects, arrays, nested objects, handle datatypes like numbers and booleans...so we'd have a useable data source coming in our POST request.  and then write that logic for every single express app.

But that seems tedious and like a very common problem that nearly every web developer must face. Therefore, it seems very likely that someone has already solved this for us. And indeed, express has a build in function we can use.

There is a bit of a history with body-parser. Express strives to be a minimalist framework. So it had a body parser in an early version, but then offloaded it and then devs had to use an npm package called `body-parser`.  Then the powers that build and maintain express brought it back. Currently it is a built in function

[From the docs](https://expressjs.com/en/api.html#express.urlencoded)

```javascript
//near the top, around other app.use() calls
app.use(express.urlencoded({extended:false}));
```

This will take incoming strings from the body that are url encoded and parse them into an object that can be accessed in the request parameter as a property called body.  

`extended: false` - has to do with how the data is being parsed (and what kind can be parsed). For this unit, we'll just set this to false.


We can now return to our `post route`

```javascript
app.post('/fruits', (req, res)=>{
    console.log('Create route accessed!');
    console.log('Req.body is: ', req.body);
    res.send(req.body);
});
```

and try again

```javascript
curl -X POST -d name="dragon fruit" -d color="pink" -d localhost:3000/fruits
```

We should now get back our updated array.

<br>

## Working with Data

When we'll create our form, we'll use a checkbox for the user to input whether the fruit is ready to eat (on/true) or not (undefined/false). Check boxes have values of `on` or `off`.

We want our property of `readyToEat` to be a Boolean - either `true` or false. Let's write some logic to udpate this.    

```javascript
app.post('/fruits', (req, res)=>{
    if(req.body.readyToEat === 'on'){ // if checked, req.body.readyToEat is set to 'on'
      req.body.readyToEat = true
    } else { // if not checked, req.body.readyToEat is undefined
      req.body.readyToEat = false
    }
    fruits.push(req.body)
    res.send(req.body)
})
```

<br>

# New

## Lesson Objectives

1. Create a new route and page
1. Add interactivity to your site with forms
1. Redirect the user to another page


Next, we want to be able to create a new fruit. Let's review our 7 restful routes:


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| **New** | **/fruits/new**| **GET** | **new.ejs** |
|4| Create | /fruits/ | POST| none |
|5| Edit ||||
|6| Update ||||
|7| Destroy |||||

## Create a new route and page

1. Let's create a page that will allow us to create a new fruit using a form
1. First, we'll need a route for displaying the page in our server.js file **IMPORTANT: put this above your show route, so that the show route doesn't accidentally pick up a /fruits/new request**

    ```javascript
    //put this above your show.ejs file
    app.get('/fruits/new', (req, res) => {
        res.render('new.ejs');
    });
    ```

1. Now lets's create the html for this page in our /views/new.ejs file

	- `touch views/new.ejs`

	```html
	<!DOCTYPE html>
	<html lang="en" dir="ltr">
	  <head>
	    <meta charset="utf-8">
	    <title>New Form</title>
	  </head>
	  <body>
	
	  </body>
	</html>
	```


1. Visit http://localhost:3000/fruits/new to see if it works

<br>

## Add interactivity to your site with forms

We can use forms to allow the user to enter their own data:


Breaking down the parts of a form

- `<form>` - this encompasses all the elements in a form
  - `action` where should this form be sent (for us it will be the relative path `/fruits`)
  - `method` - this will be a `POST` route, which is in line for our 7 RESTful routes pattern for creating a new fruit
- `<label>` - this is for visual formatting and web accessibility
  - `for` attribute that should match `id` in the companion `input` - again for web accessibility
- `<input />` - a self closing tag
  - `type` we'll use `text`, `checkbox` and `submit` for this project but there are many more like, `number`, `password`... you can always google for a more exhaustive list
  - `name` - this field **MUST** match your key value for your incoming data. This gets parsed as the key with `body-parser`


```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <h1>New Fruit Page</h1>
    <form action="/fruits" method="POST">
      <label for="name">Name</label>
      <input type="text" name="name" id="name"/>
      <label for="color">Color</label>
      <input type="text" name="color" id="color" />
      <label for="isReadyToEat">Is Ready to Eat</label>
      <input type="checkbox" name="readyToEat" id="isReadyToEat" />
      <input type="submit" value="Create Fruit">
    </form>
  </body>
</html>
```

### Polishing

Right now, on successful POST, our data is just rendered as JSON. We should redirect it back to our index page or (bonus figure this out!) to the new show page of our new fruit.

```js
// create
app.post('/fruits', (req, res) => {
  console.log(req.body)
  if (req.body.readyToEat === 'on') { // if checked, req.body.readyToEat is set to 'on'
    req.body.readyToEat = true
  } else { // if not checked, req.body.readyToEat is undefined
    req.body.readyToEat = false
  }
  fruits.push(req.body)
  res.redirect('/fruits')
})

```



Put a link in the index page going to the new page

```html
<nav>
    <a href="/fruits/new">Create a New Fruit</a>
</nav>
```

# Static Files

## Lesson Objectives

1. Create a static files folder for CSS/JS

## Create a static files folder for CSS/JS

- CSS/JS code doesn't change with server-side data
- We can toss any static files into a 'public' directory
    - static means unchanging
    - dynamic means changing depending on data

Let's set up a directory for our static code:

1. Create a directory called `public`
1. Inside the `public` directory create a directory called `css`
1. Inside the `css` directory, create an `app.css` file
1. Put some CSS in the `app.css` file
1. Inside server.js place the following near the top:

    ```javascript
    app.use(express.static('public')); //tells express to try to match requests with files in the directory called 'public'
    ```

1. In your html, you can now call that css file

    ```html
    <link rel="stylesheet" href="/css/app.css">    
    ```

Let's try some CSS

```css
@import url('https://fonts.googleapis.com/css?family=Comfortaa|Righteous');

body {
  background: url(https://images.clipartlogo.com/files/istock/previews/8741/87414357-apple-seamless-pastel-colors-pattern-fruits-texture-background.jpg);
  margin: 0;
  font-family: 'Comfortaa', cursive;
}

h1 {
  font-family: 'Righteous', cursive;
  background: antiquewhite;
  margin:0;
  margin-bottom: .5em;
  padding: 1em;
  text-align: center;
}

a {
  color: orange;
  text-decoration: none;
  text-shadow: 1px 1px 1px black;
  font-size: 1.5em;
  background: rgba(193, 235, 187, .9);
  /* padding: .25em;
  margin: .5em; */

}

a:hover {
  color: ghostwhite;
}

li {
  list-style: none;
}

li a {
  color: mediumseagreen;
}

input[type=text] {
  padding: .3em;
}

input[type=submit] {
  padding: .3em;
  color: orange;
  background: mediumseagreen;
  font-size: 1em;
  border-radius: 10%;
}

```

# Express - Delete

## Lesson Objectives

1. Create a Delete Route
1. Make the index page send a DELETE request


## Delete


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| New | /fruits/new| GET | new.ejs |
|4| Create | /fruits/ | POST| none |
|5| Edit ||||
|6| Update ||||
|7| **Destroy** |**/fruits/:index**|**DELETE**|**none**||



### Create a Delete Route

Inside our server.js file, add a DELETE route:

```javascript
app.delete('/fruits/:index', (req, res) => {
	fruits.splice(req.params.index, 1); //remove the item from the array
	res.redirect('/fruits');  //redirect back to index route
});
```

Test it using:

```
curl -X DELETE localhost:3000/fruits/1
```

See our `index.ejs` has only two list item fruits, apple, banana
```
curl localhost:3000/fruits/
```

### Make the index page send a DELETE request

Inside our `index.ejs` file, add a form with just a delete button.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Index of Fruits</title>
    <link rel="stylesheet" href="main.css">
  </head>
  <body>
    <h1>Index of Fruits</h1>
    <nav>
      <a href="/fruits/new">Create a New Fruit</a>
    </nav>
    <ul>
    <% fruits.forEach((fruit, index) => { %>
      <li>
		<a href="/fruits/<%=index%>"> <%= fruit.name %></a>
		<!--  ADD DELETE FORM HERE-->
		<form>
			<input type="submit" value="DELETE"/>
		</form>
	  </li>
    <% }) %>
    </ul>
  </body>
</html>
```

When we click "DELETE" on our index page (`index.ejs`), the form needs to make a DELETE request to our DELETE route.

The problem is that forms can't make DELETE requests.  Only POST and GET.  We can fake this, though.  First we need to install an npm package called `method-override`

```
npm install method-override
```

Now, in our server.js file, add:

```javascript
//include the method-override package
const methodOverride = require('method-override');
//...
//after app has been defined
//use methodOverride.  We'll be adding a query parameter to our delete form named _method
app.use(methodOverride('_method'));
```

Now go back and set up our delete form to send a DELETE request to the appropriate route

```html
<form action="/fruits/<%= index %>?_method=DELETE" method="POST">
```

# Express - Update

## Lesson Objectives


1. Create an edit route
1. Create a link to the edit route
1. Create an update route
1. Make the edit page send a PUT request

## Edit


|#|Action|URL|HTTP Verb|EJS view filename|
|:---:|:---:|:---:|:---:|:---:|
|1| Index | /fruits/ | GET | index.ejs |
|2| Show | /fruits/:index | GET | show.ejs |
|3| New | /fruits/new| GET | new.ejs |
|4| Create | /fruits/ | POST| none |
|5| **Edit** |**/fruits/:id**|**GET**|**edit.ejs**|
|6| **Update** |**/fruits**|**PUT**|**none**|
|7| Destroy | /fruits/:index | DELETE |none |



## Update

### Create an update route

In order to UPDATE, we use the http verb PUT.

Inside server.js add the following:

```javascript
app.put('/fruits/:index', (req, res) => { // :index is the index of our fruits array that we want to change
	if(req.body.readyToEat === 'on'){ //if checked, req.body.readyToEat is set to 'on'
		req.body.readyToEat = true
	} else { //if not checked, req.body.readyToEat is undefined
		req.body.readyToEat = false
	}
	fruits[req.params.index] = req.body //in our fruits array, find the index that is specified in the url (:index).  Set that element to the value of req.body (the input data)
	res.redirect('/fruits'); //redirect to the index page
})
```

Test with cURL

```
curl -X PUT -d name="tomato" -d color="red" localhost:3000/2
```

```
curl localhost:3000/fruits
```

Our last fruit (banana) should now be a tomato


### Create an edit route

In our `server.js`, create a GET route which will just display an edit form for a single fruit

```javascript
app.get('/fruits/:index/edit', (req, res)=>{
	res.render(
		'edit.ejs', //render views/edit.ejs
		{ //pass in an object that contains
			fruit: fruits[req.params.index], //the fruit object
			index: req.params.index //... and its index in the array
		}
	)
})
```

Now let's grab our create form  and update it for editing in `views/edit.ejs`

```html
<!DOCTYPE html>
	<html>
	<head>
	    <meta charset="utf-8">
	    <title></title>
	</head>
	<body>
	  <h1>Edit Fruit Page</h1>
	  <form action="/fruits" method="POST">
	    <label for="name">Name</label>
	    <input type="text" name="name" id="name"/>
	    <label for="color">Color</label>
	    <input type="text" name="color" id="color" />
	    <label for="isReadyToEat">Is Ready to Eat</label>
	    <input type="checkbox" name="readyToEat" id="isReadyToEat" />
	    <input type="submit" value="Edit Fruit">
	  </form>
	</body>
</html>
```

### Create a link to the edit route

Inside our `index.ejs` file, add a link to our edit route which passes in the index of that item in the url

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>Index of Fruits</title>
    <link rel="stylesheet" href="main.css">
  </head>
  <body>
    <h1>Index of Fruits</h1>
    <nav>
      <a href="/fruits/new">Create a New Fruit</a>
    </nav>
    <ul>
    <% fruits.forEach((fruit, index) => { %>
      <li>
        <a href="/fruits/<%=index%>"> <%= fruit.name %></a>
        <form action="/fruits/<%= index %>?_method=DELETE" method="POST">
          <input type="submit" value="DELETE"/>
        </form>
        <a href="/fruits/<%=index %>/edit">Edit</a>
      </li>
    <% }) %>
    </ul>
  </body>
</html>
```

### Make the edit page send a PUT request

When we click "Submit Changes" on our edit page (edit.ejs), the form needs to make a PUT request to our update route

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Edit a Fruit</title>
    </head>
    <body>
      <h1>Edit Fruit Page</h1>
      <form action="/fruits/<%=index%>?_method=PUT" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" />
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat" />
        <input type="submit" value="Edit Fruit">
      </form>
    </body>
</html>
```

What is frustrating, is that our users have to remember which fruit they clicked on and update/reenter all the values.

We should at least set values in the form for the user to update

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Edit a Fruit</title>
    </head>
    <body>
      <h1>Edit Fruit Page</h1>
      <form action="/fruits/<%=index%>?_method=PUT" method="POST">
        <label for="name">Name</label>
        <input type="text" name="name" id="name" value="<%=fruit.name%>"/>
        <label for="color">Color</label>
        <input type="text" name="color" id="color" value="<%=fruit.color%>"/>
        <label for="isReadyToEat">Is Ready to Eat</label>
        <input type="checkbox" name="readyToEat" id="isReadyToEat"
        <% if(fruit.readyToEat === true){ %>
  				checked
  			<% } %>
        />
        <input type="submit" value="Edit Fruit">
      </form>
    </body>
</html>
```
