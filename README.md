# TD/OMS API

In order to use the TD/OMS REST API, the rest server must be configured and started. [How this is done is described on our wiki](https://remainsoftware.com/wiki/index.php?title=OCTO:Open_Core_for_Technology_Orchestration#Getting_Started).

## Usage

The TD/OMS Rest API's follow the OAS3 specification. Links to the swagger editor can be found below for each API.

### [Login](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/Login.json)[REST Login API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/Login.json)

Use the [REST Login API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/Login.json) to retrieve a JWT token for further communication.

<details><summary>Request</summary>
	
```
curl -X 'POST' \
  'https://plato.remainsoftware.com:45111/OMSRUN51/OMGAUTH/login' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "password": "password",
  "email_or_username": "user"
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "token": "token"
}
```
</details>

### [Database](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/OMRDBA_DataBase_API.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/OMRDBA_DataBase_API.json) to access the database. The database tables and fields are described in our data dictionary file OMDDI.

<details><summary>Request</summary>

```shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45111/OMSRUN51/OMRDBA/get' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "listInput": {
    "pageNumber": 1,
    "returnTotalRecords": true,
    "recordsPerPage": 50,
    "returnFields": [
      "applicationCode",
      "fixNumber",
      "shortFixDescription",
      "programmer"
    ],
    "table": "OMXFix",
    "fieldSelections": [
      {
        "field": "programmer",
        "value": "WIM",
        "operator": "EQ"
      },
      {
        "field": "fixStatus",
        "value": "*CMP",
        "operator": "NE"
      },
      {
        "field": "applicationCode",
        "value": "KE170",
        "operator": "EQ"
      }
    ]
  },
  "sortFields": [
    {
      "fieldName": "fixNumber",
      "sortBy": "desc"
    }
  ],
  "returnExtendedMessageText": true
}'
```
</details>

<details><summary>Response</summary>

``` json
{
  "returnStatus": {
    "status": "*NORM",
    "messages": [],
    "fieldMessages": []
  },
  "listStatus": {
    "totalRecords": 8,
    "recordsReturned": 8,
    "pageNumber": 1
  },
  "records": [
    {
      "applicationCode": "KE170",
      "fixNumber": "T0229",
      "shortFixDescription": "Import History for PCR",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0227",
      "shortFixDescription": "Problems with session refresh",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0201",
      "shortFixDescription": "I02985 - Authority check must include supl grps",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0187",
      "shortFixDescription": "Allow for managed deployments",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0155",
      "shortFixDescription": "Refactor OMO001_2 Part 1",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0153",
      "shortFixDescription": "SBSVAROMS Issue",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0014",
      "shortFixDescription": "TimeFlash: OMQRTVTF does not handle dups",
      "programmer": "WIM"
    },
    {
      "applicationCode": "KE170",
      "fixNumber": "T0002",
      "shortFixDescription": "Missing objects after STRFOF",
      "programmer": "WIM"
    }
  ],
  "fileName": "Fix"
}
```
</details>

### [Task](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/TaskAPI.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/TaskAPI.json) for task actions.

</details>

<details><summary>Request</summary>

``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMXSMP/OMQRTA/add' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "interfaceLevel": "V3R0M0",
  "addProcessingOptions": {
    "sendMessageToProgrammer": "1",
    "updateDb": "1",
    "updateBuffer": "1"
  },
  "addData": {
    "applicationCode": "XMP",
    "fixNumber": "*GEN",
    "fixType": "*SAME",
    "release": "*SAME",
    "priorityNumeric": "*SAME",
    "programmer": "*SAME",
    "expectedStartDate": -1,
    "expectedCompletionDate": -1,
    "expectedDevEndDate": -1,
    "expectedNumberOfHours": -1,
    "shortFixDescription": "The task is added.",
    "freeUserSpace": "*SAME",
    "pathCode": "*SAME",
    "longDescription": ""
  }
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "messages": {
    "messageFile": "OMSAPI",
    "messageID": "OMQ2011",
    "messageType": "",
    "messageData": "XMP  XT0588",
    "field": "",
    "messageText": "Action for fix XT0588 in application XMP completed normally"
  },
  "status": "*NORM",
  "taskResponseData": {
    "applicationCode": "XMP",
    "fixNumber": "XT0588",
    "fixType": "C",
    "release": "MS03",
    "developmentExitCount": 0,
    "reasonCode": "",
    "fixStatus": "*NEW",
    "priorityNumeric": "5",
    "programmer": "ALICE",
    "expectedStartDate": 1240917,
    "realizedStartDate": 0,
    "expectedCompletionDate": 1240917,
    "realizedCompletionDate": 0,
    "expectedDevEndDate": 1240917,
    "realizedDevEndDate": 0,
    "expectedNumberOfHours": 0,
    "realizedNumberOfHours": 0,
    "shortFixDescription": "Description of the task.",
    "numberOfRatificationGrps": 0,
    "ratificationCount": 0,
    "rejectedIndicator": "0",
    "freeUserSpace": "",
    "pathCode": "MAIN",
    "commentCount": 0,
    "completedSolutionsCount": 0,
    "activeSolutionCount": 0,
    "linkedItemsCount": 0,
    "linkedRequestsCount": 0,
    "priorityValueDescription": "Normal priority",
    "typeValueDescription": "Change",
    "reasonValueDescription": "",
    "taskURI": "https://octo.remainsoftware.com/org/Remain/?handle=tdoms/tasks/XMP/XT0588&config=Example",
    "activeJobsCount": 0
  },
  "longDescription": ""
}
```
</details>

### [New Object](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/NewObjectAPI.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/NewObjectAPI.json) to create a new object and attach it to a task.

</details>

<details><summary>Request</summary>

``` shell

curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQRNO/add' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "newSourceLibrary": "OMHD_DEV",
  "newObjectType": "*PGM",
  "newDetail": "",
  "environmentCode": "*DEV",
  "newSourceFile": "QCLLESRC",
  "newDescription": "This is my new object",
  "newObjectLibrary": "OMHD_DEV",
  "newObjectTemplate": "Control Language program",
  "newAttribute": "RPGLE",
  "newSourceAttribute": "RPGLE",
  "task": "XT0588",
  "temporaryOrVirtualObject": "0",
  "newObject": "XT0588CL",
  "newSourceMember": "XT0588CL",
  "applicationCode": "XMP"
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "status": "*NORM",
  "message": {
    "messageFile": "OMSAPI",
    "messageId": "OMQ1043",
    "messageData": "XT0588CL    *PGM     XT0588",
    "messageText": "New object XT0588CL, type *PGM added to task XT0588. &N Cause . . . . . :   You have connected object XT0588CL type *PGM to task XT0588. &N Recovery. . . . :   No recovery necessary. This is an informational message."
  }
}
```
</details>

### [Compile Queue](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/compileQueue.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/compileQueue.json) to add an object to the compile queue.

<details><summary>Request. Add all task objects to the compile queue</summary>

``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQRSQ/queue' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "applicationCode": "XMP",
  "taskNumber": "XT0588",
  "processingArray": {
    "clearAll": false
  }
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "messages": {
    "messageFile": "OMSAPI",
    "messageId": "OMQF305",
    "messageType": "",
    "messageData": "XMP  XT0588",
    "field": "",
    "messageText": "All objects added to the build queue for application XMP fix XT0588. Cause . . . . . :   Objects added to the build queue succesfully. &N Recovery. . . . :   No recovery necessary. This is an informational message."
  },
  "status": "*NORM"
}
```
</details>

<details><summary>Request. Release the compile queue</summary>

``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQRSQ/release' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "applicationCode": "XMP",
  "taskNumber": "XT0588",
  "processingArray": {
    "clearAll": false
  }
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "messages": {
    "messageFile": "OMSAPI",
    "messageId": "OMQF303",
    "messageType": "E",
    "messageData": "XMP  XT0588",
    "field": "",
    "messageText": "Compile queue processed for application XMP task XT0588. Cause . . . . . :   The compile queue is processed succesfully. &N Recovery. . . . :   No recovery necessary. This is an informational message."
  },
  "status": "*NORM"
}
```
</details>

<details><summary>Request. Fetch the compile results</summary>

``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMRDBA/get' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "listInput": {
    "pageNumber": 1,
    "returnTotalRecords": true,
    "recordsPerPage": 50,
    "returnFields": [
      "objectCode",
      "objectLibrary",
      "objectType",
      "createStatus",
      "jobNumber",
      "jobName",
      "userIDOrUserClass"
    ],
    "table": "OMXCreateMonitor",
    "fieldSelections": [
      {
        "field": "applicationCode",
        "value": "XMP",
        "operator": "EQ"
      },
      {
        "field": "fixNumber",
        "value": "XT0588",
        "operator": "EQ"
      }
    ]
  },
  "sortFields": [
    {
      "fieldName": "objectCode",
      "sortBy": "asc"
    }
  ],
  "returnExtendedMessageText": true
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "returnStatus": {
    "status": "*NORM",
    "messages": [],
    "fieldMessages": []
  },
  "listStatus": {
    "totalRecords": 1,
    "recordsReturned": 1,
    "pageNumber": 1
  },
  "records": [
    {
      "objectCode": "XT0588CL",
      "objectLibrary": "OMHD_DEV",
      "objectType": "*PGM",
      "createStatus": "*TERM",
      "jobNumber": 989956,
      "jobName": "XT0588CL",
      "userIDOrUserClass": "WIM"
    }
  ],
  "fileName": "CreateMonitor"
}
```
</details>

