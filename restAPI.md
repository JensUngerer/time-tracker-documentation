cf. https://github.com/kellerj/command-line-time-tracker
cf. https://docs.feathersjs.com/guides/basics/rest.html
cf. https://docs.feathersjs.com/api/client/rest.html#http-api

### REST-API (from WebUI to NodeJS-server)

#### Operations

##### Create a new project

In order to create a new project, two properties (in /MongoDB/projects) need to be created:

* name := HTTP-BODY.name
* projectId := uuid.v4()

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/projects   | POST        |                | name                   | /MongoDb/projects create                                | projectId              |

##### Create a new feature

In order to create a new feature, several properties (in /MongoDB/features) need to be created:

* name := HTTP-BODY.name
* featureId: uuid.v4()
* _taskIds := new Array()
* _projectId := URL-parameter.projectId

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/features   | POST        | projectId      | name                   | /MongoDb/features create                                | featureId              |

##### Create a new task

In order to create a new task, several properties (in /MongoDB/tasks) need to be created:

* taskId := uuid.v4()
* _featureId := URL-paramters.featureId
* duration := 0

Furthermore it is necessary to patch the corresponding feature (via /MongoDB/features):

* _taskIds.push(taskId)

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/tasks      | POST        |  featureId     | name                   | /MongoDB/tasks create + /MongoDB/features patch         | taskId                 |

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
