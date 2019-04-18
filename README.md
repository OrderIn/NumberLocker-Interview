<h1>Number Locker Service</h1>

<h2>Context:</h2>

You will need to create 2 micro services at minimum. API & Worker.

<h3>API.</h3>

This service will need to allow a person to interact with the data model via API calls. One of the endpoints should allow someone to submit a JSON item `{"item":"foo"}` to it like below:

> /save

``` bash
curl -d '{"item":"foo"}' -H "Content-Type: application/json" -X POST http://localhost:3000/save
```

Multiple submits of the same value **IS ALLOWED**, **only if** the record **IS OPEN**, and the system should keep track of their submit count.

The other endpoint should allow a person to read the amount of times a value was submitted as well as their current status.

>/status/{item}

```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:3000/status/foo
```

This API should return the following.
```JSON
 { "item":"foo", "status": "locked", "count": 3 }
```
where 
* `item` is the `submitted value` 
* `status` is either `open` or `locked` 
* `count` is a number `> 1` (based on the amount of times the item was submitted before being locked)


<h3>Worker</h3>
This service should after 1 minute, flag the record as processed, after which, IF a person tries to submit to the API `/save`  endpoint again, it should return a status code `400` bad request.

The locking service can be sceduled uing a CRON or manual loop of sorts... but I'd suggest using something like Hangfire or cronos. Cheatcode: the cron for every minute is: `* * * * *`

-----

<h2>Instructions:</h2>

1. Create an **EMPTY** github repo and add our reviewers as collaborators **BEFORE** your first commit.
    * Smaller regular commits are beter 
2. Look into the `dotnet new` command (or use VisualStudio templates)
    * https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new?tabs=netcore21
      * Console 
      * WebAPI
3. Create (at the LEAST) 2 microservices
      * API
      * Worker
4. Persist data in whatever means you feel is appropriate that will allow both "core" applications to access them.
      * Local or Hosted SQL or NoSQL with any SDK or ORM of your choosing 
      * A basic common FlatFile
      * Any other cloud storage
        * Firebase, other?
      * You could have a 3rd linking service that acts as a state manager
        *  E.g. a service that allows you to read/write in memory items to it.

<h2>Finally</h2>
The goal of this is not nessicarily to have a fully function application eco system, but I'd like to see how you structure & write code and if you'll be able to work comfrotably within our code bases.

Goodluck :)