### [Event File](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/EVFEventFileAPI.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/EVFEventFileAPI.json) to fetch the event file records with the compile errors. The event file raw records can be parsed using the IBM i Event File Parser on NPM.

<details><summary>Request</summary>
	
``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQREV/getRawRecords' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "pageNumber": 1,
  "memberName": "XT0588CL",
  "library": "OMHD_DEV"
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "status": "*NORM",
  "totalNumberOfRecords": 6,
  "data": [
    {
      "record": "TIMESTAMP  0 20250315210435"
    },
    {
      "record": "PROCESSOR  0 000 1"
    },
    {
      "record": "FILEID     0 001 000000 027 OMHD_DEV/QCLLESRC(XT0588CL) 20250315210433 0"
    },
    {
      "record": "ERROR      0 001 1 000002 000002 000 000002 000 CPD0727 S 40 116 Variable &TEST is referred to but not declared."
    },
    {
      "record": "ERROR      0 001 1 000003 000003 000 000003 000 CPD0792 W 10 116 No data areas, variables, or labels used in program."
    },
    {
      "record": "FILEEND    0 001 000000"
    }
  ]
}
```
</details>


### [Transfer](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/transferObjectsAPI.json)

Use [this API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/transferObjectsAPI.json) to push tasks or single objects through the cycle.

<details><summary>Request. Prepare a transfer.</summary>
	
``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45011/OMS/OMQRTO/prepare' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "action": "move",
  "application": "XMP",
  "taskNumber": "XT0588",
  "fromEnvironment": "*DEV",
  "toEnvironment": "*TST",
  "confirmation": true
}'
```
</details>

<details><summary>Response</summary>
	
``` json
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQRTO/prepare' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "action": "move",
  "application": "XMP",
  "taskNumber": "XT0588",
  "fromEnvironment": "*DEV",
  "toEnvironment": "*TST",
  "confirmation": true
}'
```
</details>

<details><summary>Request. Execute the transfer.</summary>
	
``` shell
curl -X 'POST' \
  'https://plato.remainsoftware.com:45311/OMSXMP/OMQRTO/execute' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json' \
  -d '{
  "transferNumber": "OT951200"
}'
```
</details>

<details><summary>Response</summary>
	
``` json
{
  "job": {
    "jobName": "OM050537",
    "userName": "WIM",
    "jobNumber": 990243
  },
  "transferNumber": "OM050537"
}
```
</details>


## The TD/OMS Rest APIs 
* [REST Branch API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/BranchAPI.json)
* [REST Dashboard API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/dashboardAPI.json)
* [REST Database API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/OMRDBA_DataBase_API.json)
* [REST Fill Object File API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/FillObjectFileAPI.json)
* [REST EVF Event File API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/EVFEventFileAPI.json)
* [REST Compile Override API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/CompileOverrideAPI.json)
* [REST Compile Queue API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/compileQueue.json)
* [REST Comment API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/CommentAPI.json)
* [REST Conversion Definition API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/ConversionDefinitionAPI.json)
* [REST Connection List API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/ConnectionListAPI.json)
* [REST Login API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/Login.json)
* [REST New Object API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/NewObjectAPI.json)
* [REST Octo KanBan Provider API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/octo_kanban_provider.json)
* [REST OMS Log Message API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/OMSLogMessageAPI.json)
* [REST Information Service API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/infoService.json)
* [REST Impact Analysis API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/ImpactAnalysis.json)
* [REST Label API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/LabelAPI.json)
* [REST Task API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/TaskAPI.json)
* [REST Remain API Generator API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/apigenerator.json)
* [REST Request API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/RequestAPI.json)
* [REST Retrieve Command Definition API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/RetrieveCommandDefinitionAPI.json)
* [REST Solution API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/SolutionAPI.json)
* [REST Source API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/sourcefileAPI.json)
* [REST Spool File API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/SpoolFileAPI.json)
* [REST Transfer Objects API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/transferObjectsAPI.json)
* [REST User Defined Variable API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/UserDefinedVariableAPI.json)
* [REST User Options API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/UserOptionsAPI.json)
* [REST Version Conflict API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/main/VersionConflictAPI.json)
* [REST Object API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/ObjectAPI.json)
* [REST Managed Deployment API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/ManagedDeploymentAPI.json)
* [REST Program Call API](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/RemainSoftware/tdomsapi/refs/heads/main/programCall.json)
