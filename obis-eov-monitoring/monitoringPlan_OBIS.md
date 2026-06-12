# DRAFT: EOV Monitoring Plan for OBIS

**Authors**: Elizabeth Lawrence, Ruben Perez Perez, Pieter Provoost, Takashi Hosono, Katherine Tattersall

## Background

OBIS plays a central role in aggregating marine biodiversity data and supports the Global Ocean Observing System (GOOS). The primary objective of this deliverable is therefore to design a systematic approach for tagging, quantifying, and monitoring EOV-relevant datasets within OBIS, in support of both OBIS Nodes and the GOOS BioEco community.

## Approach

To quantify and monitor EOV data in OBIS, it must first be made explicitly identifiable. Our proposed approach focuses on identifying and tagging datasets with **EOV keywords**. Keywords should include the full GOOS EOV names, however it may be useful to consider tags for each of the EOV specific sub-variables. Keywords should also be linked to controlled vocabulary, such as those defined by ENVO or NERC.

Tagging will occur at the **dataset level**, rather than at the individual record level, for the following reasons:

* EOV reporting and attribution often align with dataset-level initiatives or projects  
* Metadata fields are easier to standardize at dataset level  
* OBIS infrastructure already supports dataset-level keyword indexing.

In cases where a dataset contains data on multiple EOVs, all relevant EOVs will be included as keywords.

### Tagging Criteria

A dataset may be tagged with an EOV if:

1. It contains taxonomic or measurement data corresponding to an EOV and its sub-variables(s)  
2. It includes sufficient methodological metadata to interpret its relevance (e.g. samplingProtocol)  
3. It supports at least one defined EOV sub-variable.

We have identified two possible tagging categories to be considered:

* **Collected for EOV monitoring** (explicitly aligned with GOOS objectives)  
* **Contributing to EOV (ad hoc)** (retrospective alignment)

The distinction between these categories will be defined at a later phase as they may not necessarily require different tag types, although we acknowledge that it would be beneficial to distinguish between datasets identified programmatically compared to datasets that considered EOV sampling approaches from the beginning.

### Challenges

Several challenges are expected in implementing this approach:

**Partial dataset relevance.** Some datasets may contain only a subset of records relevant to a given EOV (e.g., multi-taxon surveys). In these cases, a dataset qualifies if at least one EOV sub-variable is represented. Future refinements may enable partial dataset or record-based tagging.

**Tag application.** If tags are added to EML metadata, datasets would need to be republished from the IPT, causing potentially significant overhead for some OBIS Nodes. Therefore we suggest applying tags in the OBIS data pipeline, maintaining them separately from the source EML and applying them programmatically at ingestion into the global OBIS database.

**Methodology identification.** Identifying the methodological approach used to measure a specific sub-variable. The DwC field samplingProtocol will be necessary to filter, but since the field is free-text, this poses challenges to filter systematically. We will further explore this challenge during pilot cases. We acknowledge that either or both the eMoF or EML may capture additional methodological details and may also need to be included when assessing whether a dataset is tagged as EOV-compliant.

## Implementation Plan

To facilitate downstream querying and filtering of EOV-tagged datasets, a dedicated field will be added to the OBIS Parquet export at the database level, with corresponding API filter support. This technical step will be implemented by Pieter Provoost.

For initial implementation, we focus on a subset of EOVs with more clearly defined sub-variables and taxonomic scope: Seagrass cover and composition and Marine Mammals abundance and distribution. These will serve as pilot cases before extending the approach to Macroalgae, Mangrove, Sea Turtles, and Seabirds. For the pilot EOVs, the tagging workflow will proceed as follows:

1. Filter by taxonomy  
2. Identify presence records  
3. Determine whether relevant measurement records map to EOV sub-variables  
4. Add EOV keywords to qualifying datasets

Step 3 requires a clear understanding of how EOV specification sheets map to Darwin Core terms, so that qualifying records can be identified programmatically. For the pilot EOVs, the relevant sub-variables are:

* Seagrass: percent cover, species composition, and areal extent

* Marine Mammals: presence/absence, count data, and repeated individual presence (tracking/resights).

To support this mapping, we are developing a separate [EOV Specification Sheet Transformation Guide](https://docs.google.com/document/d/12S0Rxfha0HYZJnm6E6X2LhO2akJq6TEcl6dQE9r0t-g/edit?tab=t.0), which documents how AI-assisted prompting is used to generate DwC criteria lists from the spec sheets and transform them into machine-readable formats (e.g. LinkML) suitable for programmatic querying. This guide and associated code will be documented in the OBIS EOV Data Management GitHub repository ([https://github.com/iobis/eov-data-management](https://github.com/iobis/eov-data-management)).

## Timeline

The first step for this pipeline \- adding the API filtering support, was completed by the end of April 2026\. Then, a first prototype of the pipeline, focusing on Seagrass, including the AI-prompts to be reused for EOV spec sheets and the conversion to machine readable formats, and the code to programmatically query using that machine readable format is planned to be completed by the end of June 2026\. The extension of the pipeline to the remaining EOVs will be tested and implemented by October 2026\.
