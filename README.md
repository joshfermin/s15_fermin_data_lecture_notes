Lecture 1 
=========
Josh Fermin - 1/13/2015 

##Big Data:

#### Data analytics
  - Sampling ... Machine Learning
  - Statistics largely based on sampling. If you already know the answer, not really statistics.

#### Storage
  - Data modeling is very hard - i.e. data formatting in facebook. New posts will have a different format than the old posts. They wont convert the old posts into the new data type.
  - NoSQL
    - Graph
    - Key-Value Stores
    - Columnar

####Twitter
  - Twitter Clients -> Twitter <- Tweet
  - Sometimes twitter clients use different encoding than utf-8 
  - Twitter uses utf-8
  - End tweet is half utf-8 and half different encoding (hard to perform analytics on)
    - i.e. python would expect it to be all utf-8, blows up if different

####Info Viz
  - D3 (visualiztion - graphs)
  - excel / R (data analysis)


##Data Lifecycle
0. Question
  * Curation / Triage Persistence  - prioritization of data sources
  * Which source will give me the answer?
1. Collection / Generation
2. Cleanup
3. Storage
4. Processing / Analysis
5. Query / visualize / ACT (data transformed to knowledge we can act upon)
  * This usually gives you new questions

This lifecycle is in many startup companies that do big data
Teams of people within each step.




Lecture 2
=========
Josh Fermin - 1/15/2015

##Request Response Cycle
http://
http request methods
  * GET
  * POST
  * PUT
  * DELETE

Some request comes out, associated with url, server recieves url, maps request to file and returns back a response

Web Bowser -> Web Server <- ebay.com/products/10
* Passes /products/10 to web server
  * will get handled as a GET / POST / DELETE / PUT 
  * Web Server passes request off to handler(s) 
  * Server now has to wait until handler(s) comes back with response to give to user
  * I.e. different requests could correspond to different servers storing different images that have to be loaded on same page.
* Desired response time is 2 seconds. Anything longer, and you will start to lose a lot of users.

## Amazon - Example
* Want to modularize templates - Calls to web services that return in a JSON format.
* JavaScript then assembles data in memory and returns in html to div.

## REST 
* Representation - Resources all over the world, you ask for one and that will be conveyed to you in some sort of representation
  * Resources referenced by URIs (URLs are URIs)
  * CRUD operations 
    * Create 
    * Read
    * Update
    * Destroy
* State
* Transfer

#### Users Example
* /users/{id}
  * GET - Read current state of users or get user id
  * POST - Create a new user { data }
  * PUT - Update an existing user
  * DELETE - Destroy a user
  
#### Time example 
```ruby 
require 'sinatra' # dont use sinatra in production
require 'json'

configure do
	set :port, 3000
end

get '/api/1.0/whattimeisit' do
	{ status: true, message: Time.now}.to_json + "\n"
end

# curl http://localhost:3000/api/1.0/whattimeisit
  # will return json object: {"status":true,"message":"2015-01-15 13:38:16 -0700"}
```

#### Issues
* Database? How to persist info
* Ids 
* input/output
* errors




Lecture 3
=========
Josh Fermin - 1/20/2015

##Restful Web Services
* Architectural style for web services
	- Invented by Roy Fielding
* Approach to developing web services
* Service provides access to a linked set of resources
* For each resource you can perform operations on it simliar to the main operations

#### API Examples
```javascript
GET /api/1.0/users
//Retrieve list of users

GET /api/1.0/users/0
//Retrieve details of user 0

POST /api/1.0/users
//create a new user
```
* /api/1.0 is convention so that you can update your api
	* i.e. /api/1.1/users
	* keep your other api url running for transition

```javascript
PUT /api/1.0/users/0
Update user 0

DELETE /api/1.0/users/0
Delete user 0

GET /api/1.0/search?q=tattersail
Perform a search with the query tattersail.
```

#### Discussion - Request Response cycle
* Each operation may produce a result - Synchronously
	* With RESTful services, JSON format is king
* JSON - highly compressible
* Asynchronously - Use AJAX calls.
* POST and PUT methods typically send data
	* Also in JSON format
	* May be in the URL or in the body of the HTTP Request
		* GET, data may appear as query params
