# Messages

Tangle messages are sent over TCP/IP. Any node may receive a message from any other node.

## Schema

Tangle uses MessagePack to encode messages.

```
{
  version: uint8 # Protocol Version
  type: string # Message Type
  recipients: node_id[] # Recipient nodes (can be empty; see below)
  sender: node_id # Sender node
  seq: uint32 # Sequence number
  ts: uint64 # UTC Unix Timestamp
  payload: uint8[] # Payload with variable length
}
```

### Protocol Version

The `version` field stores the version of the Tangle protocol the client has sent. The first version is `0x0`.

### Message Type

Indicates the type of message being sent. This will determine how the message should be interpreted and processed.

### Sender

Specifies the node via it's Node ID the message originates from.

### Recipients

Specifies the intended recipient nodes via their Node ID.

The way messages are published to nodes depends on the amount of recipients:

| Amount    | Description                                        |
| --------- | -------------------------------------------------- |
| 1         | Publishes a message to one specified node          |
| >1        | Publishes a message to one or more specified nodes |
| 0         | Publishes a message to all nodes                   |

### Sequence Number

This is a monotonically increasing number.

Each node in a Tangle network keeps track of a *sequence number*. It is unique per node.

This is used for ordering, de-duplicating and handling retransmissions of messages.

### Payload

The payload contains data related to the message itself.

It is an array of bytes. It has a variable length.
