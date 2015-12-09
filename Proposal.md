This is a very simple language to define Simplifier connectors. Currently this is just a proposal. It might never need a parser. It is just an easy way to exploreand design the necessary features to implement a connector system that allows everyone to build Connectors.

We define the following:
- commands like `SERVER.READ`, `BROWSER.GET`. 
- variables (identifiers) starting with #, like `#patient`
- place holders: `{ }`
- body notation: `[ ]`
- body capture notation: `[ ]`

Notice that each connector must have a unique name.

Server reachout commands
========================
We have two types of commands: internal server operations and commands for the client / browser. These are the server commands:
```
SERVER.GET <url> [#<identifier>]  - store the body in a new variable 
SERVER.POST <url> [#<identifier>] - posts a variable in the body of a post command
SERVER.PUT <url> [#<identifier>] 
```

Fhir Commands
=============
There is a special type of server commands for FHIR. Based on https://www.hl7.org/fhir/http.html
```
FHIR.READ // a SERVER.GET with FHIR headers etc.
FHIR.UPDATE // a SERVER.PUT with FHIR headers
FHIR.CREATE // a SERVER.POST with FHIR headers
```

Browser/Client commands
=======================
An OUTGOING command 
After the command link has been clicked and the internal commands have been processed, the connector allows a redirect get or post.
```
BROWSER.GET <url> // 
BROWSER.POST <url> [#<identifier>]
```

Incoming connector
==================
An incoming connector must start with a `INCOMING` statement. It has a name for online discovery. It can define variables in the url query. The name of the query field will be the same as the variable. The body can be captured in a variable with the body capture notation `[ ]`.

The following example is an incoming connector that can be activated through a POST url and must have a body
```
  http://simplifier.net/connector/ReceivePatient?callback=... 
```

receives a url with one parameter that is stored in the #callback variable and a body that is stored
```
INCOMING POST ReceivePatient ? #callback [#patient] 
```

NOTE : We have to distinguish between a browser incoming and an API incoming. For now we wont allow underwater API calls

Predefined variables
====================
```
{here} - the current page url
{endpoint} - the fhir endpoint of the resource on the current page
{callback:<name>} - a call back url for an incoming connector
```

Example connectors
==================

#### Externally edit an instance of the StructureDefinition on the page.
```
BROWSER.GET http://clinfhir.com/editpatient?structure={endpoint}&instance=http://fhir.org/
```

#### Fetch a resource from one server and post it to a another server
```
FHIR.READ http://spark.furore.com/fhir/Patient/example >> #patient
FHIR.CREATE http://fhirtest.uhn.ca/baseDstu2/Patient [#patient]

INCOMING ? #callback [#patient]








