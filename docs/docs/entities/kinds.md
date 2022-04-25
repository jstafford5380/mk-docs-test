---
uid: entity-kinds
title: Specifications
description: Documentation for the various kinds of entities in Curator
author: jestafford
---

# Entity 'Kinds'

Each 'kind' refers to a spec within the [Entity Object](common-objects#entity). The objects documented below are what go into the `spec` section of the entity object and there for each property is prefixed with `spec.` to keep the context clear.

## `Solution`

Generally, solutions describe a logical grouping of components of a particular subsystem. For example, when you have a single service that has been decomposed into separate physical components that handle various parts of the same capability such as an API, Message Consumer, Job and/or shared library. There are many times where the component(s) where created without respect to it being part of a Solution. In this case, you can use the Solution concept to retroactively 'regroup' the components without necessarily relocating them physically.

Solutions are also used to represent systems that we do not own such as FSL, Salesforce Marketing Cloud, Commerce Cloud etc. Doing so allows them to be referenced by other components as having some type of relationship.

### `spec.type`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(monorepo\|polyrepo\|external)`
| Obtained From | Developer

| Value    | Description
|----------|-------------------------------------------------------------------------------------------
|`monorepo`| All components in this solution reside in the same repository.
|`polyrepo`| Components in this solution reside in several locations.
|`external`| This solution represents an external platform that may or may not reside in source control.

### `spec.subComponents`

| Attribute     | Type |
|-              |-
| Type          | `array`
| Constraints   | `Component`
| Obtained From | Developer

A list of `Component` that compose this solution.

#### Solution Spec Example

```yml
type: monorepo
subComponents:
  - # omitted for brevity
```

## `Component`

Components describe a physical deployable artifact such as a service or package/library.

### `spec.urn`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^urn:(?<system>[a-z0-9-]+):(?<subdomain>[a-z][a-z0-9-]+):(?<component>[a-z][a-z0-9-]+)$`
| Obtained From | Architects

### `spec.subdomain`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^[a-z][a-z0-9-]+$`
| Obtained From | Architects

A recognized subdomain aligned to the Business Capability Model

### `spec.owner`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | convention: [ownerReference](../entities/common.md#ownerReference)
| Obtained From | Developer

Designated owner of the component. Can be a group or an individual.

### `spec.lifecycle`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(experimental\|production\|deprecated\|sunset)`
| Obtained From | Developer

Describes where in the the lifecycle the component currently resides.

| Value | Description
|-      |-
| `experimental` | The component has not yet made it to production.
| `production`   | The component is currently in production and must be maintained.
| `deprecated`   | The component is scheduled for sunset or otherwise should not be further developed.
| `sunset`       | The component should no longer exist in production.

### `spec.dependsOn`

A list of [entity reference](xref:common-objects#entityReference) for entities on which this component depends.

| Attribute     | Type |
|-              |-
| Type          | `array`
| Constraints   | convention: [entityReference](xref:common-objects#entityReference)
| Obtained From | Developer

#### Component Spec Example

```yml
urn: urn:petsmart:devops:curator-ui
subdomain: devops
owner: Group:curator-devs
lifecycle: experimental
dependsOn:
  - Component:curator-api
```

## `Resource`

Resources describe the backing infrastructure on which the component depends. This does not include other services/component.

### `spec.type`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(database\|cache\|messagebus\|storage\|datapipeline)`
| Obtained From | Developer

The type of resource.

### `spec.technology`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^[a-z][a-z0-9]+$`
| Obtained From | Developer

Description of the technology. This is a somewhat arbitrary value, but should be made to be consistent. TODO: consider making this configurable?

### `spec.deploymentModel`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(cloud\|datacenter\|saas)`
| Obtained From | Developer

Describes how the resource is deployed.

| Value             | Description
|-                  |-
| `cloudpaas`       | The resource is deployed in the cloud using a PaaS offering.
| `cloudsaas`       | The resource is deployed in the cloud using a self-hosted PaaS solution.
| `cloudcontainer`  | The resource is deployed in the cloud as a container.
| `onprem`          | The resource is deployed on-prem using a self-hosted/traditional solution.
| `onpremcontainer` | The resource is deployed on-prem as a container.
| `saas`            | The resource is provided by an external cloud provider and the true deployment model is unknown.

### `spec.location`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(gcpsaas\|gcpgke\|gcpselfhosted\|azure\|onprem\|saas)`
| Obtained From | Developer, Architects

Describes where the resource is deployed.

| Value   | Description
|-        |-
| `gcp`   | The resource is deployed to Google Cloud Platform
| `azure` | The resource is deployed to Microsoft Azure
| `onprem`| The resource is deployed on-prem
| `saas`  | The resource is provided by an external SaaS provider (e.g. Aiven, Mongo Atlas, CloudAMQP, etc.)

### `spec.notes`

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | None (freeform)
| Obtained From | Developer

Freeform notes about the resource.

#### Resource Example

```yml
type: database
technology: raven-db
deploymentModel: cloudcontainer
location: gcp
notes: Deployed as a container inside of the Curator namespace.
```
