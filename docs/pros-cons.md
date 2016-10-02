## Benefits

[Join Monster](https://github.com/stems/join-monster) generates the SQL dynamically. The SQL queries will **adapt** not only with the varying complexities of queries, but also changes in the schema. No back-and-forth from web server to database. No need to get convert the raw result to the nested structure. Lots of benefits for a little bit of configuration.


<input type="checkbox" checked>Fetch all the data in a single database query.<br>
<input type="checkbox" checked>No over-fetching.<br>
<input type="checkbox" checked>Automatically generate adapting SQL queries.<br>
<input type="checkbox" checked>Automatically build the nested object structure.<br>
<input type="checkbox" checked>Easier maintainability.<br>
<input type="checkbox" checked>Coexists with your custom resolve functions and existing schemas.<br>

## Cons

There are some constraints on how your data model looks. We need to be able to make some assumptions in order to sensibly translate queries from a hierarchical structure to a relational one. Not all schemas will be able to allow [Join Monster](https://github.com/stems/join-monster) to handle all the data fetching, perhaps not in whole but in part.

We try to produce ANSI-compliant SQL. We currently do not specialize for any particular dialects. Hence, it won't always produce be the optimal way to query on every SQL database. Because you aren't writing the queries yourself, you lose some freedom to do some performance tuning. [Joins Monster](https://github.com/stems/join-monster), however, remains transparent about what SQL it actually produces. You can `console.log` it or run your Node.js process with the environment variable `DEBUG=join-monster` for some useful diagnostic information. Furthermore, you can use our [custom version of GraphiQL](https://github.com/acarl005/graphsiql) used in the demo to visualize the SQL.

![graphsiql](img/graphsiql.png)

## Alternatives

One way to not worry about how your API fetches data from the database is not not create a custom API in the first place. If you're using PostgreSQL, the entire API aith authentication can be generated via reflection using [PostGraphQL](https://github.com/calebmer/postgraphql). But you might still want to create a custom API.

### DataLoader

[DataLoader](https://github.com/facebook/dataloader) is Facebook's own solution for batch requests. It also caches individual entities. It's a simple and general tool for data fetching.

[Join Monster](https://github.com/stems/join-monster) is able to specialize for SQL, so you can avoid manually writing the queries (more configuring less programming). Data loader only batches requests for one resource, e.g. batching requests for users, or posts, etc. Join Monster can batch **everything** into one trip.

DataLoader derives its performance benefit from caching. Caching opens a can of worms. You have to think about cache invalidation, memory pressure, loading the cache, etc. For us, the batching was such a win that our performance issues were solved. The cache is complexity that we didn't need. This means a **simpler** mental model.