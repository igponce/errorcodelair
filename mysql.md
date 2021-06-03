# MySQL Error code lair

## Error Code: 1005. Can't create table

MySQL could not create a table.
Two possible cases: 
- Foreign key constraints (does the FK exist before creation?)
- Are you renaming an internal table ?

## Error Code: 1044. Access denied for user 'user'@'10.%.%.%' to database 'databaseName'

Troubleshooting

- DatabaseName: In mySQL database names are CaSe SeNsItIvE.
- Username incorrect?
- And finally.. permissions.

This is a quick fix for permissions:
```sql
grant all privileges on databaseName.* to 'user'@'%';
```

## Error Code: 1075. Incorrect table definition; there can be only one auto column and it must be defined as a key

Troubleshooing: Error caused by copy-paste.

## Error: 1095 SQLSTATE: HY000 (ER_KILL_DENIED_ERROR)

Message: You are not owner of thread %lu 

Troubleshooting:

- Are you the owner of the thread you're trying to kill?
- Do you have sufficient permissions to kill a thread lauched by another user?

## Error Code: 1222. The used SELECT statements have a different number of columns

You are trying to make a UNION with two tables that have diferent number of columns.
How to reproduce:

```{sql}
SELECT 1,2,3,4
UNION
SELECT 1
```

Be careful with UNIONs and data types. You could end up with something unexpected
like the example below:

```{sql}
SELECT 1,2,3,4
UNION
SELECT 1.1, "hello", 3, 4
```


## Error Code: 1451. Cannot delete or update a parent row: a foreign key constraint fails ( details about the foreign key )

Looks like someone defined foreign keys in the database... 
People call this referential integrity. 
This means in order to delete/update the table you'll need to 
delete/update other stuff first.

## Error Code: 1630. FUNCTION *schema_name*.sum does not exist. Check the 'Function Name Parsing and Resolution' section in the Reference Manual

Looks like you have an space character between the function name and the opening parenthesis, like this:

```{sql}
SELECT sum (column) FROM ...
```

This applies not just to the sum function; but for any sql function.

How to reproduce:

Let's make a 3 row table from scratch:

```{sql}
SELECT 1 as aa 
UNION
SELECT 2 as aa 
UNION
SELECT 3 as aa ) 
```

```{sql}
SELECT sum(uu.aa) FROM ( SELECT 1 as aa union select 2 as aa union select 3 as aa ) uu;
``

```{sql}
SELECT sum (1,2) ;
```


## Error Code: 1701. Cannot truncate a table referenced in a foreign key constraint ( details about the foreign key constrain )

This is like error code 1451 above.
Foreign keys force you to update and remove data in a particular order.
Try to remove the data that refereces this table first.
Be warned MySQL can have cross-database constraints!


## Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.

This is a MySQL Workbench error to avoid deleting data unexpectedly.

If you want to delete/update a whole table don't look for the prefences option:
this error acts as your life vest and wi. The alternative is simple: just add 
*WHERE <key> NOT NULL* to your SQL statement.

## Error Code: 1427. For float(M,D), double(M,D) or decimal(M,D), M must be >= D (column 'whatever').

This happened trying to change a column from decimal(10,0) to decimal(3,7)...
... because I thought that (10,0) meant 10 digits total, zero decimal places
*instead* of 10 digits *total*, 0 of those are for *decimals*.

In case you're wondering how to store properly store longitude/latitude take a look at this table:

|  decimal places |  decimal<br>degrees | DMS         | qualitative scale that can be identified | resolution at equator |
|-----------------|---------------------|-------------|------------------------------------------|-----------------------|
| 0               | 1.0                 | 1° 00′ 0″   | country or large region | 111.32&nbsp;km |
| 1               | 0.1                 | 0° 06′ 0″   | large city or district  |  11.132&nbsp;km |
| 2               | 0.01                | 0° 00′ 36″  | town or village         | 1.1132&nbsp;km |
| 3               | 0.001               | 0° 00′ 3.6″ | neighborhood, street    | 111.32 m |
| 4               | 0.0001              | 0° 00′ 0.36″| individual street, land parcel | 11.132 m |
| 5               | 0.00001             | 0° 00′ 0.036″| individual trees       | 1.1132 m |
| 6               | 0.000001            | 0° 00′ 0.0036″| individual humans     | 111.32&nbsp;mm |
| 7               | 0.0000001           | 0° 00′ 0.00036″| practical limit of commercial surveying | 11.132&nbsp;mm |
| 8               | 0.00000001          | 0° 00′ 0.000036″| specialized surveying (e.g. [[tectonic plate]] mapping) | 1.1132&nbsp;mm |

Table source: [Wikipedia: Decimal Degrees](https://en.wikipedia.org/wiki/Decimal_degrees#Precision)


## Error Code: 2013. Lost connection to MySQL server during query

https://stackoverflow.com/questions/10563619/error-code-2013-lost-connection-to-mysql-server-during-query
