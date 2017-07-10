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
