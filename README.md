# Micro-Redis-Server
This project was completed as a part of the ICCS492: The Guts of Modern System course.

## Core Design Requirements
- **Single-machine service.** Make the assumption that the underlying hardware won’t crash and the underlying software stack (e.g., OS, Filesystem, etc.) isn’t going wrong, so the only source of issues (for now) will be our own code.
- **Multithreaded asynchronous.** The service will be asynchronous by design and take advantage of multithreading.
- **Linearizable.** The service guarantees the interaction (request/response) is linearizable under the failure mode assumption above. If it crashes, it will stop and give up (but it shouldn’t crash).
- **Multi Databases.** The service supports keeping multiple *independent* databases (namespaces). Like real Redis, the `SELECT` command can be used to select which namespace to work on; there are only 16 databases available, numbered 0, 1, …, 15. The default namespace is 0.
*Wire Protocol*. It follows the wire protocol specification, which is described at https://redis.io/topics/protocol. This project uses protocol version 2.

## API Specifications
Referring to the official Redis command specification (https://redis.io/docs/latest/commands/) for the description of each command and its expected behaviors/performance guarantees. Properly handle errors that may arise. This project will only support the following commands:
- **SELECT** - As described in the real spec. However, our service will only allow `SELECT` to be called at the start of the session (when a client first connects).
- **GET** - As described in the real spec.
- **SET** - As described in the real spec. However, we won’t support any of the options, just the barebones `SET key value` command.
- **PING** - As described in the real spec. This command will be excluded from the linearizability requirement, i.e., won’t be counted in the history.
- **EXISTS** - As described in the real spec.
- **RPUSH, LPUSH** - As described in the real spec.
- **BLPOP, BRPOP** (with timeout) - As described in the real spec. The number of keys that can be “checked” in one call will be limited to 5. Our timeout is interpreted as a floating-number (double).
