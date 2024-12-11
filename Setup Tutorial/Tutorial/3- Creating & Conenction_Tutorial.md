Here's the comprehensive list of commands to create policies, things, and connections with Eclipse Ditto, formatted for clarity:

---

### 1. **Create Policies**

This command sets up a policy for access to Things, allowing read and write options.

**Command:**
```bash
curl -X PUT 'http://<your-ip>:8080/api/2/policies/my.test:policy' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     --data '{
         "policyId": "my.test:policy",
         "entries": {
             "owner": {
                 "subjects": {
                     "nginx:ditto": {
                         "type": "nginx basic auth user"
                     }
                 },
                 "resources": {
                     "thing:/": {
                         "grant": ["READ", "WRITE"],
                         "revoke": []
                     },
                     "policy:/": {
                         "grant": ["READ", "WRITE"],
                         "revoke": []
                     },
                     "message:/": {
                         "grant": ["READ", "WRITE"],
                         "revoke": []
                     }
                 }
             },
             "observer": {
                 "subjects": {
                     "ditto:observer": {
                         "type": "observer user"
                     }
                 },
                 "resources": {
                     "thing:/features": {
                         "grant": ["READ"],
                         "revoke": []
                     },
                     "policy:/": {
                         "grant": ["READ"],
                         "revoke": []
                     },
                     "message:/": {
                         "grant": ["READ"],
                         "revoke": []
                     }
                 }
             }
         }
     }'
```

---

### 2. **Create Things**

**Feedback Sensors:**
```bash
curl -X PUT 'http://<your-ip>:8080/api/2/things/sensor1.iff:sensor01' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "accelerometer": {
                 "properties": {
                     "x": 5896,
                     "y": 456,
                     "z": 456
                 }
             },
             "gyroscope": {
                 "properties": {
                     "x": 45,
                     "y": 5,
                     "z": 5
                 }
             }
         }
     }'

curl -X PUT 'http://<your-ip>:8080/api/2/things/sensor2.iff:sensor02' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "accelerometer": {
                 "properties": {
                     "x": 5896,
                     "y": 456,
                     "z": 456
                 }
             },
             "gyroscope": {
                 "properties": {
                     "x": 45,
                     "y": 5,
                     "z": 5
                 }
             }
         }
     }'
```

**Alert Sensors:**
```bash
curl -X PUT 'http://<your-ip>:8080/api/2/things/mys1.alert:sensor' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "alert": {
                 "properties": {
                     "m": 0
                 }
             }
         }
     }'

curl -X PUT 'http://<your-ip>:8080/api/2/things/mys2.alert:sensor' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "alert": {
                 "properties": {
                     "m": 0
                 }
             }
         }
     }'
```

**Force Sensors:**
```bash
curl -X PUT 'http://<your-ip>:8080/api/2/things/sensor1.fs:sensor01' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "accelerometer": {
                 "properties": {
                     "x": 5896,
                     "y": 456,
                     "z": 456
                 }
             },
             "gyroscope": {
                 "properties": {
                     "x": 45,
                     "y": 5,
                     "z": 5
                 }
             }
         }
     }'

curl -X PUT 'http://<your-ip>:8080/api/2/things/sensor2.fs:sensor02' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "accelerometer": {
                 "properties": {
                     "x": 5896,
                     "y": 456,
                     "z": 456
                 }
             },
             "gyroscope": {
                 "properties": {
                     "x": 45,
                     "y": 5,
                     "z": 5
                 }
             }
         }
     }'
```

