# Transaction Isolation Levels

## MySQL (Percona)

|                  | Dirty reads | Non-repeatable reads | Phantom reads | Lost updates |
| :--------------- | :---------: | :------------------: | :-----------: | :----------: |
| Repeatable reads | ?           | ?                    | ?             | ?            |
| Read committed   | ?           | ?                    | ?             | ?            |
| Read uncommitted | ?           | ?                    | ?             | ?            |

## PostgreSQL

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

