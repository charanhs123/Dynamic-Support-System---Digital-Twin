Open a Terminal and paste the commands as below to create Policies , Things and Connections

Policies
This provides the access to the Things , like read and write options. Also with the same policy ID we can give access to number of Things we create.

command for setting up the policy ( Sample IP : 192.168.50.142 --- Use the IP of your System)

curl -X PUT 'http://192.168.50.142:8080/api/2/policies/my.test:policy' -u 'ditto:ditto' -H 'Content-Type: application/json' --data '{
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



Create a Thing

Definition
A Thing may contain a definition. The definition can also be used to find Things. The definition is used to link a thing to a corresponding model defining the capabilities/features of it.
The definition can for example point to a:
					a valid HTTP(s) URL (e.g. in order to define that the Thing is described by a WoT (Web of Things) Thing Model)
					or some other model reference using the syntax 
					
					<namespace>:<name>:<version>
					
				        ex -  org.eclipse.ditto:xdk_53
					      foo:xdk_53
					      org.eclipse.vorto_42:xdk_thing
				        
				        
				        
command:

NOTE: use your System IP and the assigned Port number

1. Things for Sensors for Feedback System

curl -X PUT 'http://192.168.50.142:8080/api/2/things/sensor1.iff:sensor01' \
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

curl -X PUT 'http://192.168.50.142:8080/api/2/things/sensor2.iff:sensor02' \
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


2. Things for ALert 



curl -X PUT 'http://192.168.50.142:8080/api/2/things/mys1.alert:sensor' \
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

curl -X PUT 'http://192.168.50.142:8080/api/2/things/mys2.alert:sensor' \
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
     
 
3. Things for Force Sensors


curl -X PUT 'http://192.168.50.142:8080/api/2/things/sensor1.fs:sensor01' \
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




curl -X PUT 'http://192.168.0.106:8080/api/2/things/sensor2.fs:sensor02' \
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


4. Thing for Pitch Angle

 curl -X PUT 'http://192.168.0.106:8080/api/2/things/sensor.pa:sensor' \
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
 




Create a Connection between the things and Mosquitto Broker and Kafka 

1.Establishing Conenction between Things and Mosquitto Broker for Publishing and subscribing


Feedback Sensors

sensor1---
     
curl -X POST "http://192.168.50.142:8080/devops/piggyback/connectivity?timeout=10"      -u 'devops:foobar'      -H 'Content-Type: application/json'      -d '{
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
                "uri": "tcp://192.168.50.142:1885",
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
        "loadBytebufferJS": false,
        "loadLongJS": false                                                                                                                                                                                }                                                                                                                             
                }                                   
            }                                
        }            
    }'


sensor2---


curl -X POST "http://192.168.50.142:8080/devops/piggyback/connectivity?timeout=10"      -u 'devops:foobar'      -H 'Content-Type: application/json'      -d '{
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
                "uri": "tcp://192.168.50.142:1888",
                "sources": [{
                    "addresses": ["sensor2.iff/#"],
                    "authorizationContext": ["nginx:ditto"],
                    "qos": 0,                    
                    "filters": []
                }],
                "mappingContext": {
                    "mappingEngine": "JavaScript",
                    "options": {
                        "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { accelerometer: { properties: { x: jsonData.accx, y: jsonData.accy, z: jsonData.accz } }, gyroscope: { properties: { x: jsonData.gyrx, y: jsonData.gyry, z: jsonData.gyrz } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
        "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
        "loadBytebufferJS": false,
        "loadLongJS": false                                                                                                                                                                                 }                                                                                                                             
                }                                   
            }                                
        }            
    }'
     
     
Force Sensors


Sensor 1 ----