**Pitch Angle Sensor:**
```bash
curl -X PUT 'http://<your-ip>:8080/api/2/things/sensor.pa:sensor' \
     -u 'ditto:ditto' \
     -H 'Content-Type: application/json' \
     -d '{
         "policyId": "my.test:policy",
         "attributes": {
             "Accelerometer": "M5core 2",
             "Gyroscope": "M5core 2",
             "location": "IFF"
         },
         "features": {
             "accelerometer": {
                 "properties": {
                     "x": 5896,
                     "y": 456,
                     "z": 456
                 }
             },
             "gyroscope": {
                 "properties": {
                     "x": 45,
                     "y": 5,
                     "z": 5
                 }
             }
         }
     }'
```
### 3. **Delete Things**
   ```bash
   curl -X DELETE 'http://<your-ip>:8080/api/2/things/<thing_id>' -u 'ditto:ditto' 
   ```
---

### 4. **Create Connections**

**Feedback Sensors:**
```bash
curl -X POST "http://<your-ip>:8080/devops/piggyback/connectivity?timeout=10" \
     -u 'devops:foobar' \
     -H 'Content-Type: application/json' \
     -d '{
         "targetActorSelection": "/system/sharding/connection",
         "headers": {
             "aggregate": false
         },
         "piggybackCommand": {
             "type": "connectivity.commands:createConnection",
             "connection": {
                 "id": "mqtt-s1connection-source",
                 "connectionType": "mqtt",
                 "connectionStatus": "open",
                 "failoverEnabled": true,
                 "uri": "tcp://<your-ip>:1885",
                 "sources": [{
                     "addresses": ["sensor1.iff/#"],
                     "authorizationContext": ["nginx:ditto"],
                     "qos": 0,
                     "filters": []
                 }],
                 "mappingContext": {
                     "mappingEngine": "JavaScript",
                     "options": {
                         "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { accelerometer: { properties: { x: jsonData.accx, y: jsonData.accy, z: jsonData.accz } }, gyroscope: { properties: { x: jsonData.gyrx, y: jsonData.gyry, z: jsonData.gyrz } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                         "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                         "loadByte

ArrayFromBase64": false
                     }
                 }
             }
         }
     }'
```

**Alert Sensors:**
```bash
curl -X POST "http://<your-ip>:8080/devops/piggyback/connectivity?timeout=10" \
     -u 'devops:foobar' \
     -H 'Content-Type: application/json' \
     -d '{
         "targetActorSelection": "/system/sharding/connection",
         "headers": {
             "aggregate": false
         },
         "piggybackCommand": {
             "type": "connectivity.commands:createConnection",
             "connection": {
                 "id": "mqtt-s2connection-source",
                 "connectionType": "mqtt",
                 "connectionStatus": "open",
                 "failoverEnabled": true,
                 "uri": "tcp://<your-ip>:1885",
                 "sources": [{
                     "addresses": ["mys1.alert/#"],
                     "authorizationContext": ["nginx:ditto"],
                     "qos": 0,
                     "filters": []
                 }],
                 "mappingContext": {
                     "mappingEngine": "JavaScript",
                     "options": {
                         "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { alert: { properties: { m: jsonData.alertM } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                         "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                         "loadByteArrayFromBase64": false
                     }
                 }
             }
         }
     }'
```

**Force Sensors:**
```bash
curl -X POST "http://<your-ip>:8080/devops/piggyback/connectivity?timeout=10" \
     -u 'devops:foobar' \
     -H 'Content-Type: application/json' \
     -d '{
         "targetActorSelection": "/system/sharding/connection",
         "headers": {
             "aggregate": false
         },
         "piggybackCommand": {
             "type": "connectivity.commands:createConnection",
             "connection": {
                 "id": "mqtt-s1fsconnection-source",
                 "connectionType": "mqtt",
                 "connectionStatus": "open",
                 "failoverEnabled": true,
                 "uri": "tcp://<your-ip>:1885",
                 "sources": [{
                     "addresses": ["sensor1.fs/#"],
                     "authorizationContext": ["nginx:ditto"],
                     "qos": 0,
                     "filters": []
                 }],
                 "mappingContext": {
                     "mappingEngine": "JavaScript",
                     "options": {
                         "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { accelerometer: { properties: { x: jsonData.accx, y: jsonData.accy, z: jsonData.accz } }, gyroscope: { properties: { x: jsonData.gyrx, y: jsonData.gyry, z: jsonData.gyrz } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                         "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                         "loadByteArrayFromBase64": false
                     }
                 }
             }
         }
     }'
```

