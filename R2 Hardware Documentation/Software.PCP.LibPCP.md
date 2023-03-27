   libpcp | RingEdge 2    window.dataLayer = window.dataLayer || \[\]; function gtag() { dataLayer.push(arguments); } gtag('js', new Date()); gtag('config', 'G-LG6C6HT317');

[Contents](/eamuse/sega/)

[Intro](/eamuse/sega/intro/)

[Software](/eamuse/sega/software/)

[Hardware](/eamuse/sega/hardware/)

[Manual](/eamuse/sega/manual/)

libpcp
======

A download for a precompiled libpcp can be found here: [libpcp.lib (7b31fa7a5145b6a945655a970022df8d)](/eamuse/mice/libpcp.lib). The required headers can be found in the micetools source code.

Main server section
-------------------

### Server setup

#### `e_pcpa_t pcpaInitStream(pcpa_t *stream)`

Initialise a pcp stream, allocating memory as needed.

#### `e_pcpa_t pcpaSetCallbackFuncBuffer(pcpa_t *stream, pcpa_cb_table_t *callback_table, uint callbacks_max)`

Assign a callback table to the stream in preperation for callback registration.

#### `e_pcpa_t pcpaSetCallbackFunc(pcpa_t *stream, char *keyword, pcpa_callback *callback, void *data)`

Register a callback. If a command is received where the first keyword matches `keyword`, `callback` is called to process it. The stream is passed as the first argument to the callback, and `data` as the second.

#### `e_pcpa_t pcpaOpenServerWithBinary(pcpa_t *stream, int open_mode, u_short port, u_short binary_port, undefined4 param_5)`

Open the pcp server on `port`, and prepare to use `binary_port` for data transfer.

Binds to `0.0.0.0` if `open_mode` is `0`, `127.0.0.1` if it is `1`.

`param_5` is currently unknown. Pass `300000` for now; I think it's some sort of timeout.

#### void pcpaClose(pcpa\_t \*stream)

Close a pcp stream

### Server mainloop

#### `e_pcpa_t pcpaServer(pcpa_t *stream, timeout_t timeout_ms)`

Perform one tick of the server. If no events occure within `timeout_ms`, this returns.

Within callbacks
----------------

### Reading commands

#### `char* pcpaGetCommand(pcpa_t* pcpa, char* command)`

Fetch the value for a given key. Returns `NULL` if the key is not found.

#### `char* pcpaGetKeyword(pcpa_t* stream, uint keyword_num)`

Request the keyword at a given index. Useful for fetching the 0-index keyword when the same callback is registered to multiple keywords.

### Sending responses

#### `pcp_send_data_t *pcpaSetSendPacket(pcpa_t *stream, char *keyword, char *value)`

Begin a response, setting the initial key-value pair.

#### `pcp_send_data_t *pcpaAddSendPacket(pcpa_t *stream, char *keyword, char *value)`

Add an additional key-value pair onto the packet being generated.

#### `e_pcpa_t pcpaSetBinaryMode(pcpa_t *stream, binary_mode_t binary_mode)`

Set the binary mode to either disabled, receiving, or transmitting data.

#### `e_pcpa_t pcpaSetBeforeBinaryModeCallBackFunc(pcpa_t *stream, pcpa_callback *callback, void *data)`

Register a callback to be called before a data transfer commences.

#### `e_pcpa_t pcpaSetSendBinaryBuffer(pcpa_t *stream, byte *send_buffer, size_t len)`

Set the buffer to transmit from when performing a server->consumer data transfer. Should usually be called in the before callback.

#### `e_pcpa_t pcpaSetRecvBinaryBuffer(pcpa_t *stream, byte *recv_buffer, size_t len)`

Set the buffer to receive into when performing a consumer->server data transfer. Should usually be called in the before callback.

#### `e_pcpa_t pcpaSetAfterBinaryModeCallBackFunc(pcpa_t *stream, pcpa_callback *callback, void *data)`

Register a callback to be called after a data transfer completes.

[sega](/eamuse/sega/)/[software](/eamuse/sega/software/)/[pcp](/eamuse/sega/software/pcp/)/libpcp.html
