---
uid: common-objects
title: Common Objects
description: Documentation around common objects used within entities.
author: jestafford
---

# Common Objects and Conventions

## <a name="entity"/> Entity

Envelope for various entity kinds.

### `apiVersion`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^curator\/v[0-9]+$`
| Obtained From | Development Platform Team

Version of the entity object and enclosed specs.

### `kind`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | `oneOf(Solution\|Component\|Resource)`
| Obtained From | Development Platform Team

Describes the 'kind' contained within the `spec`.

| Value | Description
|-      |-
| `Solution`  | Logical grouping of components and/or resource that make up a single solution or capability.
| `Component` | A physically deployable and maintained entity within the system such as a service (e.g. API, worker, job, pipeline, etc.) or package (e.g. NuGet, Maven, NPM, etc.).
| `Resource`  | An infrastructure dependency for a component such as a database, cache, service bus, etc.

### `metadata`*

| Attribute     | Type |
|-              |-
| Type          | [Metadata](#Metadata)
| Constraints   | [Metadata](#Metadata)
| Obtained From | Enterprise Architecture (EA)

### `spec`*

| Attribute     | Type |
|-              |-
| Type          | see: [Entity Types](kinds.md)
| Constraints   | Dictated by `kind`
| Obtained From | Curator Devs

#### Example

```yml
apiVersion: curator/v1
kind: Component
metadata:
  # omitted for brevity
spec: 
  # omitted for brevity
```

## <a name="Metadata" /> Metadata

Group of descriptive data about the entity.

### `metadata.uniqueName`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^[a-z][a-z0-9-]$`
| Obtained From | Developer

Used as an ID for the entity.

### `metadata.displayName`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | None (freeform)
| Obtained From | Developer

Will be used as a display value for the entity where needed.

### `metadata.description`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | None (freeform)
| Obtained From | Developer

Gives more context and explanation for the entity.

### `metadata.annotations`

| Attribute     | Type |
|-              |-
| Type          | `dictionary`
| Constraints   | `dictionary(string, string)`
| Obtained From | Various

Used to assign strongly typed labels to the entity which have meaning.

### `metadata.links`

| Attribute     | Type |
|-              |-
| Type          | `array`
| Constraints   | [Link](xref:common-objects#Link)
| Obtained From | Developer

List of [Link](#Link) used to provide additional references for the entity.

### `metadata.tags`

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^[a-z-]+$`
| Obtained From | Arbitrary

List of arbitrary values to attach to the entity. Useful for making the entity searchable/groupable.

#### Example

```yml
uniqueName: curator-ui
displayName: Curator UI
description: The user interface for Curator
annotations:
  github/project-slug: "Development/petm-devops-curator"
  gcp/project-slug: curator
links:
  - # omitted for brevity (see below)
tags:
  - devops
  - tools
```

## <a name="Link" /> Link

### `link.url`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | URL ([rfc1738](https://www.ietf.org/rfc/rfc1738.txt))
| Obtained From | Arbitrary

The link target.

### `link.text`*

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | None (freeform)
| Obtained From | Arbitrary

The text that will be used for the link.

### `link.icon`

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^[a-z][a-z0-9-_]+$`
| Obtained From | Curator Devs

Name of the icon that can be used with the link.

#### Example

```yml
url: https://https://confluence.ssg.petsmart.com/display/TEC/Curator
text: Curator Documentation
icon: document
```

## Conventions

### <a name="entityReference" />`entityReference`

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^(?<kind>(Solution\|Component\|Resource))+:(?<uniquename>[a-z][a-z0-9-]+)+$`
| Examples      | `Component:curator-ui`<br /> `Resource:curator-db`

A concatenation of the Entity 'kind' and the entities unique name.

### <a name="ownerReference" /> `ownerReference`

| Attribute     | Type |
|-              |-
| Type          | `string`
| Constraints   | regex: `^(?<ownertype>(Group\|User)):(?<owner>[a-zA-Z-@._]+)$`
| Examples      | `Group:pet-services-devs`<br /> `User:jestafford@petsmart.com`

A concatenation of the owner type and the owner name or email address.