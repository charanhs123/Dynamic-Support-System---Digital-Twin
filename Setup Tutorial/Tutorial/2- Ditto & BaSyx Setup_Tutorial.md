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
         "uri": "http://<System_IP>:<PORT>",
         "failoverEnabled": true,
         "mappingDefinitions": {
           "mappingforShell": {
             "mappingEngine": "JavaScript",
             "options": {
               "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { ... }"
             }
           },
           "mappingforSubmodel": {
             "mappingEngine": "JavaScript",
             "options": {
               "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { ... }"
             }
           },
           "mappingforSubmodelElement": {
             "mappingEngine": "JavaScript",
             "options": {
               "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { ... }"
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
               "_/_/things/twin/events?filter=and(in(topic:action,'created'),eq(resource:path,'/'))"
             ],
             "payloadMapping": [
               "mappingforShell"
             ]
           },
           {
             "address": "PUT:/aasServer/shells/{{ thing:namespace }}/aas/submodels/{{ thing:name }}_{{ resource:path | fn:substring-after('/features/') }}",
             "headerMapping": {
               "content-type": "{{ header:content-type }}"
             },
             "authorizationContext": ["nginx:ditto"],
             "topics": [
               "_/_/things/twin/events?filter=and(in(topic:action,'created'),not(eq(resource:path,'/features')),like(resource:path,'/features*'),not(like(resource:path,'*properties*')))"
             ],
             "payloadMapping": [
               "mappingforSubmodel"
             ]
           },
           {
             "address": "PUT:/aasServer/shells/{{ thing:namespace }}/aas/submodels/{{ thing:name }}_{{ resource:path | fn:substring-after('/features/') | fn:substring-before('/properties/') }}/submodel/submodelElements/properties_{{ resource:path | fn:substring-after('/properties/') | fn:replace('/', '_') }}",
             "headerMapping": {
               "content-type": "{{ header:content-type }}"
             },
             "authorizationContext": ["nginx:ditto"],
             "topics": [
               "_/_/things/twin/events?filter=and(in(topic:action,'modified'),not(eq(resource:path,'/features')),like(resource:path,'/features*'),like(resource:path,'*properties*'),not(like(resource:path,'*properties')))"
             ],
             "payloadMapping": [
               "mappingforSubmodelElement"
             ]
           }
         ]
       }
     }
   }' "http://<ADD_System_IP>:8080/devops/piggyback/connectivity?timeout=10"
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
         "uri": "http://192.168.50.142:8083",
         "failoverEnabled": true,
         "mappingDefinitions": {
           "mappingforShell": {
             "mappingEngine": "JavaScript",
             "options": {
               "outgoingScript": "function mapFromDittoProtocolMsg(namespace, name, group, channel, criterion, action, path, dittoHeaders, value, status, extra) { ... }"
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
               "_/_/things/twin/events?filter=and(in(topic:action,'created'),eq(resource:path,'/'))"
             ],
             "payloadMapping": [
               "mappingforShell"
             ]
           }
         ]
       }
     }
   }' "http://192.168.50.142:8080/devops/piggyback/connectivity?timeout=10"
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
   }' <ditto-instance-url>/api/2/things/$

DEVICE_ID/features/$FEATURE_ID
   ```

5. **Verify Feature:**
   ```bash
   curl -X GET <ditto-instance-url>/api/2/things/$DEVICE_ID/features
   ```


## **Next Steps**

Go to the [Creating & Conenction_Tutorial](#) for further instructions.
