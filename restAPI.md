cf. https://github.com/kellerj/command-line-time-tracker
cf. https://docs.feathersjs.com/guides/basics/rest.html
cf. https://docs.feathersjs.com/api/client/rest.html#http-api

### REST-API (from WebUI to NodeJS-server)

#### Operations

#### Add project-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY                     | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/projects   | POST        |                | c.f. projects-collection-docu |  /MongoDb/projects - document is created                |                        |

#### Retrieve project-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY                     | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:------------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/projects   | GET         |                | c.f. projects-collection-docu |  /MongoDb/projects is read                              |                        |

#### Add task-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY                          | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/tasks      | POST        |                | c.f. tasks-collection-docu         |  /MongoDb/tasks - document is created                   |                        |

#### Retrieve task-name

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY                  | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:---------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/tasks      | GET         |                | c.f. tasks-collection-docu |  /MongoDb/tasks - document is read                      |                        |

#### Add a commit to a project

| REST-API-suffix    | HTTP-Method | URL-parameters | HTTP-BODY                          | Description                                             | Resolves               |
|:-------------------|:------------|:---------------|:-----------------------------------|:--------------------------------------------------------|:-----------------------|
| /NodeJS/timeRecords| POST        |                | c.f. timeRecords-collection-docu   |  /MongoDb/timeRecords create                            |                        |
