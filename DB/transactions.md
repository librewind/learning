# Transaction Isolation Levels

## MySQL (Percona 8.0.29-21)

|                  | Dirty reads | Non-repeatable reads | Phantom reads | Lost updates |
| :--------------- | :---------: | :------------------: | :-----------: | :----------: |
| Repeatable reads | ?           | ?                    | ?             | ?            |
| Read committed   | ?           | ?                    | ?             | ?            |
| Read uncommitted | ?           | ?                    | ?             | ?            |

## PostgreSQL 14.6-1.pgdg110+1

|                  | Dirty reads | Non-repeatable reads | Phantom reads | Lost updates | 
| :--------------- | :---------: | :------------------: | :-----------: | :----------: |                                                                                                                
| Serializable     | ?           | ?                    | ?             | ?            | 
| Repeatable reads | ?           | ?                    | ?             | ?            | 
| Read committed   | ?           | ?                    | ?             | ?            | 
| Read uncommitted | ?           | ?                    | ?             | ?            |

## Problem examples

### Dirty reads

| Transaction 1      | Transaction 2      |
| ------------------ | ------------------ |
| START TRANSACTION; | START TRANSACTION; |
| COMMIT;            | COMMIT;            |

### Non-repeatable reads

### Phantom reads

### Lost updates

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
