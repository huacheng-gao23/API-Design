# API-Design
I am creating these notes not only for my own knowledge, but to share with others.

## Fundamentals
API stands for Application Programming Interface. It represents a contract for communication between two applications.

### Types
#### REST
- Stands for Representational State Transfer
- Standard HTTP Methods (GET, POST, PUT, DELETE)
- Resource-based URLs for endpoints
- Typically use JSON
- Stateless
- Good for public APIs/web services
- Pros: simple, widely understood, standard HTTP, scalable
- Cons: Can require multiple requests to fetch all data, potential over/under fetching

#### GraphQL
- Single endpoint that accepts queries
- Client specifies the exact data they want
- Strongly-typed schema
- Get multiple resources in one request
- Good for flexible data requirements or mobile apps
- Pros: precise data fetching, reduced network requests, self-documenting
- Cons: More complex setup, performance issues with complex queries

#### gRPC
- Stands for Google Remote Procedure Call
- uses HTTP/2 protocol
- Binary data format (Protocol buffers)
- Strongly-typed contracts with .proto files
- Supports streaming
- Good for microservices and internal service communication
- Pros: Fast, good for microservices, good for streaming
- Cons: Not easily browser-compatible, steeper learning curve

### REST Principles
1. Client-Server Separation
    - Client and server are independent and can evolve separately
2. Stateless
    - No session dependency, each request contains all needed information
3. Cacheable
    - Responses must define themselves as cacheable or not (Cache-Control header)
4. Uniform Interface
    - Consistent resource identification, descriptive messages
5. Layered System
    - Clients don't know which exact server they hit
    - Load balancing, cache layers
6. Code on Demand
    - Server can extend client functionality, eg sending Javascript to clients

#### Naming Conventions
- URL Structure and Hierarchy
    - Represent relationships with nesting
- Query vs Path Parameters
    - Path Params: used for identifying resources, part of the resource hierarchy
    - Query Params: used for filtering, sorting, or pagination. Handles optional params

### Pagination Strategies
- Offset Pagination: ```GET /api/users?limit=10&offset=20```
    - Pros
        - Simple to implement
        - Can jump to any page directly
        - Good for small, static datasets
    - Cons
        - Performance issues with large datasets
        - Invonsistent with real-time data
        - Skipping large offsets is expensive
        - Can miss items or show duplicates if data changes
- Cursor Pagination: ```GET /api/users?limit=10&cursor=abc123```
    - Pros
        - Consistent results with real-time data
        - Good for timeline-based data
        - Better performance for large datasets
        - More scalable
        - No skipped or duplicate items
    - Cons
        - Can't jump to specific pages
        - No total count
        - Bookmarking specific pages is not possible
        - Needs a stable sort criteria

### Filtering, Sorting, and Field Selection
- We can leverage query parameters for filtering, sorting and field selection
- Filtering: ```GET /api/products?price_min=10&price_max=100```
- Sorting: ```GET /api/users?sort=name,-created_at``` (+) ascending, (-) descending
- Field Selection: ```GET /api/users?fields=id,name,email```

### Security
#### Authentication
- JWT
    - Stands for JSON Web Token
    - Stateless Authentication
    - Consists of 3 parts: Header, Payload, Signature
    - Tokens are signed to prevent tampering
    - Flow: Client auths with the server -> server creates JWT with user data -> server sends JWT to client -> client sends JWT with each request -> server validates the JWT signature
    - Pros
        - No server storage needed
        - No DB lookups
    - Cons
        - Can't invalidate until the token expires
        - Each token carries all data
- Session
    - Flow: Client auths with the server -> server stores session data in DB -> server sends session id to client (stored in a cookie) -> client sends cookie with each request
    - Pros
        - Server must store session data
        - Can instantly invalidate a session
    - Cons
        - Requires DB/cache lookups
        - Takes server memory
- JWT vs Sessions
    - Server Storage: JWT requires no server storage like sessions, but we cannot immediately invalidate the token until it expires
    - Scalability: JWT is better for horizontal scaling as it's inherently stateless. Sessions require either sticky sessions or shared storage.
    - Security: We can invaliate sessions immediately. For JWTs, we have to wait until token expiry, JWTs also have token hijacking risks, but sessions have CSRF vulnerabilities with cookies.
    - Performance: Sessions require a DB/cache lookup for every request, however the cookie size is usually small and it requires less bandwidth per request compared to JWTs.

    Sessions are good when we need immediate invalidation, or our data is sensitive. JWTs are good for distributed systems as they are more horizontally scalable due to their stateless nature.

#### Partial Responses
We can leverage query parameters to pass in the fields we would like to retrieve from our API. This will give us performance benefits as we only transfer the necessary data, rather than large payloads.

```
// Without partial response
// Transfers unnecessary data
GET /api/users
{
  "id": "123",
  "name": "John",
  "email": "john@example.com",
  "address": {...},
  "preferences": {...},
  "history": [...],
  // Many more fields
}

// With partial response
// Only transfers needed data
GET /api/users?fields=id,name
{
  "id": "123",
  "name": "John"
}
```

Best Practices
- Default fields
- Field validation
- Error handling

***

Documentation

API documentation formats (OpenAPI/Swagger, RAML)
Writing clear endpoint descriptions
Request/Response examples
Error documentation
Versioning documentation


Versioning

Version control strategies
URL versioning vs header versioning
Breaking vs non-breaking changes
Deprecation policies
Backward compatibility


Performance

Caching strategies
Compression
Batch operations
Rate limiting implementation
Response time optimization


Best Practices

Idempotency
Statelessness
Consistency in design
Partial response/updates
Bulk operations
HATEOAS (Hypermedia)