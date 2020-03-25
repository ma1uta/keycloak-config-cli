[![Build Status](https://travis-ci.com/adorsys/keycloak-config-cli.svg?branch=master)](https://travis-ci.com/adorsys/keycloak-config-cli) [![Maintainability](https://api.codeclimate.com/v1/badges/bd89704bfacbe1fcd215/maintainability)](https://codeclimate.com/github/adorsys/keycloak-config-cli/maintainability) [![codecov](https://codecov.io/gh/adorsys/keycloak-config-cli/branch/master/graph/badge.svg)](https://codecov.io/gh/adorsys/keycloak-config-cli)

# keycloak-config-cli

tool to configure keycloak via json file

## Config files

The config files are based on the keycloak export files. You can use them to re-import your settings.
But keep your files as small as possible. Remove all UUIDs and all stuff which is default set by keycloak.

[moped.json](./contrib/example-config/moped.json) is a full working example file you can consider.
Other examples are located in the [test resources](./config-cli/src/test/resources/import-files).

## Supported features

| Feature                                    | Since  | Description                                                                                              |
| ------------------------------------------ | ------ | -------------------------------------------------------------------------------------------------------- |
| Create clients                             | 0.9.0  | Create client configuration while creating or updating realms                                            |
| Update clients                             | 0.9.0  | Update client configuration while updating realms                                                        |
| Add roles                                  | 0.9.0  | Add roles while creating or updating realms                                                              |
| Update roles                               | 0.9.0  | Update role properties while updating realms                                                             |
| Add composites to roles                    | 0.12.0 | Add role with realm-level and client-level composite roles while creating or updating realms             |
| Add composites to roles                    | 0.12.0 | Add realm-level and client-level composite roles to existing role while creating or updating realms      |
| Remove composites from roles               | 0.12.0 | Remove realm-level and client-level composite roles from existing role while creating or updating realms |
| Add users                                  | 0.9.0  | Add users (inclusive password!) while creating or updating realms                                        |
| Add users with roles                       | 0.9.0  | Add users with realm-level and client-level roles while creating or updating realms                      |
| Update users                               | 0.9.0  | Update user properties (inclusive password!) while updating realms                                       |
| Add role to user                           | 0.9.0  | Add realm-level and client-level roles to user while updating realm                                      |
| Remove role from user                      | 0.9.0  | Remove realm-level or client-level roles from user while updating realm                                  |
| Add authentication flows and executions    | 0.9.0  | Add authentication flows and executions while creating or updating realms                                |
| Update authentication flows and executions | 0.9.0  | Update authentication flow properties and executions while updating realms                               |
| Add components                             | 0.9.0  | Add components while creating or updating realms                                                         |
| Update components                          | 0.9.0  | Update components properties while updating realms                                                       |
| Update sub-components                      | 0.9.0  | Add sub-components properties while creating or updating realms                                          |
| Add groups                                 | 0.12.0 | Add groups (inclusive subgroups!) to realm while creating or updating realms                             |
| Update groups                              | 0.12.0 | Update existing group properties and attributes while creating or updating realms                        |
| Remove groups                              | 0.12.0 | Remove existing groups while updating realms                                                             |
| Add/Remove group attributes                | 0.12.0 | Add or remove group attributes in existing groups while updating realms                                  |
| Add/Remove group roles                     | 0.12.0 | Add or remove roles to/from existing groups while updating realms                                        |
| Update/Remove subgroups                    | 0.12.0 | Like groups, subgroups may also be added/updated and removed while updating realms                       |
| Add scope-mappings                         | 0.9.0  | Add scope-mappings while creating or updating realms                                                     |
| Add roles to scope-mappings                | 0.9.0  | Add roles to existing scope-mappings while updating realms                                               |
| Remove roles from scope-mappings           | 0.9.0  | Remove roles from existing scope-mappings while updating realms                                          |
| Add required-actions                       | 0.9.0  | Add required-actions while creating or updating realms                                                   |
| Update required-actions                    | 0.9.0  | Update properties of existing required-actions while updating realms                                     |
| Update identity providers                  | 1.2.0  | Update properties of existing identity providers while updating realms                                   |

## Compatibility matrix

| keycloak-tools | **Keycloak 4.x** | **Keycloak 6.x** | **Keycloak 7.x** | **Keycloak 8.x** | **Keycloak 9.x** |
| -------------- | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: |
| **v0.4.x**     |        ✓         |        ✗         |        ✗         |        ✗         |        ✗         |
| **v0.5.x**     |        ✓         |        ✗         |        ✗         |        ✗         |        ✗         |
| **v0.6.x**     |        ✓         |        ✓         |        ✗         |        ✗         |        ✗         |
| **v0.7.x**     |        ✓         |        ✓         |        ✓         |        ✗         |        ✗         |
| **v0.8.x**     |        ✓         |        ✓         |        ✓         |        ✗         |        ✗         |
| **v1.0.x**     |        ✗         |        ✗         |        ✗         |        ✓         |        ✓         |
| **v1.1.x**     |        ✗         |        ✗         |        ✗         |        ✓         |        ✓         |
| **v1.2.x**     |        ✗         |        ✗         |        ✗         |        ✓         |        ✓         |
| **master**     |        ✗         |        ✗         |        ✗         |        ✓         |        ✓         |

- `✓` Supported
- `✗` Not supported

## Build this project

```bash
$ mvn package
```

## Run integration tests against real keycloak

We are using [TestContainers](https://www.testcontainers.org/) in our integration tests.
To run the integration tests a configured docker environment is required.

```bash
$ mvn verify
```

## Run this project

### via Maven

Start a local keycloak on port 8080:

```bash
$ docker-compose down --remove-orphans && docker-compose up keycloak
```

before performing following command:

```bash
$ java -jar ./target/config-cli.jar \
    --keycloak.url=http://localhost:8080 \
    --keycloak.sslVerify=true \
    --keycloak.user=admin \
    --keycloak.password=admin123 \
    --import.file=./contrib/example-config/moped.json
```

### Docker

#### Docker run

```bash
$ docker run \
    -e KEYCLOAK_URL=http://<your keycloak host>:8080 \
    -e KEYCLOAK_ADMIN=<keycloak admin username> \
    -e KEYCLOAK_ADMIN_PASSWORD=<keycloak admin password> \
    -e WAIT_TIME_IN_SECONDS=120 \
    -e IMPORT_FORCE=false \
    -v <your config path>:/config \
    adorsys/keycloak-config-cli:latest
```

## Perform release

```bash
mvn -Dresume=false -DdryRun=true release:prepare
mvn -Dresume=false release:prepare
```
