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

***
Topics to cover

Security

Authentication methods
Authorization (OAuth, JWT, API keys)
Rate limiting
CORS
Input validation
Security headers


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