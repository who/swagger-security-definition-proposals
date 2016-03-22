# Swagger securityDefinition proposals
This repository holds Swagger spec proposals for extending security definitions.

### Proposal: customHash

The **customHash** security definition would allow the spec to define `n` fields that need to be concatenated and passed through `Y` hash algorithm, the output of which is stored in `Z` location.

#### Example customHash spec

```
swagger: '2.0'
info:
  version: 1.0.0
  title: Simple API
host: example.com
securityDefinitions: 
  customHash: 
    algorithm: "HMACSHA256"
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
      name: "X-SECURITY-HASH"
      in: "header"
      description: "Holds the hashed value"
paths:
  /foobar:
    get:
      security:
       - customHash: []
      responses:
        200:
          description: OK
```

Using the spec above, in order to HTTP GET example.com/foobar, the caller would have to concatenate a timestamp, the full URI of the request, a secret key and a string of the HTTP body (empty string in this case, as it's an HTTP GET). After concatenation, the resulting string would be passed into the hashing function specified above, HMACSHA256. The resulting value of the hashing step would then be placed into an HTTP header parameter with a key of `X-SECURITY-HASH`.


