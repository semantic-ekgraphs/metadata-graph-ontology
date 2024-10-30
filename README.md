# Metadata Graph Ontology Documentation

Welcome to the **Metadata Graph Ontology** documentation! This repository provides a comprehensive guide to the ontology designed to facilitate the creation of **Enterprise Knowledge Graphs (EKGs)**. By leveraging this vocabulary, you can model and integrate metadata from various data sources into a unified semantic framework, effectively capturing every step involved in constructing an EKG.

---

<p align="center">
  <img src="images/metadata-graph-ontology-banner.png" alt="Metadata Graph Ontology" width="600">
</p>

---

## Table of Contents

- [Introduction](#introduction)
- [Understanding EKGs](#understanding-ekgs)
- [Incremental Construction of the META-EKG](#incremental-construction-of-the-meta-ekg)
  - [Step 1: Knowledge Acquisition](#step-1-knowledge-acquisition)
  - [Step 2: Construction of Semantic Views](#step-2-construction-of-semantic-views)
  - [Step 3: Construction of Linkset Views](#step-3-construction-of-linkset-views)
  - [Step 4: Construction of Fusion Views](#step-4-construction-of-fusion-views)
  - [Additional Step: Unification View Construction](#additional-step-unification-view-construction)
- [Metadata Graph Ontology Overview](#metadata-graph-ontology-overview)
  - [Classes and Properties Overview](#classes-and-properties-overview)
- [Using SHACL for Active Metadata](#using-shacl-for-active-metadata)
  - [SHACL Example: Enforcing Data Quality Constraints](#shacl-example-enforcing-data-quality-constraints)
- [Use Cases and Examples](#use-cases-and-examples)
  - [Case Study: Building an EKG for a Retail Company](#case-study-building-an-ekg-for-a-retail-company)
- [Contributing](#contributing)
- [License](#license)
- [Additional Resources](#additional-resources)

## Introduction

The **Metadata Graph Ontology** provides a set of classes and properties to model metadata related to data sources, mappings, ontologies, and processes involved in building an EKG. This documentation aims to guide you through the process of using the ontology to create a comprehensive EKG that integrates diverse data sources into a cohesive semantic model.

## Understanding EKGs

An **Enterprise Knowledge Graph** (EKG) is a structured representation of an organization's data, capturing entities, relationships, and metadata in a machine-readable format. EKGs enable organizations to:

- **Integrate data** from heterogeneous sources.
- **Enhance data interoperability** and reusability.
- **Facilitate advanced analytics** and insights.
- **Support decision-making** processes.

By utilizing the Metadata Graph Ontology, organizations can define standard vocabularies and schemas to represent their data consistently across different domains, while also capturing the metadata of each construction step.

<p align="center">
  <img src="images/ekg-overview.png" alt="Enterprise Knowledge Graph Overview" width="700">
</p>

## Incremental Construction of the META-EKG

The proposal for the incremental construction of the **META-EKG** comprises four primary steps (with an additional unification step), where for each step carried out in the construction of the EKG, the Metadata Graph Ontology is used to represent the metadata.

### Step 1: Knowledge Acquisition

**Objective:** Collect and describe metadata about data sources and their provenance.

- **Data Source Metadata:** Information about the data source itself (e.g., type, format, access details).
- **Data Source Provenance Metadata:** Details about the origin, creator, and history of the data source.

#### Relevant Classes and Properties

| Class                      | Description                                            |
|----------------------------|--------------------------------------------------------|
| `drm:DataAsset`            | Represents a data source or asset.                     |
| `vekg:DataSourceProvenance`| Captures provenance metadata of a data source.         |

| Property               | Domain                | Range                  | Description                                             |
|------------------------|-----------------------|------------------------|---------------------------------------------------------|
| `vekg:numberNullableFields` | `dcat:Dataset`    | `xsd:integer`| Informs the number of nullable fields in a data source.              |
| `vekg:numberUniqueFields`   | `dcat:Dataset`    | `xsd:integer`| Informs the number of unique fields in a data source.                |
| `vekg:numberIncrementalFields` | `dcat:Dataset` | `xsd:integer`| Informs the number of incremental fields in a data source.           |
| `vekg:numberAggregateFields` | `dcat:Dataset`   | `xsd:integer`| Informs the number of aggregated fields in a data source.            |
| `vekg:nullableFields`       | `dcat:Dataset`    | `xsd:string` | Lists the nullable fields in a data source.                          |
| `vekg:uniqueFields`         | `dcat:Dataset`    | `xsd:string` | Lists the unique fields in a data source.                            |
| `vekg:aggregateFields`      | `dcat:Dataset`    | `xsd:string` | Lists the aggregated fields in a data source.                        |
| `vekg:incrementalFields`    | `dcat:Dataset`    | `xsd:string` | Lists the incremental fields in a data source.                       |
| `vekg:recordCount`          | `dcat:Dataset`    | `xsd:integer`| Informs the total number of records in a data source.                |
| `vekg:recordCountTime`      | `dcat:Dataset`    | `xsd:dateTime`| Indicates the time when the record count was taken.                  |
| `vekg:totalNumberPerField`  | `xsd:string`      | `xsd:integer`| Informs the total number of values per field.                        |
| `vekg:hasProvenance`   | `drm:DataAsset`       | `vekg:DataSourceProvenance` | Links a data source to its provenance metadata.         |
| `pav:createdBy`        | `vekg:DataSourceProvenance` | `foaf:Person`       | The creator of the data source.                         |
| `pav:createdOn`        | `vekg:DataSourceProvenance` | `xsd:dateTime`     | The date the data source was created.                   |
| `pav:importedFrom`     | `vekg:DataSourceProvenance` | `rdfs:Resource`    | The source from which the data was imported.            |


#### Example

```turtle
:DataSource1 rdf:type drm:DataAsset ;
    rdfs:label "Customer Database"@en ;
    dcterms:description "A relational database containing customer records."@en ;
    vekg:nullableFields "age","phone";
    vekg:numberNullableFields 3;
    vekg:uniqueFields "id","registry";
    vekg:numberUniqueFields 2;
    vekg:incrementalFields "id";
    vekg:numberIncrementalFields 1;
    vekg:transformedFields "name", "phone";
    vekg:numberTransformedFields 2;
    vekg:hasProvenance :DataSource1Provenance .

:DataSource1Provenance rdf:type vekg:DataSourceProvenance ;
    pav:createdBy :DataEngineer1 ;
    pav:createdOn "2023-10-01"^^xsd:date ;
    pav:importedFrom "LegacyCRMSystem" ;
    pav:sourceLastAccessedOn "2023-10-15T10:00:00Z"^^xsd:dateTime .
```

### Step 2: Construction of Semantic Views

**Objective:** Create a global semantic view by constructing exported views for each data source, comprising an ontology, mappings, and metadata.

- **Semantic View:** Represents the unified schema and vocabulary for the EKG.
- **Exported Semantic View:** Contains the ontology, mappings, and data source metadata for a specific data source.

#### Relevant Classes and Properties

| Class                            | Description                                               |
|----------------------------------|-----------------------------------------------------------|
| `vekg:SemanticView`              | Represents the global semantic perspective of the EKG.    |
| `vekg:ExportedSemanticView`      | Represents the semantic view specific to a data source.   |
| `vekg:SemanticViewOntology`      | Represents the domain ontology used in the Semantic View. |
| `vekg:LocalOntology`             | Represents the local ontology of a data source.           |
| `vekg:Mappings`                  | Represents mappings from data source to ontology terms.   |

| Property                      | Domain                   | Range                        | Description                                                |
|-------------------------------|--------------------------|------------------------------|------------------------------------------------------------|
| `vekg:hasSemanticViewOntology`| `vekg:SemanticView`      | `vekg:SemanticViewOntology`  | Associates a Semantic View with its Domain Ontology.       |
| `vekg:hasExportedSemanticView`| `vekg:SemanticView`      | `vekg:ExportedSemanticView`  | Associates a Semantic View with Exported Semantic Views.   |
| `vekg:hasLocalOntology`       | `vekg:ExportedSemanticView` | `vekg:LocalOntology`      | Links an Exported Semantic View to its Local Ontology.     |
| `vekg:hasMappings`            | `vekg:ExportedSemanticView` | `vekg:Mappings`          | Connects an Exported Semantic View with its Mappings.      |
| `vekg:hasDataSource`          | `vekg:ExportedSemanticView` | `drm:DataAsset`          | Links an Exported Semantic View to its data source.        |

#### Example

```turtle
:SemanticView rdf:type vekg:SemanticView ;
    rdfs:label "Global Semantic View"@en ;
    vekg:hasExportedSemanticView :ExportedView_DS1, :ExportedView_DS2 ;
    vekg:hasSemanticViewOntology :DomainOntology .

:ExportedView_DS1 rdf:type vekg:ExportedSemanticView ;
    rdfs:label "Exported View for DataSource1"@en ;
    vekg:hasLocalOntology :LocalOntology_DS1 ;
    vekg:hasMappings :Mappings_DS1 ;
    vekg:hasDataSource :DataSource1 .
```

### Step 3: Construction of Linkset Views

**Objective:** Specify linksets that define relationships between instances in different exported semantic views, facilitating identity resolution.

- **Linkset View:** Contains metadata about the links (e.g., `owl:sameAs`) between data from different sources.

#### Relevant Classes and Properties

| Class                        | Description                                                    |
|------------------------------|----------------------------------------------------------------|
| `vekg:LinksetView`           | Represents linksets between different datasets.                |
| `vekg:LinkageRule`           | Defines the rules for linking entities across datasets.        |
| `vekg:Compare`               | Represents a comparison function used in linkage rules.        |
| `vekg:MatchClass`            | Represents the class of entities to be matched.                |

| Property                      | Domain                | Range                  | Description                                                  |
|-------------------------------|-----------------------|------------------------|--------------------------------------------------------------|
| `vekg:hasLinkageRule`         | `vekg:LinksetView`    | `vekg:LinkageRule`     | Associates a Linkset View with a linkage rule.               |
| `vekg:hasLinkFunction`        | `vekg:LinkageRule`    | `vekg:Compare`         | Specifies the comparison function in a linkage rule.         |
| `vekg:sourceView`             | `vekg:LinksetView`    | `vekg:ExportedSemanticView` | The source view in the linkset.                        |
| `vekg:targetView`             | `vekg:LinksetView`    | `vekg:ExportedSemanticView` | The target view in the linkset.                        |
| `vekg:hasMatchClass`          | `vekg:LinkageRule`    | `vekg:MatchClass`      | The class of entities to match.                               |

#### Example

```turtle
:LinksetView_DS1_DS2 rdf:type vekg:LinksetView ;
    rdfs:label "Linkset between DataSource1 and DataSource2"@en ;
    vekg:hasLinkageRule :LinkageRule_EmailMatch ;
    vekg:sourceView :ExportedView_DS1 ;
    vekg:targetView :ExportedView_DS2 ;
    void:linkPredicate owl:sameAs .

:LinkageRule_EmailMatch rdf:type vekg:LinkageRule ;
    vekg:hasLinkFunction :EmailMatchFunction ;
    vekg:hasMatchClass :Customer .

:EmailMatchFunction rdf:type vekg:Compare ;
    vekg:operator "equalsIgnoreCase" ;
    vekg:inputSource "email" ;
    vekg:inputTarget "emailAddress" ;
    vekg:threshold 1.0 .
```

### Step 4: Construction of Fusion Views

**Objective:** Define how data from different sources should be fused, resolving conflicts and redundancies to create a unified view.

- **Fusion View:** Contains metadata about how to merge data from different sources.

#### Relevant Classes and Properties

| Class                              | Description                                              |
|------------------------------------|----------------------------------------------------------|
| `vekg:FusionView`                  | Represents the fusion specifications for data merging.   |
| `vekg:PropertyFusionAssertion`     | Specifies how to fuse a particular property.             |
| `vekg:FusionProperty`              | Represents a property involved in data fusion.           |

| Property                          | Domain                    | Range                 | Description                                            |
|-----------------------------------|---------------------------|-----------------------|--------------------------------------------------------|
| `vekg:hasPropertyFusionAssertion` | `vekg:FusionView`         | `vekg:PropertyFusionAssertion` | Links a Fusion View to its property fusion assertions. |
| `vekg:hasFusionProperty`          | `vekg:PropertyFusionAssertion`    | `vekg:FusionProperty`            | Specifies the property to be fused.                     |
| `vekg:fusionFunction`             | `vekg:PropertyFusionAssertion` | `xsd:string`       | Defines the function used to fuse data.                |

#### Example

```turtle
:FusionView_Customers rdf:type vekg:FusionView ;
    rdfs:label "Fusion View for Customer Data"@en ;
    vekg:hasPropertyFusionAssertion :EmailFusionAssertion ;
    vekg:hasGeneralizationClass :Customer .

:EmailFusionAssertion rdf:type vekg:PropertyFusionAssertion ;
    vekg:hasFusionProperty :email ;
    vekg:fusionFunction "mostRecentValue" .

:email rdf:type vekg:FusionProperty ;
    rdfs:label "Email Address"@en ;
    rdfs:domain :Customer ;
    rdfs:range xsd:string .
```

### Additional Step: Unification View Construction

**Objective:** Normalize and standardize data representations across different sources.

- **Unification View:** Contains metadata about the normalization functions applied to data.

#### Relevant Classes and Properties

| Class                      | Description                                                |
|----------------------------|------------------------------------------------------------|
| `vekg:UnificationView`     | Represents unification specifications for data normalization. |

| Property                  | Domain               | Range             | Description                                             |
|---------------------------|----------------------|-------------------|---------------------------------------------------------|
| `vekg:normalizeFunction`  | `vekg:UnificationView` | `xsd:string`    | Specifies the normalization function applied.           |
| `vekg:hasGeneralizationClass` | `vekg:UnificationView` | `rdfs:Class` | Links to the class being unified.                       |

#### Example

```turtle
:UnificationView_Customers rdf:type vekg:UnificationView ;
    rdfs:label "Unification View for Customer Data"@en ;
    vekg:normalizeFunction "standardizeDateFormats" ;
    vekg:hasGeneralizationClass :Customer .
```

## Metadata Graph Ontology Overview

### Classes and Properties Overview

To facilitate understanding, the following tables summarize the key classes and properties used in constructing an EKG.

#### Classes

| Class                           | Description                                               | Used in Step                       |
|---------------------------------|-----------------------------------------------------------|------------------------------------|
| `vekg:MetadataGraph`            | Captures metadata about the EKG construction process.     | All Steps                          |
| `vekg:SemanticView`             | Represents the global semantic perspective of the EKG.    | Step 2                             |
| `vekg:ExportedSemanticView`     | Represents the semantic view specific to a data source.   | Step 2                             |
| `vekg:SemanticViewOntology`     | Represents the domain ontology used in the Semantic View. | Step 2                             |
| `vekg:LocalOntology`            | Represents the local ontology of a data source.           | Step 2                             |
| `vekg:Mappings`                 | Represents mappings from data source to ontology terms.   | Step 2                             |
| `vekg:LinksetView`              | Represents linksets between different datasets.           | Step 3                             |
| `vekg:LinkageRule`              | Defines the rules for linking entities across datasets.   | Step 3                             |
| `vekg:FusionView`               | Contains metadata about data fusion specifications.       | Step 4                             |
| `vekg:PropertyFusionAssertion`  | Specifies how to fuse a particular property.              | Step 4                             |
| `vekg:FusionProperty`           | Represents a property involved in data fusion.            | Step 4                             |
| `vekg:UnificationView`          | Contains metadata about data normalization functions.     | Additional Step                    |

#### Properties

| Property                        | Domain                            | Range                            | Description                                             | Used in Step                       |
|---------------------------------|-----------------------------------|----------------------------------|---------------------------------------------------------|------------------------------------|
| `vekg:hasProvenance`            | `drm:DataAsset`                   | `vekg:DataSourceProvenance`      | Links a data source to its provenance metadata.         | Step 1                             |
| `vekg:hasSemanticViewOntology`  | `vekg:SemanticView`               | `vekg:SemanticViewOntology`      | Associates a Semantic View with its Domain Ontology.    | Step 2                             |
| `vekg:hasExportedSemanticView`  | `vekg:SemanticView`               | `vekg:ExportedSemanticView`      | Associates a Semantic View with Exported Semantic Views.| Step 2                             |
| `vekg:hasLocalOntology`         | `vekg:ExportedSemanticView`       | `vekg:LocalOntology`             | Links an Exported Semantic View to its Local Ontology.  | Step 2                             |
| `vekg:hasMappings`              | `vekg:ExportedSemanticView`       | `vekg:Mappings`                  | Connects an Exported Semantic View with its Mappings.   | Step 2                             |
| `vekg:hasDataSource`            | `vekg:ExportedSemanticView`       | `drm:DataAsset`                  | Links an Exported Semantic View to its data source.     | Step 2                             |
| `vekg:hasLinkageRule`           | `vekg:LinksetView`                | `vekg:LinkageRule`               | Associates a Linkset View with a linkage rule.          | Step 3                             |
| `vekg:hasLinkFunction`          | `vekg:LinkageRule`                | `vekg:Compare`                   | Specifies the comparison function in a linkage rule.    | Step 3                             |
| `vekg:hasPropertyFusionAssertion` | `vekg:FusionView`               | `vekg:PropertyFusionAssertion`   | Links a Fusion View to its property fusion assertions.  | Step 4                             |
| `vekg:hasFusionProperty`        | `vekg:PropertyFusionAssertion`    | `vekg:FusionProperty`            | Specifies the property to be fused.                     | Step 4                             |
| `vekg:normalizeFunction`        | `vekg:UnificationView`            | `xsd:string`                     | Specifies the normalization function applied.           | Additional Step                    |

## Using SHACL for Active Metadata

The Shapes Constraint Language ([SHACL](https://www.w3.org/TR/shacl/)) is used to validate RDF graphs against a set of conditions. SHACL shapes can be used to enforce data quality constraints, making metadata active by ensuring data adheres to specified standards.

### SHACL Example: Enforcing Data Quality Constraints

Suppose we want to enforce that every `Customer` must have a valid email address and that the `email` property must conform to a specific pattern.

#### SHACL Shape

```turtle
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix ex: <http://example.com/ns#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

:CustomerShape a sh:NodeShape ;
    sh:targetClass ex:Customer ;
    sh:property [
        sh:path ex:email ;
        sh:datatype xsd:string ;
        sh:pattern "^[\\w.-]+@[\\w.-]+\\.\\w+$" ;
        sh:minCount 1 ;
    ] ;
    sh:property [
        sh:path ex:phoneNumber ;
        sh:datatype xsd:string ;
        sh:minCount 1 ;
    ] .
```

#### Explanation

- **Target Class:** Applies to all instances of `ex:Customer`.
- **Email Constraint:**
  - Must be a string (`sh:datatype xsd:string`).
  - Must match the email regex pattern (`sh:pattern`).
  - Must have at least one email (`sh:minCount 1`).
- **Phone Number Constraint:**
  - Must be a string.
  - Must have at least one phone number.

#### Applying the SHACL Shape

By applying this shape to your data, you can ensure that all customer data conforms to the specified constraints, thus activating the metadata rules in your data validation process.

#### Validation Example

Suppose we have the following data:

```turtle
:Customer123 rdf:type ex:Customer ;
    ex:email "customer@example.com" ;
    ex:phoneNumber "123-456-7890" .

:Customer456 rdf:type ex:Customer ;
    ex:email "invalid-email" ;
    ex:phoneNumber "098-765-4321" .
```

Running the SHACL validation would flag `:Customer456` because the email does not match the specified pattern.

## Use Cases and Examples

### Case Study: Building an EKG for a Retail Company

*(This case study was detailed earlier in the documentation.)*

---

## Contributing

We welcome contributions to improve the Metadata Graph Ontology and its documentation. Please feel free to submit issues or pull requests.

### How to Contribute

1. **Fork the repository** to your GitHub account.
2. **Create a new branch** for your feature or bugfix.
3. **Commit your changes** with clear and descriptive messages.
4. **Submit a pull request** explaining your changes and their benefits.

## License

This project is licensed under the [MIT License](LICENSE).

---

## Additional Resources

- **Ontology File:** [Download the Metadata Graph Ontology](ontology/metadata-graph.owl)
- **Example Mappings:** [View example mappings](mappings/)
- **Sample Data:** [Access sample data sources](data/)
- **Contact:** For questions or support, please [open an issue](issues/) or contact the maintainers directly.

---

<p align="center">
  <img src="images/thank-you.png" alt="Thank You" width="200">
</p>

We hope this documentation helps you in creating robust and integrated Enterprise Knowledge Graphs using the Metadata Graph Ontology. By capturing detailed metadata at each step and leveraging SHACL for active metadata enforcement, you can ensure transparency, data quality, and maintainability in your EKG projects.

Happy modeling!

---

*This documentation was generated to assist in understanding and utilizing the Metadata Graph Ontology for building Enterprise Knowledge Graphs. For detailed ontology files, additional examples, and support, please refer to the repository files or contact the maintainers.*
