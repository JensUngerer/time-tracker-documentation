cf. https://github.com/kellerj/command-line-time-tracker
cf. https://docs.feathersjs.com/guides/basics/rest.html
cf. https://docs.feathersjs.com/api/client/rest.html#http-api

### REST-API (from WebUI to NodeJS-server)

#### Operations

#### Add a commit to a project (with the task(s) as comment)

* durationDataStructure := body.durationDataStructure
* dateDataStructure: body.dateDataStructure
* descriptionArr := body.descriptionArray
TODO: really necessary ?
* taskIds? := body.taskIds

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY               | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeRecords| POST        |                | durDs, dateDs, descrArr |  /MongoDb/timeRecords create                            |                        |

### TODO: read and visualize the db-entries regarding timeRecords

#### Add project-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY               | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/projects   | POST        |                |                         |  /MongoDb/projects - document is created                |                        |

#### Retrieve project-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY               | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/projects   | GET         |                |                         |  /MongoDb/projects is read                              |                        |

#### Add task-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY               | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/tasks      | POST        |                |                         |  /MongoDb/tasks - document is created                   |                        |

#### Retrieve task-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY               | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/tasks      | GET         |                |                         |  /MongoDb/tasks - document is read                      |                        |

#### Begin Comment

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
* pauses := new Array()

Furthermore, it is necessary to patch an existing tasks-document (/MongoDB/tasks) with the timeEntryId of the timeEntry-document (see above):

* _timeEntryIds.push(timeEntryId)

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeEntries| POST        | typeId, taskId | description            | /MongoDb/timeEntries create + /MongoDb/tasks patch      | timeEntryId            |

#### Stop new timeEntry for a specific timeEntry(Id)

In order to "stop" a timeEntry-document, it is necessary to patch the timeEntry-document with:

* endTime := new Date()
* duration := endTime - startTime

Additionally, the pauseDurationSum must be calculated by entries of the

* pauses - Array

Furthermore it is necessary to patch a tasks-document:

* duration := duration + timeEntryDocument.duration - pauseDurationSum

| REST-API-suffix     | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:--------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeEntries | PATCH       | timeEntryId    |                        | /MongoDb/timeEntries patch + /MongoDB/tasks patch       | duration               |

#### Toggle Pause

| REST-API-suffix     | HTTP-Method | URL-parameters | HTTP-BODY              | Description                                             | Resolves               |
|:--------------------|:------------|:---------------|:-----------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/togglePause | PATCH       | timeEntryId    |                        | /MongoDb/timeEntries patch                              | pauseDuration          |

#### End Comment
