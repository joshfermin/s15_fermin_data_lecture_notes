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


