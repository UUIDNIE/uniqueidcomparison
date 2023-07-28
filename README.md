The purpose of this table is to help compare the number of possible bits that could be used as entropy bits in IDs that are commonly used to produce unique values.  
Some IDs require coordination to prevent collisions. Techniques such as prefixing a timestamp, namespace, node id or similar are not evaluated intentionally. 

I made this quickly and I need to double check the math.  The collision risk column is the square root of the total number of permutations of each ID.  We take the
square root of that value to estimate the probability of a 50% collision based only on the total number of potential entropy bits. 

| ID Name       | Bits in Native Format | Max Entropy Bits | Reasoning | Collision Risk (Base 10) |
| ------------- | --------------------- | ---------------- | --------- | ------------------------ |
| Snowflake     | 64                    | 22               | Snowflake allocates 22 bits for randomness, with the rest being for timestamp and machine ID. | 2,097 |
| FlakeID       | 64                    | 41               | Similar to ShardingID, FlakeID is based on a timestamp, a node ID, and a sequence number. | 130,768 |
| ShardingID    | 64                    | 41               | ShardingID is essentially a timestamp plus a shard ID and an auto-incrementing sequence number. The auto-incrementing part can be considered random, hence the 41 bits. | 130,768 |
| ObjectID      | 96                    | 63               | Similar to XID, MongoDB's ObjectID has the same components and hence similar entropy. | 2,320,229,388 |
| XID           | 96                    | 63               | XID consists of a 4-byte timestamp, 3-byte machine ID, 2-byte process ID, and 3-byte incrementing counter. The counter can be considered entropy. | 2,320,229,388 |
| pushID        | 112                   | 72               | Firebase pushID includes a timestamp, a client ID, and a per-process counter. The client ID and the counter part have entropy. | 42,024,647,484 |
| Elasticflake  | 128                   | 64               | Elasticflake includes a timestamp, a sequence, and a machine ID, similar to Sonyflake, hence the entropy comes from the sequence. | 2,814,749,767 |
| Sonyflake     | 128                   | 64               | Sonyflake is similar to Snowflake but with 128-bit IDs. It includes a timestamp, a sequence, and a machine ID, and the sequence is the entropy part. | 2,814,749,767 |
| Flake         | 128                   | 80               | Flake has a timestamp and a random component similar to ULID. | 1,000,000,000,000 |
| KSUID         | 128                   | 80               | KSUID also includes a timestamp and a random value, so the random value contributes to entropy. | 1,000,000,000,000 |
| SID           | 128                   | 80               | SID is similar to ULID with a timestamp and a random component, so the entropy comes from the random part. | 1,000,000,000,000 |
| ULID          | 128                   | 80               | ULID includes a timestamp and a random component, hence the random part contributes to entropy. | 1,000,000,000,000 |
| CUID          | 128                   | 120              | CUID includes a timestamp, a counter, a client identifier, and two random blocks. The counter and the random blocks contribute to entropy. | 1,000,000,000,000,000,000 |
| COMBGUID      | 128                   | 122              | COMBGUID is a variant of UUID, thus having the same entropy as standard UUIDv4. | 1,318,433,449,495,883,392 |
| LexicalUUID   | 128                   | 122              | LexicalUUID is similar to standard UUID, with a few bits set aside for version and variant, hence 122 bits of entropy. | 1,318,433,449,495,883,392 |
| orderedUuid   | 128                   | 122              | Similar to LexicalUUID, orderedUuid also provides a UUID variant with entropy in all but the version and variant bits. | 1,318,433,449,495,883,392 |
| UUIDv4        | 128                   | 122              | UUIDv4 includes a few bits set aside for version (4) and variant information, leaving 122 bits for random or pseudo-random values, hence the entropy. | 1,318,433,449,495,883,392 |

