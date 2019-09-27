# 1crmOpenApi
OpenApi 2.0 (Swagger) definitions for 1CRM API

OpenApi is an API definition format, that can be consumed by a multitude of Tools, to automatically generate API clients.

1CRM offers a REST API (in Professional and Enterprise Edition).

This project was started in order to add 1CRM connectors to Microsoft Flow, therefore OpenAPI 2.0 is used and not the newer version.

## Usage
it's possible to directly use the openAPI files from the openApiFiles subdirectory to inspect them via https://editor.swagger.io - here you can also create client examples for a multitude of languages and platforms, including PHP, Angular and others.

In Microsoft Flow, you can create a custom connector from openAPI files. See https://docs.microsoft.com/en-us/connectors/custom-connectors/define-openapi-definition (section #2 referencing Microsoft Flow and PowerApps)

## Functionality
### Endpoints
#### Data
currently every openAPI contains definitions for a list of multiple objects and create, read, update and delete for single objects from 1CRM
#### Triggers
additionaly there is a webhook endpoint. With this Endpoint, Microsoft Flow can automatically create a webhook in 1CRM, that sends a notification to Flow whenever an object, for example an Account is updated or created. These steps are called Triggers in Flow.

Unfortunately the filters for webhooks are a bit tricky at the moment; The webhook will not trigger without or with a wrong filter. In the openAPI file, you can find a default, working filter - but unfortunately Flow does not use the defaults, as soon, as the filters are editable. We decided to make the filters editable, therefore you'll have to fill in the defaults under advanced settings of the trigger:
```
glue: or
conditions field -1:  deleted
conditions filter -1: is_false
conditions value -1:  0
conditions when -1:   current_value
```
additional information about the filters can be found on https://1crm.com/docs/1CRM_8.6_Developer_Guide.pdf page 24ff

### Known Issues
- the default filters for Triggers
- complex objects as query parameters cannot defined in openAPI 2.0. 1CRM expects an array for the fields parameter (`?fields[]=last_name&fields[]=first_name`) and an object for filters in data queries (`filters[first_name]=John&filters[last_name]=Smith`)

## Advanced Usage
With the [Microsoft Power Platform Connectors CLI](https://github.com/visual4/1crmOpenApi/tree/master/powerPlatformConnectors/1crm-meta-connector), you can manipulate download and update your connectors from CLI. We have used this to create the 1CRM Meta connector, that currently can fetch metadata information from 1CRM API. The connector is located in [powerPlatformConnectors/1crm-meta-connector](https://github.com/visual4/1crmOpenApi/tree/master/powerPlatformConnectors/1crm-meta-connector).

Based on this connector we have built a flow, that extracts module metadata from a given 1CRM URL (we use demo.1crm.de by default) and builds the openAPI files you can find in the openAPI directory. If you have a lot of customisation with custom fields and custom modules, you can use this flow to create openAPI files that are specific to your 1CRM instance. See [logicAppTemplates](https://github.com/visual4/1crmOpenApi/tree/master/logicAppTemplates) folder.

## What to use all this for
- ERP synchronisation: when an account is updated in 1CRM update ERP entity and vice versa
- Ticketing: bidirectional case synchronisation between 1CRM and Jira
- create Sharepoint lists from 1CRM objects
- create or update 1crm objects from Excel files
- send a mail if a case is updated
- connect Hubspot, Trello, Mailchimp to 1CRM; see https://emea.flow.microsoft.com/de-de/connectors/ for full list of connectors

## TODOs
- get quotes, salesorders and invoices working (there is an extra endpoint, that returns the object including all line items)
- build some example flows
- extend the metadata connector, to get filters, relationships, available models, add translated dropdown values to field definition...
- build a combined connector and submit this one to MS for certification
