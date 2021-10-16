**typed-sql-db**
This is meant to be a wrapper around any SQL database (mysql, sqlite, PostgreSQL, etc).
It's easy to type and use, and agnostic to any SQL driver you want to use.

***Usage***

The main constructor is DatabaseTable, exported both as a named class and a default.
All you need to pass it is the table name, the primary key of the table, and a query function.

For example:

```ts
import {DatabaseTable} from 'typed-sql-db';
import * as pg from 'pg';

const pool = new pg.Pool(config);

interface DBSchema {
    userid: string; // primary key
    username: string;
    ip: string;
    registertime: Date;
    rank: number;
}

// table name, primary key id, query function
const db = new DatabaseTable<DBSchema>('users', 'userid', sqlStatement => {
    return pool.query(query.text, query.values);
});

```

**Functions**

```ts
// Selects one row. Pass the keys you want to select.
// where clause is for optional stuff like ON DUPLICATE KEY
selectOne(entries: string | string[], where?: SQLStatement): Promise<Partial<T> | null>;

// select all results. pass the keys you want to select, with an optional where clause.
selectAll(entries: string | string[], where?: SQLStatement): Promise<Partial<T>[]>;
// select one row matching a primary key. pass the columns you want and the primary key
// value
get(entries: string | string[], keyId: SQLInput): Promise<Partial<T> | null>;
// pass an object of {[key: string]: value you want it set to}.     
updateAll(toParams: Partial<T>, where?: SQLStatement, limit?: number): Promise<Partial<T>[]>;
// see above, but update one row
updateOne(to: Partial<T>, where?: SQLStatement): Promise<Partial<T>[]>;
// delete all rows with a matching WHERE
deleteAll(where?: SQLStatement, limit?: number): Promise<Partial<T>[]>;
// delete one row with a primary key col matching the given key
delete(keyEntry: SQLInput): Promise<Partial<T>[]>;
// delete one row matching the where
deleteOne(where: SQLStatement): Promise<Partial<T>[]>;
// insert one row. colMap is {[key: string]: value you want to insert}.
insert(colMap: Partial<T>, rest?: SQLStatement, isReplace?: boolean): Promise<Partial<T>[]>;
// see above, but REPLACE instead of INSERT
replace(cols: Partial<T>, rest?: SQLStatement): Promise<Partial<T>[]>;
// update the column matching the primary key given with the {[key: string]: value} object.
update(primaryKey: SQLInput, data: Partial<T>): Promise<Partial<T>[]>;
```

