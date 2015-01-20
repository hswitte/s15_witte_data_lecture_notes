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
