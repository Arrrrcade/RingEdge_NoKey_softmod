PCP
===

What is PCP?
------------

PCP is the protocol used for inter-process communication between services running on Ring\* systems. I have no idea what it stands for; head-canon it as Process Communication Protocol or whatever you want really. The official implementation is `libpcp`, which is statically linked in to binaries that make use of the protocol (and is itself dependent on `amLib`).

On paper, there are many things the format would at first appear to support, but is unable to due to the reference implementation in `libpcp`. Specification of this nature will be marked. Custom implementations should be liberal in what they receive, and conservative in what they transmit; marked specification are key areas of focus for this.

Protocol Specification
----------------------

We consider two processes communicating: a server, and a consumer. A server need only be capable of processing at least one consumer concurrently, though implementations may desire the ability to process multiple consumers.

When a server is ready to process a command, it transmits a single `>` byte to its connected consumer.

The consumer responds with a CRLF-terminated payload packet, containing the command.

The server then responds syncronously with a CRLF-terminated payload packet. If a data transfer is being a performed, this packet will contain `port` and `size` as paramaters.

In a server->consumer data transfer operation, the consumer connects to the provided port, and expects to receive `size` bytes of data. It then closes the connection to the data port and transmits a `$` byte to the server to ackgnowledge receipt. The server will only process this ackgnowledgement after it has succesfully transmitted its data to a consumer.

The characteristics of a consumer->server transfer as as yet undocumented. I'll get round to it!

If the server is unable to process a request for any reason, it may respond with a single `?`. This may be due to a non existant command being requested, or it may be due to an invalid packet.

### Payload format

Outside of special packets such as `>` and `$`, all communication in both directions strictly follows the following structure:

<p align="left">
<img src="./pcp_railroad.png)">
</p>

_`Text`_ is defined as a series of one or more bytes matching `[a-zA-Z0-9._:@%/\{}-]`.

Spaces (ascii 20h) and tabs (ascii 09h) are allowable whitespace. They may be present anywhere in the packet surrounding keys, values, any delimiter, or seperator, and will be ignored. They are not valid within a key or a value.

Comments begin and end with the `#` symbol. They may appear at any point in a packet, and the packet should be processed as if the comment is not there. **The content of comments must match _`Text`_. This notably includes no whitespace.**

Format restrictions
-------------------

*   Payloads MUST NOT exceed 256 bytes, including the terminating CRLF.
*   There may be at most 64 key-value pairs in a payload.

Example communication
---------------------

Communication from the server is marked in blue, and communication from the client is marked in red.

    >nonsense
    ?
    >keychip.version=?&device=n2&cache=0
    keychip.version=0104
    >keyc#comment#hip.version=?
    keychip.version=0104
    >keychip.billing.cacertification=?
    keychip.billing.cacertification=0&port=40107&size=817
    $>

(Note an additional connection was made to `:40107` before transmitting the `$`.)

libpcp bugs

When parsing requests, libpcp null-terminates `?` values with an off-by-one error. This means if you transmit `test=12345` followed by `test=?`, it will be parsed as if you had transmitted `test=?2`. This could actually be an issue with how I am inspecting the internal state; I will update/remove this spoilier once I've had a chance to dig deeper.

libpcp does not enforce ampersand placement, causing strange memory artifacts. Consumers **MUST** conform to the ampersand specification.

libpcp allows empty keys and values. The case of an empty key causes the pair to be keyed with an empty string, however an empty value causes it to contain a random value, reading from memory where the previous packet was decoded. In fact, the presence of a `=` is not validated either, likewise causing it to read undefined regions of memory. Consumers **MUST** always provide both the key and the value.

Using libpcp

I have reproduced a locally functioning standalone distribution of libpcp, warts and all. Eventually I will produce some basic docs for making use of the exported functions, and will hopefully be able to provide a download for a precompiled library. I'm still unsure if the source code will ever be made available however because it's a _very_ true to the original reproduction, potentially problematically so!

If rather than implementing your own pcp you wish to use _the_ libpcp, watch this space.

[sega](/eamuse/sega/)/[software](/eamuse/sega/software/)/pcp[Next page](/eamuse/sega/software/drivers)
