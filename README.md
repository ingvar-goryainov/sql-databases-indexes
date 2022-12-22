# MySql InnoDB testing

### Test sysvar_innodb_flush_log_at_trx_commit parameters while inserts

first 10 000 records:

|           | value = 1 | value = 2 | value = 0 |
|-----------|-----------|-----------|-----------|
| Time, sec | 54        | 42        | 22        |


first 100 000 records:

|           | value = 1 | value = 2 | value = 0 |
|-----------|-----------|-----------|-----------|
| Time, min | 8:40      | 6:22      | 6:15      |


### Test Btree and Hash Indexes

Table schema:

```
create table users
(
    id         int auto_increment
        primary key,
    name       varchar(255) not null,
    birth_date date         not null,
    created_at datetime     null,
    constraint users_id_uindex
        unique (id)
);
```

Overall Number of records: 40 000 000

Example of query:

```
SELECT * FROM users WHERE birth_date = '2018-05-03';
```

Example of query creating index:

```
CREATE INDEX date_hash ON users (birth_date) USING HASH;
```

Results:


|       | no-index    | btree  | hash   |
|-------|-------------|--------|--------|
| Time  | 14 s 678 ms | ~200ms | ~200ms |

If present both of index mysql always used btree ones
