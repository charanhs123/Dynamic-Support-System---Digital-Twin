Certainly! Here's a cleaner and more organized version of the setup guide:

---

## Setting Up Eclipse Ditto

1. **Clone the Ditto Repository:**
   ```bash
   git clone https://github.com/eclipse-ditto/ditto.git
   ```

2. **Navigate to the Docker-Compose Directory:**
   ```bash
   cd ditto/deployment/docker
   ```

3. **Start Ditto Using Docker Compose:**
   ```bash
   docker-compose up -d
   ```

4. **Access Ditto:**
   Open your web browser and go to `http://localhost:8080` or `http://<IP-address>:8080`.

5. **Setting up IP when using Ditto on different Machine**
   There is no need to change IP while using Ditto on the browser from the Host Ubuntu Desktop

   However when Ubuntu Server is used , Ditto can be accessed from the Browser of other machines by changing the IP in Environment to the respective system IP address
   

   ![image](https://github.com/user-attachments/assets/edabc64e-e2f5-4409-b08c-e8aec877807a)


---

## Setting Up BaSyx

1. **Navigate to the BaSyx Directory:**
   Use the Docker Compose file to set up BaSyx AAS Server and BaSyx AAS Registry. Ensure BaSyx is configured before creating Policies, Things, and Connections.

2. **Start BaSyx Components:**
   Run the following command to establish a connection between BaSyx and Ditto, enabling access and modification of Ditto Things and Policies:
   ```bash
   curl -X POST -u devops:foobar -H 'Content-Type: application/json' --data-binary '{
    "targetActorSelection": "/system/sharding/connection",
    "headers": {
      "aggregate": false
    },
    "piggybackCommand": {
      "type": "connectivity.commands:createConnection",
      "connection": {
        "id": "basyxserver-http-connection",
        "connectionType": "http-push",
        "connectionStatus": "open",
        "uri": "htpp://<System IP:PORT.>",
        "failoverEnabled": true,
        "mappingDefinitions": {
          "mappingforShell": {
            "mappingEngine": "JavaScript",
            "options": {
              "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) {\n  let headers = dittoHeaders;\n  let textPayload = JSON.stringify({\n    conceptDictionary: [],\n    identification: {\n      idType: '"'Custom'"',\n      id: namespace\n    },\n    idShort: namespace,\n    dataSpecification: [],\n    modelType: {\n      name: '"'AssetAdministrationShell'"'\n    },\n    asset: {\n      identification: {\n        idType: '"'Custom'"',\n        id: namespace + '"'-asset'"'\n      },\n      idShort: namespace + '"'-asset'"',\n      kind: '"'Instance'"',\n      dataSpecification: [],\n      modelType: {\n        name: '"'Asset'"'\n      },\n      embeddedDataSpecifications: []\n    },\n    embeddedDataSpecifications: [],\n    views: [],\n    submodels: []\n  });\n  let bytePayload = null;\n  let contentType = '"'application/json'"';\n  return Ditto.buildExternalMsg(headers, textPayload, bytePayload, contentType);}"            
            }
          },
          "mappingforSubmodel": {
            "mappingEngine": "JavaScript",
            "options": {
                "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) {\n  \n  let feature_id = path.split('"'/'"').slice(2);\n  let headers = dittoHeaders;\n  let textPayload = JSON.stringify(\n    {\n      parent: {\n        keys: [\n          {\n            idType: '"'Custom'"',\n            type: '"'AssetAdministrationShell'"',\n            value: namespace,\n            local: true\n          }\n        ]\n      },\n      identification: {\n        idType: '"'Custom'"',\n        id: name+'"'_'"'+feature_id\n      },\n      idShort: name+'"'_'"'+feature_id,\n      kind: '"'Instance'"',\n      dataSpecification: [],\n      modelType: {\n        name: '"'Submodel'"'\n      },\n      embeddedDataSpecifications: [],\n      submodelElements: []\n    }\n\n  );\n  let bytePayload = null;\n  let contentType = '"'application/json'"';\n  return Ditto.buildExternalMsg(headers, textPayload, bytePayload, contentType);}"
            }
          },
          "mappingforSubmodelElement": {
            "mappingEngine": "JavaScript",
            "options": {
              "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) {\n  let property_id = path.split('"'/'"').slice(3).join('"'_'"');\n  let feature_id = path.split('"'/'"').slice(2,3);\n  let headers = dittoHeaders;\n  let dataType = typeof value;\n  dataType = mapDataType(dataType)\n\n  function mapDataType(dataType) {\n    switch (dataType) {\n        case '"'undefined'"':\n        return '"'Undefined'"';\n        case '"'boolean'"':\n        return '"'boolean'"';\n        case '"'number'"':\n        return '"'int'"';\n        case '"'string'"':\n        return '"'string'"';\n        case '"'symbol'"':\n        return '"'Symbol'"';\n        case '"'bigint'"':\n        return '"'BigInt'"';\n        case '"'object'"':\n        return '"'string'"';\n        case '"'function'"':\n        return '"'Function'"';\n        default:\n        return '"'Unknown'"';\n    }\n  }\n  let textPayload = JSON.stringify(\n  {\n    parent: {\n      keys: [\n        {\n          idType: '"'Custom'"',\n          type: '"'Submodel'"',\n          value: name+'"'_'"'+feature_id,\n          local: true\n        }\n      ]\n    },\n    idShort: property_id,\n    kind: '"'Instance'"',\n    valueType: dataType,\n    modelType: {\n      name: '"'Property'"'\n    },\n    value: value\n  }\n  );\n  let bytePayload = null;\n  let contentType = '"'application/json'"';\n  return Ditto.buildExternalMsg(headers, textPayload, bytePayload, contentType);}"
            }
          }
        },
        "sources": [],
        "targets": [
          {
            "address": "PUT:/aasServer/shells/{{ thing:namespace }}",
            "headerMapping": {
              "content-type": "{{ header:content-type }}"
            },
            "authorizationContext": ["nginx:ditto"],
            "topics": [
              "_/_/things/twin/events?filter=and(in(topic:action,'"'created'"'),eq(resource:path,'"'/'"'))"
            ],
            "payloadMapping": [
              "mappingforShell"
            ]
          },
          {
            "address": "PUT:/aasServer/shells/{{ thing:namespace }}/aas/submodels/{{ thing:name }}_{{ resource:path | fn:substring-after('"'/features/'"') }}",
            "headerMapping": {
              "content-type": "{{ header:content-type }}"
            },
            "authorizationContext": ["nginx:ditto"],
            "topics": [
              "_/_/things/twin/events?filter=and(in(topic:action,'"'created'"'),not(eq(resource:path,'"'/features'"')),like(resource:path,'"'/features*'"'),not(like(resource:path,'"'*properties*'"')))"
            ],
            "payloadMapping": [
              "mappingforSubmodel"
            ]
          },
          {
            "address": "PUT:/aasServer/shells/{{ thing:namespace }}/aas/submodels/{{ thing:name }}_{{ resource:path | fn:substring-after('"'/features/'"') | fn:substring-before('"'/properties'"') }}/submodel/submodelElements/properties_{{ resource:path | fn:substring-after('"'/properties/'"') | fn:replace('"'/'"','"'_'"') }}",
            "headerMapping": {
              "content-type": "{{ header:content-type }}"
            },
            "authorizationContext": ["nginx:ditto"],
            "topics": [
              "_/_/things/twin/events?filter=and(in(topic:action,'"'modified'"'),not(eq(resource:path,'"'/features'"')),like(resource:path,'"'/features*'"'),like(resource:path,'"'*properties*'"'),not(like(resource:path,'"'*properties'"')))"
            ],
            "payloadMapping": [
              "mappingforSubmodelElement"
            ]
          }
        ]
      }
    }
   }' htpp://<System IP:PORT.>/devops/piggyback/connectivity
   ```

   **Note:** Replace `<System_IP>` and `<PORT>` with your actual IP address and port number (e.g., `192.168.0.105`, `8082`).

3. **Create Connection Between AASRegistry and Ditto:**
   ```bash
   curl -X POST -u devops:foobar -H 'Content-Type: application/json' --data-binary '{
    "targetActorSelection": "/system/sharding/connection",
    "headers": {
      "aggregate": false
    },
    "piggybackCommand": {
      "type": "connectivity.commands:createConnection",
      "connection": {
        "id": "basyxregistry-http-connection",
        "connectionType": "http-push",
        "connectionStatus": "open",
        "uri": "htpp://<System IP:PORT.>",
        "failoverEnabled": true,
        "mappingDefinitions": {
          "mappingforShell": {
            "mappingEngine": "JavaScript",
            "options": {
              "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) {\n  let headers = dittoHeaders;\n  let textPayload = JSON.stringify({\n    endpoints: [\n        {\n            address: '"'<basyx-server-instance-url>:4001/aasServer/shells/'"' + namespace + '"'/aas'"',\n            type: '"'http'"'\n        }\n    ],\n    modelType: {\n        name: '"'AssetAdministrationShellDescriptor'"'\n    },\n    identification: {\n        idType: '"'Custom'"',\n        id: namespace\n},\n    idShort: namespace,\n      asset: {\n          identification: {\n              idType: '"'Custom'"',\n              id: namespace + '"'-asset'"'\n          },\n          idShort: namespace + '"'-asset'"',\n          kind: '"'Instance'"',\n          dataSpecification: [],\n          modelType: {\n              name: '"'Asset'"'\n          },\n          embeddedDataSpecifications: []\n      },\n      submodels: []\n  });\n  let bytePayload = null;\n  let contentType = '"'application/json'"';\n  return Ditto.buildExternalMsg(headers, textPayload, bytePayload, contentType);}"
            }
          }
        },
        "sources": [],
        "targets": [
          {
            "address": "PUT:/registry/api/v1/registry/{{ thing:namespace }}",
            "headerMapping": {
              "content-type": "{{ header:content-type }}"
            },
            "authorizationContext": ["nginx:ditto"],
            "topics": [
              "_/_/things/twin/events?filter=and(in(topic:action,'"'created'"'),eq(resource:path,'"'/'"'))"
            ],
            "payloadMapping": [
              "mappingforShell"
            ]
          }
        ]
      }
    }}' htpp://<System IP:PORT.>/devops/piggyback/connectivity
   ```

4. **Access AASGui:**
   Open your browser and go to `http://localhost:5000` (or the port specified in your `docker-compose.yml` file). Add the URLs for AASServer and AASRegistry and click "Connect" to access the "Things" on the BaSyx GUI.
   - Example URLs:
     - `http://192.168.50.142:8082/registry`
     - `http://192.168.50.142:8083/aasServer`

---

## Modifying Ditto Elements via BaSyx

After establishing the connection between BaSyx and Ditto, you can create and modify Policies, Things, etc. Here are examples:

### Creating a Thing in Eclipse Ditto

1. **Setup a Common Policy:**
   ```bash
   POLICY_ID=machine:my-policy
   curl -i -X PUT -u ditto:ditto -H 'Content-Type: application/json' --data '{
     "entries": {
       "DEFAULT": {
         "subjects": {
           "{{ request:subjectId }}": {
             "type": "Ditto user authenticated via nginx"
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
       }
     }
   }' <ditto-instance-url>/api/2/policies/$POLICY_ID
   ```

2. **Create a Thing:**
   ```bash
   NAMESPACE=machine
   NAME=sensor
   DEVICE_ID=$NAMESPACE:$NAME

   curl -i -X PUT -u ditto:ditto -H 'Content-Type: application/json' --data '{
     "policyId": "'$POLICY_ID'"
   }' <ditto-instance-url>/api/2/things/$DEVICE_ID
   ```

3. **Verify Creation:**
   ```bash
   curl -X GET <basyx-server-instance-url>:4001/aasServer/shells
   curl -X GET <basyx-registry-instance-url>:4000/registry/api/v1/registry
   ```

4. **Create a Feature for the Thing:**
   ```bash
   FEATURE_ID=temperature

   curl -X PUT -u ditto:ditto -H 'Content-Type: application/json' --data-binary '{
     "properties": {
       "value": null
     }
   }' <ditto-instance-url>/api/2/things/$DEVICE_ID/features/$FEATURE_ID
   ```

5. **Verify Feature:**
   ```bash
   curl -X GET <ditto-instance-url>/api/2/things/$DEVICE_ID/features
   ```


## **Next Steps**

Go to the [Creating & Conenction_Tutorial](#) for further instructions.
