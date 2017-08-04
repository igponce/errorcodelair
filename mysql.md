# Error codes for mySQL

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