**Pitch Angle Sensor:**
```bash
curl -X POST "http://<your-ip>:8080/devops/piggyback/connectivity?timeout=10" \
     -u 'devops:foobar' \
     -H 'Content-Type: application/json' \
     -d '{
         "targetActorSelection": "/system/sharding/connection",
         "headers": {
             "aggregate": false
         },
         "piggybackCommand": {
             "type": "connectivity.commands:createConnection",
             "connection": {
                 "id": "mqtt-s1pa-connection-source",
                 "connectionType": "mqtt",
                 "connectionStatus": "open",
                 "failoverEnabled": true,
                 "uri": "tcp://<your-ip>:1885",
                 "sources": [{
                     "addresses": ["sensor.pa/#"],
                     "authorizationContext": ["nginx:ditto"],
                     "qos": 0,
                     "filters": []
                 }],
                 "mappingContext": {
                     "mappingEngine": "JavaScript",
                     "options": {
                         "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { accelerometer: { properties: { x: jsonData.accx, y: jsonData.accy, z: jsonData.accz } }, gyroscope: { properties: { x: jsonData.gyrx, y: jsonData.gyry, z: jsonData.gyrz } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                         "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                         "loadByteArrayFromBase64": false
                     }
                 }
             }
         }
     }'
```

---

**Establishing Connection Between Things and Kafka**

Before creating the connection, ensure you create a Kafka topic to subscribe from the Things. This topic should be created as `"sensor"`, which will provide values for all the Things.

**Sensor 1 - Modifying the Things `mys1.alert` through Kafka:**
```bash
curl -i -X POST -u devops:foobar \
     -H 'Content-Type: application/json' \
     -d '{
        "targetActorSelection": "/system/sharding/connection",
        "headers": {
            "aggregate": false
        },
        "piggybackCommand": {
            "type": "connectivity.commands:createConnection",
            "connection": {
                "id": "kafka-s1connection-source",
                "connectionType": "kafka",
                "connectionStatus": "open",
                "failoverEnabled": true,
                "uri": "tcp://192.168.50.234:9092",
                "specificConfig": {
                    "bootstrapServers": "192.168.50.234:29093",
                    "saslMechanism": "plain"
                },
                "sources": [
                    {
                        "addresses": [
                            "mys1.alert"
                        ],
                        "consumerCount": 1,
                        "qos": 0,
                        "authorizationContext": [
                            "nginx:ditto"
                        ],
                        "headerMapping": {},
                        "payloadMapping": [
                            "javascript"
                        ],
                        "replyTarget": {
                            "address": "{{header:reply-to}}",
                            "headerMapping": {},
                            "expectedResponseTypes": [
                                "response",
                                "error"
                            ],
                            "enabled": true
                        }
                    }
                ],
                "targets": [],
                "clientCount": 1,
                "validateCertificates": true,
                "processorPoolSize": 1,
                "mappingDefinitions": {
                    "javascript": {
                        "mappingEngine": "JavaScript",
                        "options": {
                           "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                           "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { alert: { properties: { m: jsonData.m } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                           "loadBytebufferJS": "false",
                           "loadLongJS": "false"
                        }
                    }
                }
            }
        }
    }' http://192.168.50.142:8080/devops/piggyback/connectivity
```

