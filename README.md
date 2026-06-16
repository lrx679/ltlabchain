## LTLab Chain

LTLab Chain is an EVM-compatible execution client for the LTLab Chain network.
The codebase is derived from an upstream EVM execution client and is maintained
as the foundation for LTLab Chain node operation, tooling, and protocol
development.

This repository is intended for operators, developers, and integrators who need
to build or run an LTLab Chain node from source.

## Building the source

Building the node requires Go 1.23 or later and a C compiler. After installing
the required toolchain, build the main client with:

```shell
make geth
```

To build the full suite of utilities:

```shell
make all
```

Build outputs are written to `build/bin/`.

## Executables

The project includes several command-line tools under `cmd/`.

| Command | Description |
| :--- | :--- |
| `geth` | Main LTLab Chain execution client. It can run a node, expose JSON-RPC APIs, manage chain data, and connect to supported LTLab Chain networks. |
| `clef` | Stand-alone signing tool that can be used as a backend signer for the node. |
| `devp2p` | Networking utilities for working with nodes and peer discovery. |
| `abigen` | Contract binding generator for Solidity ABI definitions. |
| `evm` | Developer utility for executing and debugging EVM bytecode in a controlled environment. |
| `rlpdump` | Utility for decoding RLP data into a readable tree structure. |

## Running a node

After building, start the client with:

```shell
./build/bin/geth
```

Common runtime options include:

```shell
./build/bin/geth --config /path/to/config.toml
./build/bin/geth --http --http.addr 127.0.0.1 --http.port 8545
./build/bin/geth dumpconfig
```

The default binary name is still `geth` for compatibility with the inherited
tooling and command structure. Public documentation and release packaging can
rename or wrap this binary later if the LTLab Chain distribution needs a
different command name.

## JSON-RPC

The client supports JSON-RPC over HTTP, WebSocket, and IPC. HTTP and WebSocket
interfaces are disabled by default and should be exposed carefully.

Useful options:

* `--http` enables the HTTP-RPC server.
* `--http.addr` sets the HTTP-RPC listen interface.
* `--http.port` sets the HTTP-RPC listen port.
* `--http.api` selects APIs exposed over HTTP.
* `--ws` enables the WebSocket RPC server.
* `--ws.addr` sets the WebSocket listen interface.
* `--ws.port` sets the WebSocket listen port.
* `--ws.api` selects APIs exposed over WebSocket.
* `--ipcdisable` disables the IPC-RPC server.
* `--ipcpath` sets the IPC socket or pipe path.

Do not expose RPC endpoints to untrusted networks without a clear security
model. Browser-accessible local RPC endpoints and public HTTP/WS endpoints can
be abused by malicious sites or remote attackers.

## Configuration

Runtime configuration can be provided with a TOML file:

```shell
./build/bin/geth --config /path/to/config.toml
```

Generate a sample configuration from the current defaults:

```shell
./build/bin/geth dumpconfig
```

Network-specific genesis files, bootnodes, chain IDs, and consensus settings
should be maintained as LTLab Chain release artifacts and documented alongside
each public network.

## Docker

Build a local Docker image from this repository:

```shell
docker build -t ltlab-chain-node .
```

Run the image with a persistent data directory:

```shell
docker run -d --name ltlab-chain-node \
  -v /path/to/ltlab-chain-data:/root \
  -p 8545:8545 -p 30303:30303 \
  ltlab-chain-node
```

Set RPC listen addresses and APIs explicitly for your deployment environment.

## Development

Run tests with:

```shell
go test ./...
```

Format Go changes with:

```shell
gofmt -w <files>
```

Contributions should keep changes focused, preserve EVM compatibility where it
is part of the intended behavior, and document any LTLab Chain specific protocol
or network changes.

## Security

Report security issues privately to the LTLab Chain maintainers. Do not open a
public issue for vulnerabilities until a fix or disclosure plan is ready.

## License

The library code outside `cmd` is licensed under the GNU Lesser General Public
License v3.0, included in `COPYING.LESSER`.

The binaries and command-line tools under `cmd` are licensed under the GNU
General Public License v3.0, included in `COPYING`.

Existing upstream copyright notices, license files, and author records are
retained where required.
