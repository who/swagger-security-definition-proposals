# Swagger securityDefinition proposals
This repository holds Swagger spec proposals for extending security definitions.

### Proposal: customHash

The **customHash** security definition would allow the spec to define `N` fields that need to be passed through `Y` hash algorithm, the output of which is stored in `Z` location.

```
swagger: '2.0'
info:
  version: 1.0.0
  title: Simple API
host: example.com
securityDefinitions: 
  basicAuth:
    type: basic
    description: HTTP Basic Authentication. Works over `HTTP` and `HTTPS`
  customHash: 
    algorithm: "sha256"
    inputs: 
      timestamp: 
        type: "integer"
        format: "int64"
      uri: 
        type: "string"
        description: "The full URI of the request"
      body: 
        type: "string"
        description: "The full string body of the request"
      secret:
        type: "string"
        description: "The shared secret key"
    outputs: 
      name: "myHash"
      in: "header"
      description: "Holds the hashed value"
paths:
  /foo:
    get:
      security:
       - basicAuth: []
      responses:
        200:
          description: OK
  /bar:
    get:
      security:
       - customHash: []
      responses:
        200:
          description: OK
```