**Sensor 2 - Modifying the Things `mys2.alert` through Kafka:**
```bash
curl -i -X POST -u devops:foobar \
     -H 'Content-Type

: application/json' \
     -d '{
        "targetActorSelection": "/system/sharding/connection",
        "headers": {
            "aggregate": false
        },
        "piggybackCommand": {
            "type": "connectivity.commands:createConnection",
            "connection": {
                "id": "kafka-s2connection-source",
                "connectionType": "kafka",
                "connectionStatus": "open",
                "failoverEnabled": true,
                "uri": "tcp://192.168.50.234:9092",
                "specificConfig": {
                    "bootstrapServers": "192.168.50.234:29093",
                    "saslMechanism": "plain"
                },
                "sources": [
                    {
                        "addresses": [
                            "mys2.alert"
                        ],
                        "consumerCount": 1,
                        "qos": 0,
                        "authorizationContext": [
                            "nginx:ditto"
                        ],
                        "headerMapping": {},
                        "payloadMapping": [
                            "javascript"
                        ],
                        "replyTarget": {
                            "address": "{{header:reply-to}}",
                            "headerMapping": {},
                            "expectedResponseTypes": [
                                "response",
                                "error"
                            ],
                            "enabled": true
                        }
                    }
                ],
                "targets": [],
                "clientCount": 1,
                "validateCertificates": true,
                "processorPoolSize": 1,
                "mappingDefinitions": {
                    "javascript": {
                        "mappingEngine": "JavaScript",
                        "options": {
                            "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
                            "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { alert: { properties: { m: jsonData.m } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
                            "loadBytebufferJS": "false",
                            "loadLongJS": "false"
                        }
                    }
                }
            }
        }
    }' http://192.168.50.142:8080/devops/piggyback/connectivity
```

**Subscribing from the Things through Kafka:**
```bash
curl -i -X POST "http://192.168.50.142:8080/devops/piggyback/connectivity?timeout=10" \
     -u devops:foobar \
     -H 'Content-Type: application/json' \
     -d '{
    "targetActorSelection": "/system/sharding/connection",
    "headers": {
        "aggregate": false
    },
    "piggybackCommand": {
        "type": "connectivity.commands:createConnection",
        "connection": {
            "id": "kafka-s2connection-target",
            "connectionType": "kafka",
            "connectionStatus": "open",
            "failoverEnabled": true,
            "uri": "tcp://192.168.50.234:9092",
            "specificConfig": {
                "bootstrapServers": "192.168.50.234:29093",
                "saslMechanism": "plain"
            },
            "sources": [],
            "targets": [{
                "address": "sensor/{{ thing:id }}",
                "topics": [                             
                    "_/_/things/twin/events",
                    "_/_/things/live/messages"
                ],
                "authorizationContext": ["nginx:ditto"],
                "qos": 0                      
            }],
            "clientCount": 1,
            "validateCertificates": true,
            "processorPoolSize": 1,
            "mappingContext": {
                "mappingEngine": "JavaScript",
                "options": {
                    "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) {return null;}",
                    "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { let textPayload = \"{\\\"accx\\\": \" + value.accelerometer.properties.x + \", \\\"accy\\\": \" + value.accelerometer.properties.y + \", \\\"accz\\\": \" + value.accelerometer.properties.z + \", \\\"gyrx\\\": \" + value.gyroscope.properties.x + \", \\\"gyry\\\": \" + value.gyroscope.properties.y + \", \\\"gyrz\\\": \" + value.gyroscope.properties.z + \", \\\"thingId\\\": \\\"\" + namespace + \":\" + id + \"\\\"}\"; let bytePayload = null; let contentType = \"text/plain; charset=UTF-8\"; return Ditto.buildExternalMsg(dittoHeaders, textPayload, bytePayload, contentType); }",
                    "loadBytebufferJS": "false",
                    "loadLongJS": "false"
                }
            }
        }
    }'
```

**Post-Setup Verification:**
Once all connections are created and established, refresh the connection page to verify that:
- Under "Live Status," it shows "open."
- Under "Recovery Status," it shows "succeeded."

This confirms that the connection is successful and the Things are now connected with the Brokers.

---






