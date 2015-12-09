This is a very simple language to define Simplifier connectors. Currently this is just a proposal. It might never need a parser. It is jsut an easy way to explore the necessary options to implement a connector system that allows everyone to build Connectors.

We define the following:
- commands like SERVER.READ, BROWSER.GET. 
- variables (identifiers) starting with #, like #patient
- place holders: { }
- body notation [ ]
- capture notation >>

Basic commands
==============
We have two types of commands: internal server operations and commands for the client / browser
SERVER.GET <url> >> #<identifier>
SERVER.POST <url> [#<identifier>] 
SERVER.PUT <url> [#<identifier>]

Fhir Commands
=============
Based on https://www.hl7.org/fhir/http.html
FHIR.READ - a SERVER.GET with FHIR headers etc.
FHIR.UPDATE - a SERVER.PUT with FHIR headers
FHIR.CREATE - a SERVER.POST with FHIR headers


