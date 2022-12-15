# Welcome to the CAP SFLIGHT App [Personal Edition v.1 realease 1.0.4->yyy]

This is a sample app for the travel reference scenario, built with the [SAP Cloud Application Programming Model (CAP)](https://cap.cloud.sap) and [SAP Fiori Elements](https://experience.sap.com/fiori-design-web/smart-templates).

The purpose of this sample app is to:
* Demonstrate SAP Fiori annotations
* Demonstrate and compare SAP Fiori features on various stacks (CAP Node.js, CAP Java SDK, ABAP)
* Run UI test suites on various stacks

![Process Travels Page](img.png)

The app still contains some workarounds that are going to be addressed over time.
In some cases, the model and the handlers can be improved or simplified once further planned CAP features become available.
In other cases, the app itself could be improved. For example, calculation of the total price for a travel
currently simply sums up the single prices ignoring the currencies.

> For enabling all features of the Analytical List Page (ALP) in the Node.js runtime, we have switched on the new OData parser 
(`odata_new_parser: true` in `package.json`), which is still in an **experimental state**.
Early adopters may use this feature in own projects on their own risk.
You can also use the ALP with the standard OData parser, but then some features like grouping in the table are not available.

![](https://github.com/SAP-samples/cap-sflight/workflows/CI/badge.svg)
[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/cap-sflight)](https://api.reuse.software/info/github.com/SAP-samples/cap-sflight)


## SAP Fiori UI with Node.js Backend

### Build and Run

In the root folder of your project run
```
npm ci
cds watch
```

### Accessing the SAP Fiori Apps

Open these link in your browser:
* http://localhost:4004/travel_processor/webapp/index.html for processing the travel data
* http://localhost:4004/travel_analytics/webapp/index.html for an [Analytical List Page](https://ui5.sap.com/#/topic/3d33684b08ca4490b26a844b6ce19b83) (ALP)

### Integration Tests

To start OPA tests, open this link in your browser:
http://localhost:4004/travel_processor/webapp/test/integration/Opa.qunit.html

Test documentation is available at:
https://ui5.sap.com/#/api/sap.fe.test

## SAP Fiori UI with Java Backend

### Build and Run

In the root folder of your project run
```
npm ci
mvn spring-boot:run
```

To start the middleware, in another console execute
```
cd app\travel_processor
npm start
```
To run the Analytical List page, use `cd app/travel_analytics` instead.


### Accessing the SAP Fiori App

Open this link in your browser:
http://localhost:8080/index.html

Note: If you access the CAP Java server directly without middleware, you will have to enter mock user credentials admin / admin which are maintained in file application.yml.


### Integration Tests

To start OPA tests, open this link in your browser:
http://localhost:8080/test/integration/Opa.qunit.html

Test documentation is available at:
https://ui5.sap.com/#/api/sap.fe.test

## Deployment to SAP Business Technology Platform

The project contains a configuration for deploying the CAP services and the SAP Fiori app to the SAP Business Technology Platform (SAP BTP) using a managed application router. The app then becomes visible in the content manager of the SAP Launchpad service.

The configuration file `mta.yaml` is for the Node.js backend of the app. If you want to deploy the Java backend, copy `mta-java.yaml` to `mta.yaml`.

### Prerequisites
#### SAP Business Technology Platform

- Create a [trial account on SAP BTP](https://www.sap.com/products/business-technology-platform/trial.html). See this [tutorial](https://developers.sap.com/tutorials/hcp-create-trial-account.html) for more information. Alternatively, you can use a sub-account in a productive environment.
- Subscribe to the [SAP Launchpad Service](https://developers.sap.com/tutorials/cp-portal-cloud-foundry-getting-started.html).
- Create an [SAP HANA Cloud Service instance](https://developers.sap.com/tutorials/btp-app-hana-cloud-setup.html#08480ec0-ac70-4d47-a759-dc5cb0eb1d58) or use an existing one.

#### Local Machine

- Install the Cloud Foundry command line interface (CLI). See this [tutorial](https://developers.sap.com/tutorials/cp-cf-download-cli.html) for more details.
- Install the [MultiApps CF CLI Plugin](https://github.com/cloudfoundry-incubator/multiapps-cli-plugin):

  ```shell
  cf add-plugin-repo CF-Community https://plugins.cloudfoundry.org
  cf install-plugin multiapps
  ```

- Install the [Cloud MTA Build Tool](https://github.com/SAP/cloud-mta-build-tool) globally:

  ```shell
  npm install -g mbt
   ```

### Build the Project

Build the project from the command line:

```shell
mbt build
```

The build results will be stored in the directory `mta_archives`.

### Deploy

1. Log in to the target space.
2. Deploy the MTA archive using the CF CLI: `cf deploy mta_archives/capire.sflight_1.0.0.mtar`

### Assign Role Collection

Any authorized user has read access to the app. For further authorization, assign a role collection to your user in the SAP BTP Cockpit:
* `sflight-reviewer-{spacename}` for executing actions *Accept Travel*, *Reject Travel*, and *Deduct Discount*
* `sflight-processor-{spacename}` for full write access

### Local Development with a HANA Cloud Instance

You need to have access to a HANA Cloud instance and SAP BTP.

1. Deploy the HDI content to a HANA HDI container (which is newly created on first call): `cds deploy --to hana`.
2. Start the application with the Spring Profile `cloud`.
   1. From Maven: `mvn spring-boot:run -Dspring-boot.run.profiles=cloud`
   2. From your IDE with the JVM argument `-Dspring.profiles.active=cloud` or env variable `spring.profiles.active=cloud`

The running application is now connected to its own HDI container/schema. Please keep in mind that the credentials for
that HDI container are stored locally on your filesystem (default-env.json).

## Deployment to SAP Business Technology Platform - Kyma Runtime

The deployment to Kyma Runtime is explained in file [README-Kyma-Runtime.md](./README-Kyma-Runtime.md).

## Creating an SAP Fiori App from Scratch

If you want to implement an SAP Fiori app, follow these tutorials:

* [Create a List Report Object Page App with SAP Fiori Tools](https://developers.sap.com/group.fiori-tools-lrop.html)
* [Developing SAP Fiori applications with SAP Fiori Tools](https://help.sap.com/viewer/17d50220bcd848aa854c9c182d65b699/Latest/en-US)

## Get Support

In case you've a question, find a bug, or otherwise need support, use the [SAP Community](https://answers.sap.com/tags/9f13aee1-834c-4105-8e43-ee442775e5ce) to get more visibility.

## License

Copyright (c) 2022 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
