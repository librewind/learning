# Transaction Isolation Levels

## MySQL (Percona 8.0.29-21)

|                  | Dirty reads | Nonrepeatable reads  | Phantom reads | Lost updates |
| :--------------- | :---------: | :------------------: | :-----------: | :----------: |
| Serializable     | no          | no                   | no            | deadlock     |
| Repeatable reads | no          | no                   | no            | yes          |
| Read committed   | no          | yes                  | yes           | yes          |
| Read uncommitted | yes         | yes                  | yes           | yes          |

## PostgreSQL 14.6-1.pgdg110+1

|                  | Dirty reads | Nonrepeatable reads  | Phantom reads | Lost updates | 
| :--------------- | :---------: | :------------------: | :-----------: | :----------: |                                                                                                                
| Serializable     | no          | no                   | no            | no *         | 
| Repeatable reads | no          | no                   | no            | no *         | 
| Read committed   | no          | yes                  | yes           | yes          | 
| Read uncommitted | no          | yes                  | yes           | yes          |

\* - записывается первая закомиченная транзакция, во второй транзакции возникает ошибка при update

## Anomaly examples

### Dirty reads

| Transaction 1                              | Transaction 2                                   |
| ------------------------------------------ | ----------------------------------------------- |
| START TRANSACTION;                         |                                                 |
| SELECT balance FROM balances WHERE id = 1; |                                                 |
|                                            | START TRANSACTION;                              |
|                                            | UPDATE balances SET balance = 600 WHERE id = 1; |
| SELECT balance FROM balances WHERE id = 1; |                                                 |
| COMMIT;                                    |                                                 |
|                                            | ROLLBACK;                                       |

### Nonrepeatable reads (read skew)

| Transaction 1                              | Transaction 2                                   |
| ------------------------------------------ | ----------------------------------------------- |
| START TRANSACTION;                         |                                                 |
| SELECT balance FROM balances WHERE id = 1; |                                                 |
|                                            | START TRANSACTION;                              |
|                                            | UPDATE balances SET balance = 600 WHERE id = 1; |
|                                            | COMMIT;                                         |
| SELECT balance FROM balances WHERE id = 1; |                                                 |
| COMMIT;                                    |                                                 |

### Phantom reads (write skew?)

| Transaction 1                      | Transaction 2                                |
| ---------------------------------- | -------------------------------------------- |
| START TRANSACTION;                 |                                              |
| SELECT SUM(balance) FROM balances; |                                              |
|                                    | START TRANSACTION;                           |
|                                    | INSERT INTO balances (balance) VALUES (500); |
|                                    | COMMIT;                                      |
| SELECT SUM(balance) FROM balances; |                                              |
| COMMIT;                            |                                              |

### Lost updates

| Transaction 1                                   | Transaction 2                                   |
| ----------------------------------------------- | ----------------------------------------------- |
| START TRANSACTION;                              |                                                 |
| SELECT balance FROM balances WHERE id = 1;      |                                                 |
|                                                 | START TRANSACTION;                              |
|                                                 | SELECT balance FROM balances WHERE id = 1;      |
|                                                 | UPDATE balances SET balance = 600 WHERE id = 1; |
|                                                 | COMMIT;                                         |
| UPDATE balances SET balance = 600 WHERE id = 1; |                                                 |
| COMMIT;                                         |                                                 |

## Database schema

### MySQL

``` sql
CREATE TABLE `balances` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `balance` INT(11) NOT NULL,
    PRIMARY KEY (`id`)
);
```

### PostgreSQL

```sql
CREATE TABLE balances (
    id SERIAL PRIMARY KEY,
    balance INTEGER NOT NULL
);
```

## Prepare data

```sql
INSERT INTO balances (balance) VALUES (500);
```

## Useful SQL queries

Set isolation level for current session (MySQL):
```sql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

Set isolation level for transaction (PostgreSQL):
```sql
START TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```
