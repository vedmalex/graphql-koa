GraphQL Koa Middleware
==========================

#### Development in progress. Currently not working. Just reserved the package name.

This is just a simple conversion of [express-graphql](https://github.com/graphql/express-graphql) for Koa.
This package has not been thoroughly tested.
We are actively looking for contributors to help with testing.

Create a GraphQL HTTP server with [Koa](http://koajs.com).

```sh
npm install --save koa-graphql
```

Install koa-graphql as middleware in your koa server:

```js
var graphqlHTTP = require('koa-graphql');

var app = koa();

app.use('/graphql', graphqlHTTP({ schema: MyGraphQLSchema }));
```


### Options

The `graphqlHTTP` function accepts the following options:

  * **`schema`**: A `GraphQLSchema` instance from [`graphql-js`][].
    A `schema` *must* be provided.

  * **`rootValue`**: A value to pass as the rootValue to the `graphql()`
    function from [`graphql-js`][].

  * **`pretty`**: If `true`, any JSON response will be pretty-printed.


### HTTP Usage

Once installed at a path, `koa-graphql` will accept requests with
the parameters:

  * **`query`**: A string GraphQL document to be executed.

  * **`variables`**: The runtime values to use for any GraphQL query variables
    as a JSON object.

  * **`operationName`**: If the provided `query` contains multiple named
    operations, this specifies which operation should be executed. If not
    provided, an 400 error will be returned if the `query` contains multiple
    named operations.

GraphQL will first look for each parameter in the URL's query-string:

```
/graphql?query=query+getUser($id:ID){user(id:$id){name}}&variables={"id":"4"}
```

If not found in the query-string, it will look in the POST request body.

If a previous middleware has already parsed the POST body, the `request.body`
value will be used.

If the POST body has not yet been parsed, koa-graphql will interpret it
depending on the provided *Content-Type* header.

  * **`application/json`**: the POST body will be parsed as a JSON
    object of parameters.

  * **`application/x-www-form-urlencoded`**: this POST body will be
    parsed as a url-encoded string of key-value pairs.

  * **`application/graphql`**: The POST body will be parsed as GraphQL
    query string, which provides the `query` parameter.




[`graphql-js`]: https://github.com/graphql/graphql-js
