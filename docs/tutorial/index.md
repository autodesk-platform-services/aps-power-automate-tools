# Tutorial: Build a Custom Connector

This tutorial guides you through the process of building and using a custom connector for Autodesk Platform Services in Microsoft Power Automate.

## Prerequisites

In order to develop or use custom connectors described in this tutorial, you must have the following:

- Access to [Microsoft Power Automate](https://make.powerautomate.com), with sufficient privileges to create custom connectors
- Existing [APS application](https://get-started.aps.autodesk.com/#create-an-account) with [provisioned access to Autodesk Construction Cloud](https://get-started.aps.autodesk.com/#provision-access-in-other-products)

## Sidenote: why not just import OpenAPI specs?

As we go through the process of creating the custom connector, you'll see that Power Automate provides ways to _auto-generate_ connectors from existing definitions, for example, from OpenAPI specs or Postman collections. While we _do_ provide [OpenAPI specs](https://aps.autodesk.com/blog/openapi-specs-are-here) and [Postman collections](https://github.com/autodesk-platform-services/aps-postman-collections) for APS, the automated connector generation has various limitations, for example:

- According to the [custom connectors documentation](https://learn.microsoft.com/en-us/connectors/custom-connectors/define-openapi-definition), when importing OpenAPI specifications, only the _OpenAPI 2.0_ format (formerly known as _Swagger_) is supported; and downgrading OpenAPI 3.0 to OpenAPI 2.0 is not trivial
- According to an [announcement from October 2022](https://learn.microsoft.com/en-us/power-platform-release-plan/2022wave1/power-platform-pro-development/openapi-3-support-custom-connectors), _OpenAPI 3.0_ is also supported
  - Apparently, when importing an OpenAPI 3.0 specification, Power Automate attempts to downgrade it to OpenAPI 2.0
  - Unfortunately we've often seen this process fail or generate invalid output
- When importing Postman collections, some information is changed or lost during the process
  - For example, response fields that should be numbers are turned to strings, causing validation errors during runtime
- There are important [extensions](https://learn.microsoft.com/en-us/connectors/custom-connectors/openapi-extensions) that still need to be added to the specification to make the best use of the Power Automate capabilities

Because of these limitations, this tutorial focuses on creating Power Automate connectors from scratch.