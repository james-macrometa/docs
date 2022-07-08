---
title: grpc-call (Sink)
---

This extension publishes event data encoded into GRPC Classes as defined in the user input jar. This extension has a default gRPC service classes jar added. The default service is called "EventService". If we want to use our custom gRPC services, we have to pack auto-generated gRPC service classes and protobuf classes into a jar file and add it into the project classpath (or to the `jars` folder in the `stream processor-tooling` folder if we use it with `stream processor-tooling`). This grpc-call sink is used for scenarios where we send a request out and expect a response back. In default mode this will use EventService process method. grpc-call-response source is used to receive the responses. A unique sink.id is used to correlate between the sink and its corresponding source.

Syntax

    CREATE SINK <NAME> WITH (type="grpc-call", map.type="<STRING>" publisher.url="<STRING>", sink.id="<INT>", headers="<STRING>", idle.timeout="<LONG>", keep.alive.time="<LONG>", keep.alive.timeout="<LONG>", keep.alive.without.calls="<BOOL>", enable.retry="<BOOL>", max.retry.attempts="<INT>", retry.buffer.size="<LONG>", per.rpc.buffer.size="<LONG>", channel.termination.waiting.time="<LONG>", max.inbound.message.size="<LONG>", max.inbound.metadata.size="<LONG>", truststore.file="<STRING>", truststore.password="<STRING>", truststore.algorithm="<STRING>", tls.store.type="<STRING>", keystore.file="<STRING>", keystore.password="<STRING>", keystore.algorithm="<STRING>", enable.ssl="<BOOL>", mutual.auth.enabled="<BOOL>")

## Query Parameters

| Name                             | Description                                                                                                                                                                                                                                                                                                           | Default Value   | Possible Data Types | Optional | Dynamic |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------------|----------|---------|
| publisher.url                    | The url to which the outgoing events should be published via this extension. This url should consist the host hostPort, port, fully qualified service name, method name in the following format. `grpc://0.0.0.0:9763/<serviceName>/<methodName>` For example: grpc://0.0.0.0:9763/org.gdn.grpc.EventService/consume |                 | STRING              | No       | No      |
| sink.id                          | a unique ID that should be set for each grpc-call-sink. There is a 1:1 mapping between grpc-call sinks and grpc-call-response sources. Each sink has one particular source listening to the responses to requests published from that sink. So the same sink.id should be given when writing the source also.         |                 | INT                 | No       | No      |
| headers                          | GRPC Request headers in format `"'<key>:<value>','<key>:<value>'"`. If header parameter is not provided just the payload is sent                                                                                                                                                                                      | \-              | STRING              | Yes      | No      |
| idle.timeout                     | Set the duration in seconds without ongoing RPCs before going to idle mode.                                                                                                                                                                                                                                           | 1800            | LONG                | Yes      | No      |
| keep.alive.time                  | Sets the time in seconds without read activity before sending a keepalive ping. Keepalives can increase the load on services so must be used with caution. By default set to Long.MAX\_VALUE which disables keep alive pinging.                                                                                       | Long.MAX\_VALUE | LONG                | Yes      | No      |
| keep.alive.timeout               | Sets the time in seconds waiting for read activity after sending a keepalive ping.                                                                                                                                                                                                                                    | 20              | LONG                | Yes      | No      |
| keep.alive.without.calls         | Sets whether keepalive will be performed when there are no outstanding RPC on a connection.                                                                                                                                                                                                                           | false           | BOOL                | Yes      | No      |
| enable.retry                     | Enables the retry and hedging mechanism provided by the gRPC library.                                                                                                                                                                                                                                                 | false           | BOOL                | Yes      | No      |
| max.retry.attempts               | Sets max number of retry attempts. The total number of retry attempts for each RPC will not exceed this number even if service config may allow a higher number.                                                                                                                                                      | 5               | INT                 | Yes      | No      |
| retry.buffer.size                | Sets the retry buffer size in bytes. If the buffer limit is exceeded, no RPC could retry at the moment, and in hedging case all hedges but one of the same RPC will cancel.                                                                                                                                           | 16777216        | LONG                | Yes      | No      |
| per.rpc.buffer.size              | Sets the per RPC buffer limit in bytes used for retry. The RPC is not retriable if its buffer limit is exceeded.                                                                                                                                                                                                      | 1048576         | LONG                | Yes      | No      |
| channel.termination.waiting.time | The time in seconds to wait for the channel to become terminated, giving up if the timeout is reached.                                                                                                                                                                                                                | 5               | LONG                | Yes      | No      |
| max.inbound.message.size         | Sets the maximum message size allowed to be received on the channel in bytes                                                                                                                                                                                                                                          | 4194304         | LONG                | Yes      | No      |
| max.inbound.metadata.size        | Sets the maximum size of metadata allowed to be received in bytes                                                                                                                                                                                                                                                     | 8192            | LONG                | Yes      | No      |
| truststore.file                  | the file path of truststore. If this is provided then server authentication is enabled                                                                                                                                                                                                                                | \-              | STRING              | Yes      | No      |
| truststore.password              | the password of truststore. If this is provided then the integrity of the keystore is checked                                                                                                                                                                                                                         | \-              | STRING              | Yes      | No      |
| truststore.algorithm             | the encryption algorithm to be used for server authentication                                                                                                                                                                                                                                                         | \-              | STRING              | Yes      | No      |
| tls.store.type                   | TLS store type                                                                                                                                                                                                                                                                                                        | \-              | STRING              | Yes      | No      |
| keystore.file                    | the file path of keystore. If this is provided then client authentication is enabled                                                                                                                                                                                                                                  | \-              | STRING              | Yes      | No      |
| keystore.password                | the password of keystore                                                                                                                                                                                                                                                                                              | \-              | STRING              | Yes      | No      |
| keystore.algorithm               | the encryption algorithm to be used for client authentication                                                                                                                                                                                                                                                         | \-              | STRING              | Yes      | No      |
| enable.ssl                       | to enable ssl. If set to true and truststore.file is not given then it will be set to default carbon jks by default                                                                                                                                                                                                   | FALSE           | BOOL                | Yes      | No      |
| mutual.auth.enabled              | to enable mutual authentication. If set to true and truststore.file or keystore.file is not given then it will be set to default carbon jks by default                                                                                                                                                                | FALSE           | BOOL                | Yes      | No      |

