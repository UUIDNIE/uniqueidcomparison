The purpose of this table is to help compare the number of possible bits that could be used as entropy bits in unique ID formats.  

Some IDs require coordination to reduce collision potential. Techniques such as prefixing a timestamp, namespace, node id, monotonic ticks or similar are not evaluated intentionally. 

The collision risk column is the square root of the total number of possible permutations of each ID based on total possible entropy bits.  We take the
square root of total possible permutations to estimate the probability of a 50% collision based only on the total number of potential entropy bits.

The intent of this table is to help estimate collision potential only using the bits that could be used for entropy in each ID format. 

This is a work in progress and I'm still checking for errors.

| ID Name       | Bits in Native Format | Max Entropy Bits | Reasoning | Number of IDs required for ~50% Collision Risk |
| ------------- | --------------------- | ---------------- | --------- | ------------------------ |
| Snowflake     | 64                    | 22               | We can use the snowflake node id and sequence number for a total of 22 entropy bits. | 2,097 |
| ShardID       | 64                    | 23               | ShardID consists of a 41-bit timestamp, 13-bit logical shard ID, and a 10-bit auto-incrementing sequence. The shard ID and sequence provide the entropy. | 4,190 |
| ObjectID      | 96                    | 63               | Similar to XID, MongoDB's ObjectID has the same components and hence similar entropy. | 2,320,229,388 |
| XID           | 96                    | 63               | XID consists of a 4-byte timestamp, 3-byte machine ID, 2-byte process ID, and 3-byte incrementing counter. The counter can be considered entropy. | 2,320,229,388 |
| pushID        | 112                   | 72               | Firebase pushID includes a timestamp, a client ID, and a per-process counter. The client ID and the counter part have entropy. | 42,024,647,484 |
| Elasticflake  | 128                   | 64               | Elasticflake,we can use machine ID + sequence to inject entropy. | 2,814,749,767 |
| FlakeID       | 128                   | 64               | FlakeID includes a timestamp, a sequence, and a machine ID, we can use machine ID + sequence to inject entropy. | 2,814,749,767 |
| Sonyflake     | 128                   | 64               | Sonyflake is similar to Snowflake but with 128-bit IDs. It includes a timestamp, a sequence, and a machine ID, and the sequence is the entropy part. | 2,814,749,767 |
| UUIDv7        | 128                   | 74               | UUIDv7 includes a 48-bit timestamp, 4-bit version, 2-bit variant, and two random fields (rand_a and rand_b) with 12 and 62 bits respectively, hence the entropy is the sum of the two random fields. | 13,553,082,591 |
| Flake         | 128                   | 80               | Flake has a timestamp and a random component similar to ULID. | 1,000,000,000,000 |
| KSUID         | 128                   | 80               | KSUID also includes a timestamp and a random value, so the random value contributes to entropy. | 1,000,000,000,000 |
| SID           | 128                   | 80               | SID is similar to ULID with a timestamp and a random component, so the entropy comes from the random part. | 1,000,000,000,000 |
| ULID          | 128                   | 80               | ULID includes a timestamp and a random component, hence the random part contributes to entropy. | 1,000,000,000,000 |
| CUID          | 128                   | 120              | CUID includes a timestamp, a counter, a client identifier, and two random blocks. The counter and the random blocks contribute to entropy. | 1,000,000,000,000,000,000 |
| COMBGUID      | 128                   | 122              | COMBGUID is a variant of UUID, thus having the same entropy as standard UUIDv4. | 1,318,433,449,495,883,392 |
| LexicalUUID   | 128                   | 122              | LexicalUUID is similar to standard UUID, with a few bits set aside for version and variant, hence 122 bits of entropy. | 1,318,433,449,495,883,392 |
| orderedUuid   | 128                   | 122              | Similar to LexicalUUID, orderedUuid also provides a UUID variant with entropy in all but the version and variant bits. | 1,318,433,449,495,883,392 |
| UUIDv4        | 128                   | 122              | UUIDv4 includes a few bits set aside for version (4) and variant information, leaving 122 bits for random or pseudo-random values, hence the entropy. | 1,318,433,449,495,883,392 |
| XUID256       | 256                   | 196              | XUID256 concatenates UUIDv4 and UUIDv7, hence the entropy is the sum of the entropy bits of UUIDv4 and UUIDv7. | 22,627,417,000,000,000,000,000,000,000,000 |



```
._______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________.
|                                                                                                                                                                                                                                                                                                                                                                                               |
| Snowflake                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|                                                    
| 000 | 001 | 002 | 003 | 004 | 005 | 006 | 007 | 008 | 009 | 010 | 011 | 012 | 013 | 014 | 015 | 016 | 017 | 018 | 019 | 020 | 021 | 022 | 023 | 024 | 025 | 026 | 027 | 028 | 029 | 030 | 031 | 032 | 033 | 034 | 035 | 036 | 037 | 038 | 039 | 040 | 041 | 042 | 043 | 044 | 045 | 046 | 047 | 048 | 049 | 050 | 051 | 052 | 053 | 054 | 055 | 056 | 057 | 058 | 059 | 060 | 061 | 062 | 063 |
|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| VER | TIMESTAMP (41 BITS)																												    | MACHINE ID (10 BITS)				        | SEQUENCE NUMBER (12 BITS)                                             |
'-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
```
