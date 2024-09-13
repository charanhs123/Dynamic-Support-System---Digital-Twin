
Setting up Ditto

Clone the official repository from Eclipse Ditto

1. git clone https://github.com/eclipse-ditto/ditto-examples.git

Navigate to docker-compose file of Ditto

2. cd ditto/deployment/docker

Compose up the docker file

3.  docker-compose up -d

Now open web browser and type localhost:8080 / IP address:8080 to open the official Ditto page 





Setting up BaSyx 

Open the BaSyx folder

1. Use the docker-compose file and compose it up , as it sets up BaSyx AAS Server and BaSyx AAS 
   
   Registry. 

  ( Make sure you set up BaSyx before creating Policies, Things and Conenctions )

   
Once the AASServer, AASGui and AASRegistry and DItto images are running and helathy , use the following command sto establish connection between BaSyx and Ditto to access and MOdify Ditto Things, Policies, etc. 



Open a Terminal and user the following Commands


1. Command for Creating Connection between AASServer and Ditto 



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
        "uri": "http://System_IP:PORT",
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
  }' "http://ADD_System_IP:8080/devops/piggyback/connectivity?timeout=10"



  NOTE: Change the IP address and Port
	Use your System IP address fr ex- 192.168.0.105

	1. "http://ADD_System_IP:8080/devops/piggyback/connectivity?timeout=10"
		Here use your IP address and the port number is where DItto is accesible 
	2. "uri": "http://System_IP:PORT",
		Here use yoir IP address and th Port mentioned in the BaSyx docker-compsoe.yml file for AASServer ,for ex-8082



2. Command for Creating Connection between AASRegistry and Ditto 


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
    }
  }' "http://192.168.50.142:8080/devops/piggyback/connectivity?timeout=10"



 
  NOTE: Use the above steps 
  
  
 
3. Access the AASGui using the local Browser " localhost:5000"

   NOTE: Use the Port mentioned in docker-compose.yml file 
   
   add the URL of AASServer and AASRegistry at the endpoints on the Browser and CLick connect to to access the "Things" on BaSyx Gui

   ex- http://192.168.50.142:8082/registry
       http://192.168.50.142:8083/aasServer 	
	



Also the Ditto elements can be modified from Terminal through BaSyx using following Examples 


NOTE : after the Connection for BaSyx and Ditto is Extablished first Create Policy,THings etc and then try to access them using the commands below
       Also in other way it can be created using the following examples from Terminal


Example*****************************************************************************************************************************

Creating a Thing in Eclipse Ditto
Setup a common policy
To define authorization information to be used by the Things, we first create a policy with the policy-id machine:my-policy.

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
Listing 8: Demo Policy Definition

You will get a 201 Created response, if the policy creation concluded successfuly. In the subsequent steps, we use the policy-id machine:my-policy to refer to the created policy.

Create a Thing
The next step is to create an actual Thing. We use the namespace and name machine:my-policy and policy-id machine:my-policy here:

NAMESPACE=machine
NAME=sensor
DEVICE_ID=$NAMESPACE:$NAME

curl -i -X PUT -u ditto:ditto -H 'Content-Type: application/json' --data '{
  "policyId": "'$POLICY_ID'"
}' <ditto-instance-url>/api/2/things/$DEVICE_ID
Listing 9: Request to add the Demo Policy to a Ditto instance ($POLICY_ID refers to Listing 8)

Again, a successful creation returns a 201 Created response.

We earlier configured two connections to trigger a mapper on the create event of a Thing. This should push a new AAS to the AAS server and a reference to that AAS in the AAS registry.

You can check whether the execution of the scripts was successful by requesting the shell at the AAS server:

curl -X GET <basyx-server-instance-url>:4001/aasServer/shells
which should return the following result

