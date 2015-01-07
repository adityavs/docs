---
layout: api-command
language: JavaScript
permalink: api/javascript/config/
command: config
io:
    -   - r
        - object
---
# Command syntax #

{% apibody %}
r.table('tablename').config() &rarr; object
r.db('dbname').config() &rarr; object
{% endapibody %}

# Description #

Query (read and/or update) the configurations for individual tables or databases.

The `config` command is effectively a shorthand for accessing the `table_config` or `db_config` [System tables](/docs/system-tables/). It will return the single row from the system that corresponds to the database or table configuration, as if [get](/api/javascript/get) had been called on the system table with the UUID of the database or table in question.

__Example:__ Get the configuration for the `users` table.

```js
r.table('users').config().run(conn, callback);
// Result passed to callback
{
    id: "31c92680-f70c-4a4b-a49e-b238eb12c023",
    name: "users",
    db: "superstuff",
    primary_key: "id",
    shards: [
        {primary_replica: "a", "replicas": ["a", "b"]},
        {primary_replica: "d", "replicas": ["c", "d"]}
    ],
    write_acks: "majority",
    durability: "hard"
}
```

__Example:__ Change the write acknowledgement requirement of the `users` table.

```js
r.table('users').config().update({write_acks: 'single'}).run(conn, callback);
// Result passed to callback
{
    id: "31c92680-f70c-4a4b-a49e-b238eb12c023",
    name: "users",
    db: "superstuff",
    primary_key: "id",
    shards: [
        {primary_replica: "a", "replicas": ["a", "b"]},
        {primary_replica: "d", "replicas": ["c", "d"]}
    ],
    write_acks: "single",
    durability: "hard"
}
```