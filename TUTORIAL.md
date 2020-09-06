## Authentication

Nestjs has [built-in support for authentication](https://docs.nestjs.com/techniques/authentication). We've implemented
the toy authentication mechanism (i.e.&mdash;do not use in production!) that validates the username and password 
supplied in the request body against a static set of users.

The [DocumentController](./src/document/document.controller.ts) uses the [LocalAuthGuard](./src/auth/local-auth.guard.ts)
via the `@UseGuards(LocalAuthGuard)` decorator to protect paths that should only be accessible to authenticated users.

If you try to get documents with invalid user credentials:

    curl http://localhost:3000/document
    curl http://localhost:3000/document/1

you should receive an 401 Unauthorized error:

    {"statusCode":401,"message":"Unauthorized"}

If you try to get documents with valid user credentials, you will be granted access:
    
    curl -X GET http://localhost:3000/document/ -d '{"username": "john", "password": "changeme"}' -H "Content-Type: applicationjson"
    curl -X GET http://localhost:3000/document/1 -d '{"username": "john", "password": "changeme"}' -H "Content-Type: application/json"

## Authorization

If we want to go beyond simple authentication-based access controls, we need to implement a richer authorization scheme.