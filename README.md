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

  
  
