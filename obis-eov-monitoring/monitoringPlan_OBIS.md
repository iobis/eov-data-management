# DRAFT: EOV Monitoring Plan for OBIS

**Authors**: Elizabeth Lawrence, Laura Brenskelle, Takashi Hosono, Rubén Pérez-Pérez, Pieter Provoost, Katherine Tattersall, Anton Van de Putte

## Background

OBIS plays a central role in aggregating marine biodiversity data and supports the Global Ocean Observing System (GOOS). To aggregate, quantify, and monitor relevant data in OBIS, it must first be made explicitly identifiable. The primary objective of this deliverable is to design a systematic approach for tagging, quantifying, and monitoring data relevant to the GOOS Essential Ocean Variables (EOVs) within OBIS, in support of OBIS Nodes (IODE NODCs and ADUs) and the GOOS BioEco community.

By tagging data in this way, OBIS can enable downstream use and facilitate development of EOV monitoring data products, directly supporting OBIS Objective I (Build a sustainable global marine biodiversity data infrastructure) and III (Delivering operational biodiversity data services).

## Approach

Our proposed approach focuses on identifying and tagging datasets with **EOV keywords**. Keywords should include the full GOOS EOV names. It also may be useful to consider tags for each of the EOV specific sub-variables. Keywords should be linked to controlled vocabulary, such as those defined by ENVO or NERC.

Tagging will initially occur at the **dataset level**, rather than at the individual record level, for the following reasons:

* EOV reporting and attribution often align with dataset-level initiatives or projects
* Metadata fields are easier to standardize at dataset level
* OBIS infrastructure already supports dataset-level keyword indexing.

In cases where a dataset contains data on multiple EOVs, all relevant EOVs will be included as keywords.

### Tagging Criteria

A dataset may be tagged with an EOV if:

1. It contains taxonomic or measurement data corresponding to an EOV and its sub-variables(s)  
2. It includes sufficient methodological metadata or data to interpret its relevance (e.g. samplingProtocol, sampleSize, instruments recorded in eMoF, etc.)
3. It supports at least one defined EOV sub-variable.

We have identified three possible tagging categories to be considered:

* **Collected for EOV monitoring** (explicitly aligned with GOOS objectives)  
* **Contributing to EOV (ad hoc)** (retrospective alignment - identified by the OBIS pipeline logic)
* **Manually identified by a third party** (e.g. by OBIS Nodes, NODCs, ADUs, etc.)

Implementing the distinction between these categories will be defined at a later phase as they require different tag types, and we acknowledge that it would be beneficial to distinguish between datasets identified programmatically or manually, compared to datasets that considered EOV sampling approaches from the beginning.

### Challenges

Several challenges are expected in implementing this approach:

**Partial dataset relevance.** Some datasets may contain only a subset of records relevant to a given EOV (e.g., multi-taxon surveys). In these cases, a dataset qualifies if at least one EOV sub-variable is represented. Future refinements may enable partial dataset or record-based tagging. Where percent of EOV-relevant records can be calculated, this information will be provided.

**Tag application.** If tags are added to EML metadata, datasets would need to be republished from the IPT, causing potentially significant overhead for some OBIS Nodes. Therefore we suggest applying tags in the OBIS data pipeline, maintaining them separately from the source EML and applying them programmatically at ingestion into the global OBIS database. Data providers and nodes are welcome to supplement this with EML level EOV tags.

**Methodology identification.** Identifying the methodological approach used to measure a specific sub-variable. Multiple DwC fields will be necessary to filter (e.g. samplingProtocol, samplingEffort, occurrenceRemarks, measurementType, etc.), and since fields are often free-text, this poses challenges to filter systematically. We will further explore this challenge during pilot cases. We acknowledge that either or both the dataset tables or EML may capture additional methodological details and may also need to be included when assessing whether a dataset is tagged as EOV-compliant.

## Implementation Plan

To facilitate downstream querying and filtering of EOV-tagged datasets, a dedicated field will be added to the OBIS Parquet export at the record level, with corresponding API filter support.

For initial implementation, we focus on a subset of EOVs with clearly defined sub-variables and taxonomic scope: Seagrass cover and composition and Marine Mammals abundance and distribution. These will serve as pilot cases before extending the approach to Macroalgae, Mangrove, Sea Turtles, and Seabirds. For the pilot EOVs, the tagging workflow will proceed as follows:

1. Filter by taxonomy  
2. Identify presence records  
3. Determine whether relevant measurement records map to EOV sub-variables  
4. Add EOV keywords to qualifying datasets

Step 3 requires a clear understanding of how EOV specification sheets map to Darwin Core terms, so that qualifying records can be identified programmatically. For the pilot EOVs, the relevant sub-variables are:

* Seagrass: percent cover, species composition, and areal extent
* Marine Mammals: presence/absence, count data, and repeated individual presence (tracking/resights).

To support this mapping, we are developing a separate EOV Specification Sheet Transformation Guide, which documents how we use AI to assist in generation of DwC criteria lists from the spec sheets and transform them to machine-readable formats (e.g. LinkML) suitable for programmatic querying. This guide and associated code will be documented in the OBIS EOV Data Management GitHub repository ([https://github.com/iobis/eov-data-management](https://github.com/iobis/eov-data-management)). A summary of this transformation process is provided below:

1. Prompt AI to transform EOV specification sheets to DwC
   a. Human validation of DwC terms
2. Transform DwC into machine readable format of EOV criteria (linkML)
   b. Programmatic validation of LinkML
3. Generate a script to evaluate OBIS data using LinkML  (e.g. AI assisted)
4. Validate list of EOV datasets identified

## Timeline

The first step for this pipeline - adding the API filtering support - was completed by the end of April 2026. A proof of concept of the pipeline, focusing on Seagrass, including a) the AI-prompts to be reused for EOV spec sheets and for conversion to machine readable formats, and b) the script to programmatically categorize OBIS data for EOV tagging was completed by the end of June 2026. The AI-prompts and associated code need to be further refined to accurately identify EOV data records.

We suggest the extension of the pipeline to Marine Mammals and the remaining EOVs be further refined and implemented as part of the next OBIS DCG workplan.