* Other formats are possible: HTML and XML are typical
* If a request needs to be authenticated
	* the authentication data appears in HTTP headers
	* i.e. used for DELETE - dont want just anyone to be able to delete users

#### Discussion - How do you think operations on two resources are handled?
```javascript
GET /api/1.0/posts/0/comments/1
Get the first comment on post 0

POST /api/1.0/posts/0/comments
Create a new comment on post 0
```

#### Issues
* Security: How do you authenticate users?
* Identity: How are ids assigned to resources?
* Failure: How do we handle failure sitatuions?
	* In example, handle it in JSON
	* Could have used http status codes
	* Most services use combination of both
* Persistence: How are resources stored?

#### EXAMPLE
* Contacts web service
* implemented in both ruby and javascript
* technologies used:
	* Sinatra
	* Rspec - Ruby testing
	* Typhoeus - http requests (libcurl wrapper)
	* Node - wrapper around chrome js engine (v8)
	* Express - alike sinatra


```ruby
def handle_request(method, uri, data = nil)
    url      = "#{base_uri}/#{uri}"
    info     = { "Content-Type" => "application/json" }
    params   = {method: method, body: data, headers: info }
    request  = Typhoeus::Request.new(url, params)
    request.run
    response = request.response
    raise NotConnected if response.code == 0 # not connected
    if response.code == 200 # successful request response cycle
      result = JSON.parse(response.body) 
      #puts result.inspect
      if result["status"]
        yield result["data"]
      else
        raise FailureResult.new(result["error"])
      end
    else
      raise NotOk
    end
    raise ServiceError
  end
```







Lecture 4
=========
Josh Fermin - 1/22/2015

##  Git Presentation
* Stages of file: untracked, unmodified, modified, staged, remote

#### Git Commands
* git init
* git clone
* git branch
* git checkout
* git add 
* git commit
* git merge


#### Github
* Branch
* Commit
* Pull Request - to add changes to production, 
* Discuss
* Merge
* Forking - creates a new project -> i.e. not a member of community yet, so add your changes to your own forked repo.



Lecture 5
=========
Josh Fermin - 1/27/2015

## Node.js 
* Most code in node is pacckaged inside of a module
* http is a core module, provided by Node itself
* npm (node package manager) - gives access to extend node

```javascript
// Load the http module to create an http server.
var http = require('http');

// Configure our HTTP server to respond with Hello World to all requests.
var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Hello World\n");
});

// Listen on port 8000, IP defaults to 127.0.0.1
server.listen(8000);

// Put a friendly message on the terminal
console.log("Server running at http://127.0.0.1:8000/");
```


#### Ex
```javascript
http.createServer(<function>).listen(1337, '127.0.0.1');
```
* returns an object with a method listen()
	* accepts server's network interface and port
	* starts the server running
* functions - first class objects
	* javascript is a functional language 
* Anonymous function - each time server receives a request
	* invokes this function and passes the http request and response objects
* This particular function ignores all input and generates a simple HTTP response.
* console.log() - printf()

#### Summary
1. Get http module
2. Create a server; register a function; start a server.
3. Print out a message

#### Event Loop
* In order to understand Node.js, you need to understand the event loop
* Event based programming is a programming style where the code you write is not in control
	* Always write handlers
	* If this event happens... this is what I will do
* This is how gui programs are written
* Basic structure of all Node.js programs
* Node tries to make it easy to add work to event queue 

#### Callback Hell
* to avoid callback hell 
	* use named callback functions 
	* then refer to them by name in the code that requires the callback.

#### Closures
```javascript
var multiple = function(multiple) {
	return multiply(num){
		return num * multiple;
	};
};

var byThree = multiple(3);
var byFour = multiple(4);

console.log("byThree(3): %d", byThree(3)); // prints 9
console.log("byFour(4): %d", byFour(4));  // prints 16
```

#### Node Execution Model
* Node is single threaded - for use written code.
* Any code you write is guaranteed to be synchronous
* You do not have to worry about race conditions
* IO is handled in parallel.
	* Every event is handled sequentially
	* Most events are small functions -> fast results (scalability)
	* can simulate parallelism

* If you issue an asynchronous call for IO
	* call back is registered
	* the io call is executed in a separate threads
* Easy to implement services that run server side
