#Lecture notes for Data Engineering, Spring 2015

##Lecture 1 (Jan. 13th)

Other related classes: 
  Greg Greenstreet (VP of engineering at GNIP/Twitter): Big Data
  Dirk Grunwald: Datacenter-scale computing
  Tom Yeh: HCC Big Data Computing
  Christine Liu: Data Mining
  
relevant topics/issues:
  social networks
  data analytics (sampling/statistics, machine learning)
  storage (NoSQL: document, graph, key-value, columnar)
  data collection & cleaning 
  info visualization (D3, tableau, excel, R)
  
'data modeling is wicked hard' (this can make optimizing/migrating NoSQL difficult & time-consuming)
twitter uses utf8 character encoding

The Data Lifecycle:
  question (curation (deciding which data sources to use & how), triage (i.e. prioritization), persistence, and preservation of data) -> collection (may include generation) -> clean-up -> storage -> processing/analysis -> query, visualize, & act upon data
  
most user interaction with big data is through web apps

http request types: GET, POST, PUT, DELETE
request-response cycle 

***

##Lecture 2 (Jan. 15th)
GitHub Markdown
  generally, Markdown converts plaintext to richtext
  - use pound symbol to create headers
  - for italics, use asterisks or underscore around text
  - for bols, use double asterisks or underscores
  - use asterisk and space to create bullet points
  - use numbers for ordered lists
  - use 3 back ticks ``` to open and close code blocks; specify code language after opening ticks to highlight text
  - pipes define columns for tables
  - 3 hypens, asterisks or underscores create a horizontal line across the page

checkout daringfireball website for an example of a website written in markdown

webbrowser sends http requests (usually GET or POST) to web server; web server uses a handler to process request

img and script tags in the html may require additional requests to be sent out

index.html is a service

in big data, nearly everything is packaged into a service


REST (representation state transfer) is an architecture used for building service-based systems
  - resources are found on the web with a URI (URI describes a class of links that are larger than URLs)
  - CRUD (create, read, update, destroy)
  - GET is a read operation (gets current state of the resource)
  - POST is a create operation (passes data and updates the resource)
  - PUT is an update (updates a particular ID of the resource)
  - DELETE is destroy operation
  
Sinatra is a tool that provides a pretty simple way to start prototyping webapps

checkout contacts (on course site) for an example web service implementation 


**things to consider**
- where are you storing the info (database?)?
- how do you do IDs?
- what input do you take for POST, PUT, DELETE?
- what is the output format?
- how do you handle errors?

##Lecture 3 (Jan. 20th)

Restful web services

Rest is an approach to developing web services that mimics the design of the web itself
- for each resource, you can perform operations on it similar to the main operations/methods of HTTP
- best practice: put version number in url (prevents conflicts with future updates/versions)
- generally send data in JSON format (javascript object notation)
- for GET, data may appear as query params
- if a request needs to be authenticated, the authentication data appears in the HTTP headers

How are operations on two resources handled?
- GET /api/1.0/posts/0/comments/1  (gets 1st comment on post 0)
- POST /api/1.0/posts/0/comments (creates new comment on post 0)
- alternate approach: while performing an operation on one resource, you reference other resources (by id) in the data that is sent with the request)
- 

issues
- security: how to authenticate users
- identity: how are IDs assigned to resources
- failure: how do we handle failures?
-- can handle in JSON, HTTP status codes (404, 500, etc.), or a combination of the two
- persistence: how to store resources

- sinatra- handle HTTP requests & send data back (web-service framework) (in service.rb)
- rspec- testing framework that uses behavioral-driven design (can find online resources for things that can be tested with rsped) (in test files)
- typhoeus- ruby lib-curl library
- node- server-side javascript (wrapper around chrome web browser)
- express- framework for node

sinatra-style: create own class as sub-class of Sinatra::Base

goal of web-services: to implement a service in any language/framework and allows clients to use that service in any language/framework (i.e. standardize on messages)

##Lecture 4 (Jan. 22nd)
GIT

workflow 
untracked - not in repo
unmodified- not changed since current head
modified- changed since head
staged

git branch commands do not move you into that branch...use git checkout
git checkout *commit* (move the current state of your copy to a particular commit)
git merge *branch_name* (merges the named branch into the current branch)

how to resolve conflicts: 
remove markers & choose lines of code which should not be in the result of the merge

git status lets you see status of files in repo; -b tag also provides name of current branch

non-tracked files: 
executables 
use file called .gitignore 
	all file paths relative to the ignore file
	*.exe will ignore all executables
	foo is a file; foo/ is a directory

additional commands:
log
remote
stash
rebase (changes how branches are related)
diff
fetch (gets changes, but doesn't integrate them)
reset (move current head)
tag
mv (mofes file location within a repo)
rm (stops tracking changes to file)

pull, merge, pull
if you branched a repo, and changes were made to the master before you merge the branches, 
checkout new branch, do git merge master to get changes to master on the new branch. Fix merge conflicts, do another git merge master to make sure that no further commits were made to master. Then checkout master branch and do git merge new_branch to get all of the changes in the other branch into the master branch


GITHUB
use branches, not rebasing in order to accurately represent history
master represents production-ready code; use new branches to add features
pull request- use @mention ; 
fork is an exact copy of a repo (useful if you don't have collaborator access to repo)
github has keywords in commit messages (look online for resources) for example, 'close #xx' might close an issue that had been previously brought up

github.com/zandrr/pull_requests_demo   (try forking repo & doing a pull request)


Ken's demo
epic-analytics.cs.colorado.edu:4000
you know there is an endpoint called api/1.0/methods
i.e. epic-analytics.cs.colorado.edu:4000/api/1.0/methods
curl http://epic-analytics.cs.colorado.edu:4000/

make sure that I have curl installed for tuesday (or some networking library, such as typheous)

javascript implementation of the contacts service
if you type node *enter* in cmd line, you get a javascript prompt (lets you play with js outside of browser)
node is wrapper of chrome js implementation
node creates a module for js, so that you are not writing into the global namespace, avoiding any potential problems that could occur from that

##Lecture 5 (Jan. 27th)
checkout active record for ruby data storage (or maybe sqllite or mongo for homework1)
node.js (functional programming language)

most code is packaged in a module (http is a core module)

http provides a function called createServer() that implements server's behavior (the .listen() function starts the server)

can create in-line anonymous functions

console.log == 'printf() of the jafascript world'

node shuts down when there is no future work to be done... in this case, the .listen() function will loop forever

event loop: uses handlers, ie. write code that says "when event x happens, do y" 

process.nextTick() : don't process things in the current event loop, process them during the next event loop

setImmediate() is similar to .nextTick() but process.nextTick() will prioritize your function before IO-related callbacks (possibly causing IO starvation); with setImmediate() the IO is processed before starting the next event loop

setTimeout() takes a parameter that specifies how long to wait before function is executed

setInterval() takes param that specifies the interval at which the function is executed (if interval is never turned off, the program will execute forever)

callback hell: where callbacks get indented; to solve: 1) use synchronous functions (frowned upon; this can cause delays in running system, because the system waits until each synchronous call is done executing), or 2) use named callback functions (define functions individually & refer to them by name later in code)

closure: reference variable that is not in the function (i.e. outside of scope); function will allow you to pass in some params that get 'remembered', so that you can later use that function and need to only pass in the last param needed

other asynchronous mechanisms: event emitters, promises

for user-written code, node is single threaded (built ontop of librariy called libevent which handles IO in parallel); everything is handled in eventloop

epic-research.cs.colorado.edu:8080/ 
/api/1.0/methods
steps:

curl .../1.0/methods

curl .../1.0/first

curl .../1.0/second

curl .../1.0/third

curl .../1.0/what

curl  -X POST --data '{"value":1000, "author": "Heather"}" .../1.0/answer

cu-data-engineerint-s15.github.io/lecture_05

##Lecture 7 (Feb. 3rd)
Angular JS (version 1.3; version 2.0 will be released soon & may be very different)

- client-side web app framework
- designed to live on top of JSON_based RESTful web services
- Data bindings; value of HTML tag can be associated w/ model object; when one changes, angular will update the others
- Controllers; defines all states and methods that can be accessed within that section of the page. Allows you to modularize your web app.
- Services; use if you need to maintain state between invocations of a controll, or if you need to share state between two different controllers (created when angular app is initialized & remain in place for entire life of app)
- Directives; allows angular to integrate into HTML in a natural way, but can also be used to create reusable components that combine controllers, data and HTML
- Embeddable; can control as much or as little of web page as you want. 
- Injectable; rather than use a main route to wire everything together, Angular rontrollers and services declare their dependencies up front; then it is up to the angular run-time system to locate the dependencies and inject them into the appropriate areas
- 
-modules:
```angular.module('contactsApp', []) ```
- this creates a module called contactsApp which has no dependencies
- this returns the previously created module: ```angular.module('contactsApp')```
- ng-show displays HTML if condition is true
- ng-hide hides HTML element if condition is true
- ng-repeat iterates over a collection


Book: AngularJS UP & Running

--Lecture 9 (Feb. 10th)

Checkout gist about this vs. self in javascript
- default binding
- implicit binding
- explicit binding

books:
- principles of object-oriented javascript
- secrets of the javascript ninja

this always points to the context of the currently executing function (what function calls it, not necessarily scope)

in javascript, blocks do not create scope, functions create scope


http://solutionoptimist.com/2013/09/30/requirejs-angularjs-dependency-injection/


requireJS:
- load single js file in index.html (i.e. RequireJS)
- all files loaded by requireJS are loaded in parallel


immediately invoked function expression (IIFE):
- define a function & call it immediately


Getting data from twitter:
- first, create a twitter account
- https://dev.twitter.com
- when creating a new app, use my github url for website...
- permissions --> change to read & write
- keys & access tokens --> 'create my access token'
- on dev.twitter.com --> developers/documentation/rest apis

##Lecture 10 (Feb. 12)
- consumer key ids a particular app/developer
- access token ids a particular user
- an app may act on behalf of many users (you may ship app with consumer keys but no access tokens)
- when you create twitter app and generate access token, you short-circuit the web-based authentication process
 
- recommendation: use rbenv to manage ruby (allows you to easily manage multiple ruby installations)
- see slides for instructions to get this all set up
- in ruby, .fetch will throw an error if the info it is fetching is not there
- ruby strings are mutable; typhoeus prefers immutable strings as keys

-check out jq tool

## Lecture 11 (Feb. 17th)

Twitter data collection framework

twitter_framework repo on course website has all of the code needed to collect twitter data
- automatically handles twitter rate limits
- implements techniques described in 'working with timelines' and 'working with cursors' sections of the twitter api documentation
- 'NotImplementedErrer' is a standard used in ruby
- tweet ids are very large (~64 bit, larger than max ints allowed in java)
- twitter queries use 'sentiment-analysis', i.e. if you query something and add ':)' you get positive tweets about that something
- twitter uses '\r\n' at message boundaries
- streaming api represents ~1% of all tweets; streaming api has no rate limit
- ruby ../apps/get_tweets.rb -p ../oauth.properties kenbod
- ruby ../apps/(filenae) -h will tell you what args the file wants
- ruby ../apps/find_tweets.rb --props ../oauth.properties 'movie -scary :)' (this finds tweets that have the word movie, not the word scary, and are positive)
- ruby ../apps/get_followers.rb --props ../oauth.properties username (will get all the userids that are following the requested user)


There is a description for homework 3 
checkout the twitter rest api documentation page & find one of the GET or POST requests for ideas

##Lecture 12 (Feb. 19th)

Intro to NoSQL

- Books: Big Data: Principles and best practices of scalable realtime data systems; Making Sense of NoSQL: A guide for managers and the rest of us
- vertical scaling
- sharding a database: need multiple copies of the database & partition data across those databases 
- scaling relational databases introduces significant complexities to the back-end (fault tolerance is hard; complexity is pushed to the application layer; lack of human fault tolerance; maintenance is a lot of work)
- NoSQL databases are aware of their distributed nature; they manage sharding and replication for you & are horizontally scalable
- NoSQL databases avoid mutable data
- service tier/application code is completely unaware of any resharding etc. that goes on behind-the-scenes
- types of NoSQL: key-value, graphs, columnar, documents
- key-value: when presented with a string (key) will return an arbitrarily large set of data (value); there is no query language; just acts like a hash table (ex: amazon simple db, s3, redis, voldemort, riak)
- graph: optimized to store graph structures; provides structural query languages (ex: neo4j, titan, infinite graph, infogrid)
- columnar - aka column family stores; can scale to enormous amounts of data; column family is a table & consists of rows that have unique row keys, rows consist of columns, columns consist of a key and a value; basically a distributed hash table which is easy to partition across the nodes of a cluster (ex: cassandra, hbase)
- document: similar to key-value store; insert documents (a bag of key-value pairs) & each document is indexed in various ways; documents can then be queried on an attribute; documents can be grouped into collections, collections can be grouped into databases (ex: mongodb, couchdb, solr/lucene)


-in NoSQL, there is no schema; you are free to store anything in the database (no specification of data-type)