## Example 1

    CREATE SINK FooStream WITH (type='grpc-call', map.type='json', publisher.url = 'grpc://194.23.98.100:8080/EventService/process', sink.id= '1') (message String);

    CREATE SOURCE BarStream WITH (type='grpc-call-response', sink.id= '1') (message String);

Here a stream named FooStream is defined with grpc sink. A grpc server should be running at 194.23.98.100 listening to port 8080. sink.id is set to 1 here. So we can write a source with sink.id 1 so that it will listen to responses for requests published from this stream. Note that since we are using EventService/process the sink will be operating in default mode

## Example 2

    CREATE SINK FooStream WITH (type='grpc-call`, map.type='json', publisher.url = 'grpc://194.23.98.100:8080/EventService/process', sink.id= '1',) (message String);

    CREATE SOURCE BarStream WITH (type='grpc-call-response', sink.id= '1') (message String);

Here with the same FooStream definition we have added a BarStream which has a grpc-call-response source with the same sink.id 1. So the responses for calls sent from the FooStream will be added to BarStream.

## Example 3

    CREATE SINK FooStream WITH (type='grpc-call`, map.type='protobuf', publisher.url = 'grpc://194.23.98.100:8888/org.gdn.grpc.test.MyService/process', sink.id= '1') (stringValue string, intValue int,longValue long,booleanValue bool,floatValue float,doubleValue double);

    CREATE SOURCE FooStream WITH (type='grpc-call-response', map.type='protobuf', receiver.url = 'grpc://localhost:8888/org.gdn.grpc.MyService/process', sink.id= '1',) (stringValue string, intValue int,longValue long,booleanValue bool,floatValue float,doubleValue double);

Here a stream named FooStream is defined with grpc sink. A grpc server should be running at 194.23.98.100 listening to port 8080. We have added another stream called BarStream which is a grpc-call-response source with the same sink.id 1 and as same as FooStream definition. So the responses for calls sent from the FooStream will be added to BarStream. Since there is no mapping available in the stream definition attributes names should be as same as the attributes of the protobuf message definition. (Here the only reason we provide receiver.url in the grpc-call-response source is for protobuf mapper to map Response into a stream processor event, we can give any address and any port number in the URL, but we should provide the service name and the method name correctly)

## Example 4

    CREATE SINK FooStream WITH (type='grpc-call', map.type='protobuf', publisher.url = 'grpc://194.23.98.100:8888/org.gdn.grpc.test.MyService/process', sink.id= '1', map.payload="stringValue='a',longValue='c',intValue='b',booleanValue='d',floatValue = 'e', doubleValue = 'f'") (a string, b int,c long,d bool,e float,f double);

    CREATE SOURCE FooStream WITH (type='grpc-call-response', map.type='protobuf', receiver.url = 'grpc://localhost:8888/org.gdn.grpc.test.MyService/process', sink.id= '1', map.attributes="a = 'stringValue', b = 'intValue', c = 'longValue',d = 'booleanValue', e ='floatValue', f ='doubleValue'") (a string, b int,c long,d bool,e float,f double);

Here with the same FooStream definition we have added a BarStream which has a grpc-call-response source with the same sink.id 1. So the responses for calls sent from the FooStream will be added to BarStream. In this stream we provided mapping for both the sink and the source. so we can use any name for the attributes in the stream definition, but we have to map those attributes with correct protobuf attributes. As same as the grpc-sink, if we are planning to use metadata we should map the attributes.