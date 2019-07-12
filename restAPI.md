cf. https://github.com/kellerj/command-line-time-tracker
cf. https://docs.feathersjs.com/guides/basics/rest.html
cf. https://docs.feathersjs.com/api/client/rest.html#http-api

### REST-API (from WebUI to NodeJS-server)

#### Operations

##### Start new timeEntry for a specific task(Id)

In order to "start" a new timeEntry-document (/MongoDB/timeEntries) for a specific task, it is necessary to create a timeEntries document with:

* startTime := new Date()
* endTime := null
* duration := null
* description := HTTP-BODY.description
* timeEntryId := uuid.v4()

Furthermore, it is necessary to patch an existing tasks-document (/MongoDB/tasks) with the timeEntryId of the timeEntry-document (see above):

* _timeEntryIds.push(timeEntryId)

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeEntries| POST        | typeId, taskId | description            | /MongoDb/timeEntries create + /MongoDb/tasks patch      | timeEntryId            |

#### Stop new timeEntry for a specific timeEntry(Id)

In order to "stop" a timeEntry-document, it is necessary to patch the timeEntry-document with:

* endTime := new Date()
* duration := endTime - startTime

Furthermore it is necessary to patch a tasks-document:

* duration := duration + timeEntryDocument.duration

| REST-API-suffix     | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:--------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeEntries | PATCH       | timeEntryId    |                        | /MongoDb/timeEntries patch + /MongoDB/tasks patch       | duration               |




<!--| projectAdd         | Add a new project in a specific time entry                                             |
| taskAdd            | Add a new task in a specific project                                                   |
| timeEntryDelete    | Delete a previously entered time entry                                                 |
| projectDelete      |                                                                                        |
| taskDelete         |                                                                                        |
| start              | Start (or re-start) the time tracking for a specific time entry of a project to a task |
| stop               | Stop (or re-stop) the time tracking for a specific time entry of a project to a task   |-->