curl -X POST "http://192.168.0.106:8080/devops/piggyback/connectivity?timeout=10"      -u 'devops:foobar'      -H 'Content-Type: application/json'      -d '{
        "targetActorSelection": "/system/sharding/connection",
        "headers": {
            "aggregate": false
        },
        "piggybackCommand": {
            "type": "connectivity.commands:createConnection",
            "connection": {
                "id": "mqtt-f1connection-source",
                "connectionType": "mqtt",
                "connectionStatus": "open",
                "failoverEnabled": true,
                "uri": "tcp://192.168.0.106:1887",
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
        "loadBytebufferJS": false,
        "loadLongJS": false                                                                                                                                                                                }                                                                                                                             
                }                                   
            }                                
        }            
    }'



Sensor 2 -------



curl -X POST "http://192.168.0.106:8080/devops/piggyback/connectivity?timeout=10"      -u 'devops:foobar'      -H 'Content-Type: application/json'      -d '{
        "targetActorSelection": "/system/sharding/connection",
        "headers": {
            "aggregate": false
        },
        "piggybackCommand": {
            "type": "connectivity.commands:createConnection",
            "connection": {
                "id": "mqtt-f2connection-source",
                "connectionType": "mqtt",
                "connectionStatus": "open",
                "failoverEnabled": true,
                "uri": "tcp://192.168.0.106:1889",
                "sources": [{
                    "addresses": ["sensor2.fs/#"],
                    "authorizationContext": ["nginx:ditto"],
                    "qos": 0,                    
                    "filters": []
                }],
                "mappingContext": {
                    "mappingEngine": "JavaScript",
                    "options": {
                         "incomingScript": "function mapToDittoProtocolMsg(headers, textPayload, bytePayload, contentType) { const jsonString = String.fromCharCode.apply(null, new Uint8Array(bytePayload)); const jsonData = JSON.parse(jsonString); const thingId = jsonData.thingId.split(\":\"); const value = { accelerometer: { properties: { x: jsonData.accx, y: jsonData.accy, z: jsonData.accz } }, gyroscope: { properties: { x: jsonData.gyrx, y: jsonData.gyry, z: jsonData.gyrz } } }; return Ditto.buildDittoProtocolMsg(thingId[0], thingId[1], \"things\", \"twin\", \"commands\", \"modify\", \"/features\", headers, value); }",
        "outgoingScript": "function mapFromDittoProtocolMsg(namespace, id, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { return null; }",
        "loadBytebufferJS": false,
        "loadLongJS": false                                                                                                                                                                                }                                                                                                                             
                }                                   
            }                                
        }            
    }'


Ptch angle Sensor


curl -X POST "http://192.168.0.106:8080/devops/piggyback/connectivity?timeout=10"      -u 'devops:foobar'      -H 'Content-Type: application/json'      -d '{
        "targetActorSelection": "/system/sharding/connection",
        "headers": {
            "aggregate": false
        },
        "piggybackCommand": {
            "type": "connectivity.commands:createConnection",
            "connection": {
                "id": "mqtt-paconnection-source",
                "connectionType": "mqtt",
                "connectionStatus": "open",
                "failoverEnabled": true,
                "uri": "tcp://192.168.0.106:1885",
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
        "loadBytebufferJS": false,
        "loadLongJS": false                                                                                                                                                                                }                                                                                                                             
                }                                   
            }                                
        }            
    }'







Establishing connection between Things and Kafka

Before creating the connection make you create a Kafka topic to subscribe fromt the Things, so we can see in the Third command below under under Targets 														

(***	"targets": [{
                "address": "sensor/{{ thing:id }}",
****)


the kafka topic created as "sensor" and it gives us the values of all the things !

NOTE: Make sure Kafka broker on your System is working 

Sensor 1-  Modifying the Things mys1.alert through kafka


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



Sensor 2 - Modifying the Things mys2.alert through kafka


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


Subscribing from the Things through Kafka

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
        }
    }'



so once all this is created and the connections is established just refresh the connection page to see under live status "open" and under recovery status "succeeded"

this means the connection is successfull and the now the Things are connected with the Brokers
