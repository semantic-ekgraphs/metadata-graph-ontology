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
  - [Classes](#classes)
  - [Properties](#properties)
- [Advanced Uses of the Metadata Graph Ontology](#advanced-uses-of-the-metadata-graph-ontology)
  - [Dynamic Data Lineage and Provenance Tracking](#dynamic-data-lineage-and-provenance-tracking)
  - [Real-Time Lineage Visibility](#real-time-lineage-visibility)
  - [Metadata-Driven Discovery](#metadata-driven-discovery)
  - [Impact Alerts](#impact-alerts)
  - [Governance and Compliance](#governance-and-compliance)
    - [Dynamic Policy Enforcement](#dynamic-policy-enforcement)
    - [Dynamic Metadata Propagation](#dynamic-metadata-propagation)
  - [Conflict Detection and Resolution](#conflict-detection-and-resolution)
    - [Active Conflict Identification](#active-conflict-identification)
    - [Resolution Propagation](#resolution-propagation)
- [Use Cases and Examples](#use-cases-and-examples)
  - [Case Study: Building an EKG for a Retail Company](#case-study-building-an-ekg-for-a-retail-company)
  - [Advanced Examples](#advanced-examples)
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

**Metadata Graph Ontology Usage:**

- Use `drm:DataAsset` to represent data sources.
- Capture provenance using `vekg:DataSourceProvenance` and properties like `pav:createdBy`, `pav:createdOn`, and `pav:importedFrom`.

**Visualization:**

<p align="center">
  <img src="images/step1-knowledge-acquisition.png" alt="Step 1: Knowledge Acquisition" width="600">
</p>

### Step 2: Construction of Semantic Views

**Objective:** Create a global semantic view by constructing exported views for each data source, comprising an ontology, mappings, and metadata.

- **Semantic View:** Represents the unified schema and vocabulary for the EKG.
- **Exported Semantic View:** Contains the ontology, mappings, and data source metadata for a specific data source.

**Metadata Graph Ontology Usage:**

- Define `vekg:SemanticView` as the global view.
- Use `vekg:ExportedSemanticView` for each data source.
- Represent ontologies with `vekg:SemanticViewOntology` and `vekg:LocalOntology`.
- Capture mappings using `vekg:Mappings`.

**Visualization:**

<p align="center">
  <img src="images/step2-semantic-views.png" alt="Step 2: Construction of Semantic Views" width="600">
</p>

### Step 3: Construction of Linkset Views

**Objective:** Specify linksets that define relationships between instances in different exported semantic views, facilitating identity resolution.

- **Linkset View:** Contains metadata about the links (e.g., `owl:sameAs`) between data from different sources.

**Metadata Graph Ontology Usage:**

- Use `vekg:LinksetView` to represent the linksets.
- Define linkage rules with `vekg:LinkageRule`.
- Capture link functions and match properties.

**Visualization:**

<p align="center">
  <img src="images/step3-linkset-views.png" alt="Step 3: Construction of Linkset Views" width="600">
</p>

### Step 4: Construction of Fusion Views

**Objective:** Define how data from different sources should be fused, resolving conflicts and redundancies to create a unified view.

- **Fusion View:** Contains metadata about how to merge data from different sources.

**Metadata Graph Ontology Usage:**

- Use `vekg:FusionView` to represent the fusion specifications.
- Define fusion properties with `vekg:PropertyFusionAssertion` and `vekg:FusionProperty`.
- Specify fusion functions.

**Visualization:**

<p align="center">
  <img src="images/step4-fusion-views.png" alt="Step 4: Construction of Fusion Views" width="600">
</p>

### Additional Step: Unification View Construction

**Objective:** Normalize and standardize data representations across different sources.

- **Unification View:** Contains metadata about the normalization functions applied to data.

**Metadata Graph Ontology Usage:**

- Use `vekg:UnificationView` to represent unification specifications.
- Define normalization functions with `vekg:normalizeFunction`.

---

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

- **Usage:** Captures the overall metadata about the EKG construction process.

#### 2. **Semantic View (`vekg:SemanticView`)**

The **Semantic View** represents the global semantic perspective of the EKG, integrating domain ontologies and data source schemas.

- **Usage:** Defines the unified schema and vocabulary used across the EKG.

#### 3. **Exported Semantic View (`vekg:ExportedSemanticView`)**

An **Exported Semantic View** is a tuple `(OVi, MVi)` defined over the schema of a local data source, containing the local ontology, mappings, and metadata.

- **Usage:** Represents the semantic view specific to a data source, including its ontology and mappings.

#### 4. **Mappings (`vekg:Mappings`)**

Mappings are a set of rules that enable the transformation of a specific data source according to a Local Ontology.

- **Usage:** Specifies how data from a source maps to the ontology terms.

#### 5. **Linkset View (`vekg:LinksetView`)**

A **Linkset View** specifies relationships (`owl:sameAs`, `rdfs:seeAlso`, etc.) between instances in different exported semantic views, aiding in identity resolution.

- **Usage:** Captures the metadata about links established between different data sources.

#### 6. **Fusion View (`vekg:FusionView`)**

A **Fusion View** contains metadata about how data from different sources should be fused to create a unified representation.

- **Usage:** Defines rules and functions for merging data, resolving conflicts, and consolidating information.

### Properties

Below are key properties used to relate classes within the Metadata Graph Ontology.

#### **Data Source Properties**

- `vekg:hasProvenance` - Links a data source to its provenance metadata.
- `pav:createdBy`, `pav:createdOn` - Capture provenance details.

#### **Semantic View Properties**

- `vekg:hasSemanticViewOntology` - Associates a Semantic View with its Domain Ontology.
- `vekg:hasExportedSemanticView` - Associates a Semantic View with its Exported Semantic Views.

#### **Mappings Properties**

- `vekg:hasMappings` - Connects an Exported Semantic View with its Mappings.

#### **Linkset Properties**

- `vekg:hasLinkageRule` - Associates a Linkset View with a linkage rule.
- `vekg:hasLinkFunction` - Specifies the function used in a linkage rule.
- `vekg:hasMatchClass`, `vekg:hasMatchProperty` - Define the classes and properties used in matching.

#### **Fusion Properties**

- `vekg:hasPropertyFusionAssertion` - Links a Fusion View to its property fusion assertions.
- `vekg:hasFusionProperty` - Specifies the property to be fused.
- `vekg:fusionFunction` - Defines the function used to fuse data.

---

## Advanced Uses of the Metadata Graph Ontology

The Metadata Graph Ontology not only aids in building EKGs but also enhances various data management aspects, including lineage tracking, discovery, governance, and conflict resolution.

### Dynamic Data Lineage and Provenance Tracking

By modeling data sources and their transformations using the ontology, you can achieve **dynamic data lineage tracking**. Each step—from raw data acquisition to final integration—is captured, allowing for:

- **Traceability:** Understand how data moves and transforms across the system.
- **Auditability:** Facilitate audits by providing a clear lineage of data transformations.

**Example:**

```turtle
# Define a transformation process
:Transformation_Process1 rdf:type vekg:Transformation ;
    rdfs:label "ETL Process for Sales Data"@en ;
    pav:createdBy :DataEngineer1 ;
    pav:createdOn "2023-10-20"^^xsd:date ;
    prov:used :DataSource_SalesRaw ;
    prov:generated :DataSource_SalesTransformed .

# Link data sources with the transformation
:DataSource_SalesRaw rdf:type drm:DataAsset ;
    rdfs:label "Raw Sales Data"@en ;
    vekg:hasProvenance :Provenance_SalesRaw .

:DataSource_SalesTransformed rdf:type drm:DataAsset ;
    rdfs:label "Transformed Sales Data"@en ;
    vekg:hasProvenance :Provenance_SalesTransformed ;
    prov:wasGeneratedBy :Transformation_Process1 .
```

In this example, we model a transformation process that uses raw sales data to produce transformed sales data, capturing the lineage and provenance information.

### Real-Time Lineage Visibility

The ontology enables **real-time visibility** into data lineage:

- **Up-to-date Lineage Graphs:** Reflect current data flows and transformations.
- **Immediate Impact Analysis:** Assess how changes in data sources affect downstream processes.

**Example:**

Suppose we have a dashboard that visualizes lineage using SPARQL queries:

```sparql
# Query to retrieve lineage information
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?process ?input ?output
WHERE {
  ?process a vekg:Transformation ;
           rdfs:label ?processLabel ;
           prov:used ?input ;
           prov:generated ?output .
}
```

By running this query, we can dynamically display the lineage of data transformations in our system.

### Metadata-Driven Discovery

A knowledge graph powered by the Metadata Graph Ontology enhances data discovery by:

- **Linking Metadata Elements:** Connect data source descriptions, quality metrics, usage patterns, and lineage.
- **Intelligent Search and Recommendation:** Users can discover datasets based on rich metadata context, such as usage histories or semantic similarities.
- **Facilitating Data Catalogs:** Build comprehensive data catalogs that are easily searchable and navigable.

**Example:**

```turtle
# Define data quality metrics
:DataSource_CustomerProfiles rdf:type drm:DataAsset ;
    rdfs:label "Customer Profiles Dataset"@en ;
    vekg:hasQualityMetric :QualityMetric_Completeness .

:QualityMetric_Completeness rdf:type vekg:QualityMetric ;
    rdfs:label "Completeness Metric"@en ;
    vekg:metricValue "0.95"^^xsd:float ;
    vekg:metricType "Completeness" .

# Define usage patterns
:DataAnalysis_Project1 rdf:type vekg:UsagePattern ;
    rdfs:label "Marketing Analysis Project"@en ;
    vekg:usesDataSource :DataSource_CustomerProfiles ;
    pav:createdBy :DataAnalyst1 ;
    pav:createdOn "2023-10-18"^^xsd:date .
```

Users can search for datasets with high completeness or those used in similar projects, enhancing data discovery.

### Impact Alerts

The ontology allows for the implementation of **impact alerts**:

- **Change Propagation Assessment:** When a dataset changes, the graph assesses how these changes affect linked datasets or applications.
- **Stakeholder Notification:** Automatically notify relevant stakeholders, ensuring real-time awareness of impacts on linked data.
- **Proactive Issue Resolution:** Enable teams to address potential data issues before they affect business processes.

**Example:**

```turtle
# Define a data source update event
:DataSource_CRM_Update rdf:type vekg:DataSourceEvent ;
    rdfs:label "CRM Data Source Updated"@en ;
    pav:createdOn "2023-10-22T15:00:00Z"^^xsd:dateTime ;
    vekg:affects :ExportedView_CRM .

# Notify stakeholders
:Notification_CRMUpdate rdf:type vekg:ImpactAlert ;
    rdfs:label "CRM Data Update Alert"@en ;
    vekg:alertMessage "CRM data source has been updated, which may affect downstream processes." ;
    vekg:relatedEvent :DataSource_CRM_Update ;
    vekg:notify :DataEngineer1, :DataAnalyst1 .
```

The system can automatically generate alerts and send notifications to the relevant users.

### Governance and Compliance

#### Dynamic Policy Enforcement

Active metadata in the knowledge graph helps enforce data governance policies:

- **Real-Time Enforcement:** Link data with access policies to ensure sensitive data is accessed appropriately.
- **Policy Updates Propagation:** When governance policies change (e.g., new regulations), updates propagate across datasets and workflows.
- **Audit Readiness:** Maintain compliance records and demonstrate adherence to policies.

**Example:**

```turtle
# Define access policies
:Policy_PIIAccess rdf:type vekg:AccessPolicy ;
    rdfs:label "PII Data Access Policy"@en ;
    vekg:appliesTo :DataSource_CustomerProfiles ;
    vekg:allowedRoles :DataPrivacyOfficer, :ComplianceManager .

# Link data source with policy
:DataSource_CustomerProfiles vekg:hasAccessPolicy :Policy_PIIAccess .

# Define user roles
:User_JaneDoe rdf:type foaf:Person ;
    rdfs:label "Jane Doe"@en ;
    vekg:hasRole :DataPrivacyOfficer .

# Enforcement logic (pseudo-code)
IF User_Requesting_Access hasRole allowedRoles
THEN Grant_Access
ELSE Deny_Access
```

In this example, access to customer profiles is controlled based on the user's role, as defined in the ontology.

#### Dynamic Metadata Propagation

The ontology supports dynamic propagation of metadata:

- **Synchronization Across Datasets:** When an exported view is updated, downstream views and linksets are automatically updated.
- **Consistency Maintenance:** Ensure active metadata remains synchronized across the entire data architecture.
- **Efficiency Gains:** Reduce manual efforts in updating metadata across multiple systems.

**Example:**

```turtle
# Update an exported semantic view
:ExportedView_CRM vekg:lastModified "2023-10-22T15:00:00Z"^^xsd:dateTime .

# Automatically update dependent linksets
:LinksetView_Customers vekg:lastModified "2023-10-22T15:05:00Z"^^xsd:dateTime ;
    vekg:dependsOn :ExportedView_CRM, :ExportedView_Ecom .

# Propagate changes to fusion views
:FusionView_Customers vekg:lastModified "2023-10-22T15:10:00Z"^^xsd:dateTime ;
    vekg:dependsOn :LinksetView_Customers .
```

Metadata updates propagate through the graph, ensuring all components are in sync.

### Conflict Detection and Resolution

#### Active Conflict Identification

The knowledge graph can automatically detect conflicts or inconsistencies:

- **Data Inconsistencies:** Identify differing values for the same attribute across datasets.
- **Real-Time Flagging:** Highlight issues as they occur, allowing for immediate attention.
- **Suggested Resolutions:** Propose strategies such as prioritizing data sources based on reliability or timeliness.

**Example:**

```turtle
# Detect conflicting data
:Customer123 ex:email "customer@example.com" .
:Customer123_duplicate ex:email "customer@oldexample.com" .

# Conflict identification
:Conflict_Email rdf:type vekg:DataConflict ;
    vekg:conflictingValues "customer@example.com", "customer@oldexample.com" ;
    vekg:affectedEntity :Customer123 ;
    vekg:detectedOn "2023-10-22T16:00:00Z"^^xsd:dateTime .

# Suggested resolution
:Conflict_Email vekg:resolutionSuggestion "Use the most recent email address." ;
    vekg:prioritySource :DataSource_CRM ;
    vekg:status "Pending" .
```

#### Resolution Propagation

Once conflicts are resolved, the ontology ensures:

- **Propagation of Resolutions:** Updates are reflected in all dependent datasets and metadata views.
- **System-Wide Consistency:** The entire data ecosystem remains consistent and up-to-date.
- **Reduced Redundancy:** Eliminates conflicting data, improving data quality.

**Example:**

```turtle
# Resolve the conflict
:Customer123 ex:email "customer@example.com" .
:Conflict_Email vekg:status "Resolved" ;
    vekg:resolutionAppliedOn "2023-10-22T17:00:00Z"^^xsd:dateTime .

# Update dependent views
:ExportedView_CRM vekg:lastModified "2023-10-22T17:05:00Z"^^xsd:dateTime .
:FusionView_Customers vekg:lastModified "2023-10-22T17:10:00Z"^^xsd:dateTime .
```

---

## Use Cases and Examples

### Case Study: Building an EKG for a Retail Company

**Scenario:** A retail company wants to integrate customer data from multiple sources, including a CRM system and e-commerce platform, into a unified EKG to enhance customer insights and personalize marketing strategies.

*(This case study was detailed earlier in the documentation.)*

### Advanced Examples

#### Example 1: Implementing Real-Time Lineage Visibility

**Objective:** Display an up-to-date lineage graph showing how data flows from sources through transformations to final datasets.

**SPARQL Query:**

```sparql
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?processLabel ?inputLabel ?outputLabel
WHERE {
  ?process a vekg:Transformation ;
           rdfs:label ?processLabel ;
           prov:used ?input ;
           prov:generated ?output .
  ?input rdfs:label ?inputLabel .
  ?output rdfs:label ?outputLabel .
}
```

**Visualization:**

Use a graph visualization tool to display the results, showing nodes for data sources and transformations, and edges representing "used" and "generated" relationships.

#### Example 2: Enforcing Dynamic Access Policies

**Objective:** Ensure that only authorized users can access sensitive data, and update policies dynamically as regulations change.

**Policy Definition:**

```turtle
:Policy_GDPRCompliance rdf:type vekg:AccessPolicy ;
    rdfs:label "GDPR Compliance Policy"@en ;
    vekg:appliesTo :DataSource_CustomerData ;
    vekg:allowedRoles :DataPrivacyOfficer, :DataAnalystGDPRCompliant ;
    vekg:policyEffectiveDate "2023-10-01"^^xsd:date ;
    vekg:policyReviewDate "2024-10-01"^^xsd:date .
```

**User Request Handling (Pseudo-code):**

```pseudo
IF User_Requesting_Access hasRole allowedRoles
AND CurrentDate >= policyEffectiveDate
AND CurrentDate < policyReviewDate
THEN Grant_Access
ELSE Deny_Access
```

**Dynamic Update:**

If regulations change, update the `vekg:allowedRoles` or policy dates, and the system will enforce the new policy immediately.

#### Example 3: Conflict Detection and Automatic Resolution

**Objective:** Detect when conflicting data is ingested and resolve it using predefined rules.

**Conflict Detection Rule:**

```turtle
# Define a rule to detect conflicting phone numbers
:ConflictDetectionRule_Phone rdf:type vekg:ConflictDetectionRule ;
    rdfs:label "Phone Number Conflict Rule"@en ;
    vekg:appliesToProperty ex:phoneNumber ;
    vekg:conflictCondition "Different values for the same customer" ;
    vekg:resolutionStrategy "Keep the phone number from the most reliable source" .
```

**Automatic Resolution:**

```turtle
# Upon detecting a conflict
:Customer456 ex:phoneNumber "123-456-7890" .
:Customer456_duplicate ex:phoneNumber "098-765-4321" .

# The system identifies the conflict and applies the resolution strategy
:Customer456 ex:phoneNumber "123-456-7890" . # From the most reliable source

# Update the conflict status
:Conflict_Phone_Customer456 vekg:status "Resolved" ;
    vekg:resolutionAppliedOn "2023-10-22T18:00:00Z"^^xsd:dateTime .
```

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

We hope this documentation helps you in creating robust and integrated Enterprise Knowledge Graphs using the Metadata Graph Ontology. By capturing detailed metadata at each step, you can ensure transparency, reproducibility, and maintainability in your EKG projects.

Happy modeling!

---

*This documentation was generated to assist in understanding and utilizing the Metadata Graph Ontology for building Enterprise Knowledge Graphs. For detailed ontology files, additional examples, and support, please refer to the repository files or contact the maintainers.*
