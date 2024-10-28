# Metadata Graph Ontology Documentation

Welcome to the **Metadata Graph Ontology** documentation! This repository provides a comprehensive guide to the ontology designed to facilitate the creation of **Enterprise Knowledge Graphs (EKGs)**. By leveraging this vocabulary, you can model and integrate metadata from various data sources into a unified semantic framework, effectively capturing every step involved in constructing an EKG.

## Table of Contents

- [Introduction](#introduction)
- [Understanding EKGs](#understanding-ekgs)
- [Incremental Construction of the META-EKG](#incremental-construction-of-the-meta-ekg)
  - [Step 1: Knowledge Acquisition](#step-1-knowledge-acquisition)
  - [Step 2: Construction of Semantic Views](#step-2-construction-of-semantic-views)
  - [Step 3: Construction of Linkset Views](#step-3-construction-of-linkset-views)
  - [Step 4: Construction of Fusion Views](#step-4-construction-of-fusion-views)
- [Metadata Graph Ontology Overview](#metadata-graph-ontology-overview)
  - [Classes](#classes)
  - [Properties](#properties)
- [Usage Examples](#usage-examples)
- [Contributing](#contributing)
- [License](#license)

## Introduction

The **Metadata Graph Ontology** provides a set of classes and properties to model metadata related to data sources, mappings, ontologies, and processes involved in building an EKG. This documentation aims to guide you through the process of using the ontology to create a comprehensive EKG that integrates diverse data sources into a cohesive semantic model.

## Understanding EKGs

An **Enterprise Knowledge Graph** (EKG) is a structured representation of an organization's data, capturing entities, relationships, and metadata in a machine-readable format. EKGs enable organizations to:

- **Integrate data** from heterogeneous sources.
- **Enhance data interoperability** and reusability.
- **Facilitate advanced analytics** and insights.
- **Support decision-making** processes.

By utilizing the Metadata Graph Ontology, organizations can define standard vocabularies and schemas to represent their data consistently across different domains, while also capturing the metadata of each construction step.

## Incremental Construction of the META-EKG

The proposal for the incremental construction of the **META-EKG** comprises five primary steps (with an additional step), where for each step carried out in the construction of the EKG, the Metadata Graph Ontology is used to represent the metadata.

### Step 1: Knowledge Acquisition

**Objective:** Collect and describe metadata about data sources and their provenance.

- **Data Source Metadata:** Information about the data source itself (e.g., type, format, access details).
- **Data Source Provenance Metadata:** Details about the origin, creator, and history of the data source.

**Metadata Graph Ontology Usage:**

- Use `drm:DataAsset` to represent data sources.
- Capture provenance using `vekg:DataSourceProvenance` and properties like `pav:createdBy`, `pav:createdOn`, and `pav:importedFrom`.

**Example:**

```turtle
:DataSource1 rdf:type drm:DataAsset ;
    rdfs:label "Customer Database"@en ;
    dcterms:description "A relational database containing customer records."@en ;
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

**Metadata Graph Ontology Usage:**

- Define `vekg:SemanticView` as the global view.
- Use `vekg:ExportedSemanticView` for each data source.
- Represent ontologies with `vekg:SemanticViewOntology` and `vekg:LocalOntology`.
- Capture mappings using `vekg:Mappings`.

**Example:**

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

**Metadata Graph Ontology Usage:**

- Use `vekg:LinksetView` to represent the linksets.
- Define linkage rules with `vekg:LinkageRule`.
- Capture link functions and match properties.

**Example:**

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

**Metadata Graph Ontology Usage:**

- Use `vekg:FusionView` to represent the fusion specifications.
- Define fusion properties with `vekg:PropertyFusionAssertion` and `vekg:FusionProperty`.
- Specify fusion functions.

**Example:**

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

**Metadata Graph Ontology Usage:**

- Use `vekg:UnificationView` to represent unification specifications.
- Define normalization functions with `vekg:normalizeFunction`.

**Example:**

```turtle
:UnificationView_Customers rdf:type vekg:UnificationView ;
    rdfs:label "Unification View for Customer Data"@en ;
    vekg:normalizeFunction "standardizeDateFormats" ;
    vekg:hasGeneralizationClass :Customer .
```

## Metadata Graph Ontology Overview

### Prefixes

Here are the primary prefixes used in the Metadata Graph Ontology:

```turtle
@prefix vekg: <http://www.arida.ufc.br/VEKG#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix pav: <http://purl.org/pav/> .
@prefix mod: <http://www.isibang.ac.in/ns/mod#> .
@prefix drm: <http://vocab.data.gov/def/drm#> .
@prefix ldp: <http://www.w3.org/ns/ldp#> .
@prefix rr: <http://www.w3.org/ns/r2rml#> .
```

### Classes

Below is a detailed explanation of the key classes in the Metadata Graph Ontology, crucial for building an EKG.

#### 1. **Metadata Graph (`vekg:MetadataGraph`)**

A **Metadata Graph** is a Knowledge Graph that describes the metadata produced during the construction of an EKG.

#### 2. **Semantic View (`vekg:SemanticView`)**

The **Semantic View** represents the global semantic perspective of the EKG, integrating domain ontologies and data source schemas.

#### 3. **Exported Semantic View (`vekg:ExportedSemanticView`)**

An **Exported Semantic View** is a tuple `(OVi, MVi)` defined over the schema of a local data source, containing the local ontology, mappings, and metadata.

#### 4. **Mappings (`vekg:Mappings`)**

Mappings are a set of rules that enable the transformation of a specific data source according to a Local Ontology.

#### 5. **Linkset View (`vekg:LinksetView`)**

A **Linkset View** specifies relationships (`sameAs`, `seeAlso`, etc.) between instances in different exported semantic views, aiding in identity resolution.

#### 6. **Fusion View (`vekg:FusionView`)**

A **Fusion View** contains metadata about how data from different sources should be fused to create a unified representation.

### Properties

Below are key properties used to relate classes within the Metadata Graph Ontology.

#### **Data Source Properties**

- `vekg:hasProvenance` - Links a data source to its provenance metadata.

#### **Semantic View Properties**

- `vekg:hasSemanticViewOntology` - Associates a Semantic View with its Domain Ontology.
- `vekg:hasExportedSemanticView` - Associates a Semantic View with its Exported Semantic Views.

#### **Mappings Properties**

- `vekg:hasMappings` - Connects an Exported Semantic View with its Mappings.

#### **Linkset Properties**

- `vekg:hasLinkageRule` - Associates a Linkset View with a linkage rule.
- `vekg:hasLinkFunction` - Specifies the function used in a linkage rule.

#### **Fusion Properties**

- `vekg:hasPropertyFusionAssertion` - Links a Fusion View to its property fusion assertions.
- `vekg:hasFusionProperty` - Specifies the property to be fused.

## Usage Examples

### Example 1: Defining Data Source Metadata

```turtle
:DataSource2 rdf:type drm:DataAsset ;
    rdfs:label "Sales Records CSV Files"@en ;
    dcterms:description "A collection of CSV files containing sales records."@en ;
    vekg:hasProvenance :DataSource2Provenance .

:DataSource2Provenance rdf:type vekg:DataSourceProvenance ;
    pav:providedBy :ExternalVendor ;
    pav:createdOn "2023-09-15"^^xsd:date ;
    pav:importedOn "2023-10-05"^^xsd:date ;
    pav:sourceLastAccessedOn "2023-10-15T12:00:00Z"^^xsd:dateTime .
```

### Example 2: Creating Mappings

```turtle
:Mappings_DS2 rdf:type vekg:Mappings ;
    rdfs:label "Mappings for DataSource2"@en ;
    vekg:urlMappings "http://example.com/mappings/ds2-mappings.ttl"^^xsd:anyURI ;
    vekg:prefixies "ex: <http://example.com/ns#>" .

# Mapping example using R2RML
@prefix rr: <http://www.w3.org/ns/r2rml#> .

:TriplesMap_DS2 rr:logicalTable [ rr:tableName "sales_records" ] ;
    rr:subjectMap [ rr:template "http://example.com/sales/{order_id}" ;
                    rr:class ex:SalesOrder ] ;
    rr:predicateObjectMap [
        rr:predicate ex:orderDate ;
        rr:objectMap [ rr:column "order_date" ; rr:datatype xsd:date ]
    ] .
```

### Example 3: Defining a Linkage Rule

```turtle
:LinkageRule_NameMatch rdf:type vekg:LinkageRule ;
    rdfs:label "Linkage Rule based on Name Similarity"@en ;
    vekg:hasLinkFunction :NameSimilarityFunction ;
    vekg:hasMatchClass :Customer .

:NameSimilarityFunction rdf:type vekg:Compare ;
    vekg:operator "jaroWinkler" ;
    vekg:inputSource "fullName" ;
    vekg:inputTarget "customerName" ;
    vekg:threshold 0.9 .
```

### Example 4: Implementing a Fusion Function

```turtle
:PhoneNumberFusionAssertion rdf:type vekg:PropertyFusionAssertion ;
    vekg:hasFusionProperty :phoneNumber ;
    vekg:fusionFunction "mergeUniqueValues" .

:phoneNumber rdf:type vekg:FusionProperty ;
    rdfs:label "Phone Number"@en ;
    rdfs:domain :Customer ;
    rdfs:range xsd:string .
```

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

**Note:** This documentation is intended to assist in understanding and utilizing the Metadata Graph Ontology for building Enterprise Knowledge Graphs. For detailed ontology files, additional examples, and support, please refer to the repository files or contact the maintainers.

# Additional Resources

- **Ontology File:** [Download the Metadata Graph Ontology](ontology/metadata-graph.owl)
- **Example Mappings:** [View example mappings](mappings/)
- **Contact:** For questions or support, please [open an issue](issues/) or contact the maintainers directly.

---

We hope this documentation helps you in creating robust and integrated Enterprise Knowledge Graphs using the Metadata Graph Ontology. By capturing detailed metadata at each step, you can ensure transparency, reproducibility, and maintainability in your EKG projects. Happy modeling!