[{"modelType":{"name":"AssetAdministrationShell"},"idShort":"machine","identification":{"idType":"Custom","id":"machine"},"dataSpecification":[],"embeddedDataSpecifications":[],"submodels":[{"keys":[{"type":"AssetAdministrationShell","local":true,"value":"machine","idType":"Custom"},{"type":"Submodel","local":true,"value":"sensor_temperature","idType":"Custom"}]}],"asset":{"keys":[{"type":"Asset","local":true,"value":"machine-asset","idType":"Custom"}],"identification":{"idType":"Custom","id":"machine-asset"},"idShort":"machine-asset","kind":"Instance","dataSpecification":[],"modelType":{"name":"Asset"},"embeddedDataSpecifications":[]},"views":[],"conceptDictionary":[]}]
In addition, the request to the AAS registry:

curl -X GET <basyx-registry-instance-url>:4000/registry/api/v1/registry
should return:

[{"modelType":{"name":"AssetAdministrationShellDescriptor"},"endpoints":[{"address":"<basyx-server-instance-url>:4001/aasServer/shells/machine/aas","type":"http"}],"identification":{"idType":"Custom","id":"machine"},"idShort":"machine","asset":{"identification":{"idType":"Custom","id":"machine-asset"},"idShort":"machine-asset","kind":"Instance","dataSpecification":[],"modelType":{"name":"Asset"},"embeddedDataSpecifications":[]},"submodels":[]}]
At this point, the newly created Thing has no features, properties, or attributes yet. So let us populate that Thing.

Create a feature for the Thing
Next, we create a feature for the Thing to contain a property with the data of a temperature sensor.

FEATURE_ID=temperature

curl -X PUT -u ditto:ditto -H 'Content-Type: application/json' --data-binary '{
  "properties": {
    "value": null
  }
}' <ditto-instance-url>/api/2/things/$DEVICE_ID/features/$FEATURE_ID
Listing 10: Request to add a feature to the demo Thing (variables refer to previous Listings)

The feature creation triggers the mapper (mappingforSubmodel) to create a corresponding Submodel in the previously created AAS.

To check if this was successful, we request the expected submodel:

curl -X GET <basyx-server-instance-url>:4001/aasServer/shells/$NAMESPACE/aas/submodels/${NAME}_${FEATURE_ID}/submodel
which should result in the following response:

{"parent":{"keys":[{"idType":"Custom","type":"AssetAdministrationShell","value":"machine","local":true}]},"identification":{"idType":"Custom","id":"sensor_temperature"},"idShort":"sensor_temperature","kind":"Instance","dataSpecification":[],"modelType":{"name":"Submodel"},"embeddedDataSpecifications":[],"submodelElements":[]}
Updating a Thing
After we have successfully created a Thing, we can check if the update of a property works as well by executing:

curl -i -X PUT -u ditto:ditto -H "content-type: application/json" --data-binary '46' <ditto-instance-url>/api/2/things/$DEVICE_ID/features/$FEATURE_ID/properties/value
Again, we check if our change was successful:

curl -u ditto:ditto -w '\n' <ditto-instance-url>/api/2/things/$DEVICE_ID
and expect:

{"thingId":"machine:sensor","policyId":"machine:my-policy","features":{"temperature":{"properties":{"value":46}}}}
If the property creation was successful, then the mapping mappingforSubmodelElement should trigger. To verify that the Submodel was updated, call:

curl -X GET <basyx-server-instance-url>:4001/aasServer/shells/$NAMESPACE/aas/submodels/${NAME}_${FEATURE_ID}/submodel/submodelElements/properties_value
This should lead to the response:

{"parent":{"keys":[{"idType":"Custom","type":"Submodel","value":"sensor_temperature","local":true}]},"idShort":"properties_value","kind":"Instance","valueType":"int","modelType":{"name":"Property"},"value":46}
Here, we see that we are able to access the sensor data of the device through the AAS Submodel API via Eclipse BaSyx.

As an alternative to plain Json responses, you can use one of the UI-tools provided by the AAS community, like the AAS Web UI.









Next go to Creating and Conenction Connection Tutorial.



