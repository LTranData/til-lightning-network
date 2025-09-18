# TCP ports
The default TCP port depends on the network used. The most common networks are:

- Bitcoin mainnet with port number 9735 or the corresponding hexadecimal 0x2607;
- Bitcoin testnet with port number 19735 (0x4D17);
- Bitcoin signet with port number 39735 (0x9B37).

# Connection handling

Implementations MUST use a single connection per peer; channel messages (which include a channel ID) are multiplexed over this single connection.

# Lightning message format

After decryption, all Lightning messages are of the form:

- type: a 2-byte big-endian field indicating the type of message
- payload: a variable-length payload that comprises the remainder of the message and that conforms to a format matching the type
- extension: an optional TLV (Type-Length-Value) stream

The messages are grouped logically into five groups, ordered by the most significant bit that is set:

- Setup & Control (types 0-31): messages related to connection setup, control, supported features, and error reporting (described below)
- Channel (types 32-127): messages used to setup and tear down micropayment channels (described in BOLT #2)
- Commitment (types 128-255): messages related to updating the current commitment transaction, which includes adding, revoking, and settling HTLCs as well as updating fees and exchanging signatures (described in BOLT #2)
- Routing (types 256-511): messages containing node and channel announcements, as well as any active route exploration (described in BOLT #7)
- Custom (types 32768-65535): experimental and application-specific messages

The size of the message is required by the transport layer to fit into a 2-byte unsigned int; therefore, the maximum possible size is 65535 bytes.

# Type-Length-Value Format

Throughout the protocol, a TLV (Type-Length-Value) format is used to allow for the backwards-compatible addition of new fields to existing message types.

A tlv_record represents a single field, encoded in the form:

[bigsize: type]
[bigsize: length]
[length: value]

The `type` is encoded using the BigSize format. It functions as a message-specific, 64-bit identifier for the tlv_record determining how the contents of value should be decoded. type identifiers below 2^16 are reserved for use in this specification. type identifiers greater than or equal to 2^16 are available for custom records. Any record not defined in this specification is considered a custom record. This includes experimental and application-specific messages.

The `length` is encoded using the BigSize format signaling the size of value in bytes.

The `value` depends entirely on the type, and should be encoded or decoded according to the message-specific format determined by type.

# Fundamental types

byte: an 8-bit byte
s8: an 8-bit signed integer
u16: a 2 byte unsigned integer
s16: a 2 byte signed integer
u32: a 4 byte unsigned integer
s32: a 4 byte signed integer
u64: an 8 byte unsigned integer
s64: an 8 byte signed integer

For the final value in TLV records, truncated integers may be used. Leading zeros in truncated integers MUST be omitted:

tu16: a 0 to 2 byte truncated unsigned integer
tu32: a 0 to 4 byte truncated unsigned integer
tu64: a 0 to 8 byte truncated unsigned integer

When used to encode amounts, the previous fields MUST comply with the upper bound of 21 million BTC:

satoshi amounts MUST be at most 0x000775f05a074000
milli-satoshi amounts MUST be at most 0x1d24b2dfac520000

The following convenience types are also defined:

- chain_hash: a 32-byte chain identifier (see BOLT #0)
- channel_id: a 32-byte channel_id (see BOLT #2)
- sha256: a 32-byte SHA2-256 hash
- signature: a 64-byte bitcoin Elliptic Curve signature
- bip340sig: a 64-byte bitcoin Elliptic Curve Schnorr signature as per BIP-340
- point: a 33-byte Elliptic Curve point (compressed encoding as per SEC 1 standard)
- short_channel_id: an 8 byte value identifying a channel (see BOLT #7)
- sciddir_or_pubkey: either 9 or 33 bytes referencing or identifying a node, respectively
    - if the first byte is 0 or 1, then an 8-byte short_channel_id follows for a total of 9 bytes
        - 0 for the first byte indicates this refers to node_id_1 in the channel_announcement for short_channel_id
        - 1 for the first byte indicates this refers to node_id_2 in the channel_announcement for short_channel_id (see BOLT #7
    - if the first byte is 2 or 3, then the value is a 33-byte point
- bigsize: a variable-length, unsigned integer similar to Bitcoin's CompactSize encoding, but big-endian. Described in BigSize.
- utf8: a byte as part of a UTF-8 string. A writer MUST ensure an array of these is a valid UTF-8 string, a reader MAY reject any messages containing an array of these which is not a valid UTF-8 string.