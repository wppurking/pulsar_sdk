# A pure ruby client for Apache Pulsar

Respecting [Pulsar binary protocol specification][2]

## Example

See `examples`.

## Dev

```shell
#!/bin/sh

PB_OUT="./lib/protobuf/"
PB_OUT_F='pulsar_api.pb.rb'

mkdir -p ${PB_OUT}

protoc -I ${PULSAR_GIT}/pulsar-common/src/main/proto/ --ruby_out ${PB_OUT} PulsarApi.proto
mv ${PB_OUT}PulsarApi_pb.rb ${PB_OUT}${PB_OUT_F}

# fix pulsar.proto.ProtocolVersion error: invalid name `v0' for constant
if [ $(uname) = 'Darwin' ]
then
    suffix='.pbbak'
fi

for i in $(seq 0 15)
do
    sed -i ${suffix} "s;value :v$i, $i;value :V$i, $i;g" ${PB_OUT}${PB_OUT_F}
done

rm -f ${PB_OUT}${PB_OUT_F}.pbbak

protoc -I ${PULSAR_GIT}/pulsar-common/src/main/proto/ --ruby_out ${PB_OUT} PulsarMarkers.proto
mv ${PB_OUT}PulsarMarkers_pb.rb ${PB_OUT}pulsar_markers.pb.rb
```

## Features

- [ ] Connection
  - [x] Establishment
  - [ ] TLS connection
  - [ ] Authentication
- [x] Producer
  - [x] Message Delivery
  - [ ] Batch Message Delivery
  - [x] [Delayed Message Delivery][1]
  - [x] Get SendReceipt
  - [x] Close Producer
  - [ ] [Deliver message after AR transaction commit][3]
- [ ] Consumer
  - [x] Flow control
  - [x] Ack
  - [x] Message Redelivery
  - [x] Listen
  - [ ] [Dead Letter Topic][4]
  - [ ] Partitioned
- [ ] Keep alive
  - [x] handle ping command
  - [ ] send ping command
- [ ] Service discovery
  - [x] Topic lookup
  - [x] Partitioned topics discovery
- [x] Log Optimization
- [x] Connection pool
- [ ] Unit Test
- [x] Thread safe
- [ ] Schema
  - [ ] Get
  - [ ] Create

## WIP

- Dead Letter Topic

[1]: https://github.com/apache/pulsar/wiki/PIP-26%3A-Delayed-Message-Delivery "PIP 26: Delayed Message Delivery"
[2]: https://pulsar.apache.org/docs/en/develop-binary-protocol/ "Pulsar binary protocol specification"
[3]: https://github.com/Envek/after_commit_everywhere "after commit everywhere"
[4]: https://github.com/apache/pulsar/wiki/PIP-22:-Pulsar-Dead-Letter-Topic "PIP 22: Pulsar Dead Letter Topic"
