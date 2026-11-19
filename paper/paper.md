---
title: 'BioHackEU26 report: A submission and visualisation interface for sharing community interoperability practices'
title_short: 'BioHackEU26 #5: community interoperability practices'
tags:
  - interoperability
  - research data management
  - ontologies
  - FAIR workflows
  - visualisation
authors:
  - name: Bilgehan Nevruz
    affiliation: 1
    role: Software, Visualization, Writing – review & editing
  - name: Nick Juty
    orcid: 0000-0002-2036-8350
    affiliation: 2
    role: Conceptualization, Project administration, Writing – review & editing
  - name: Tooba Abbassi-Daloii
    orcid: 0000-0002-4904-3269
    affiliation: 1
    role: Conceptualization, Project administration, Writing – original draft
affiliations:
  - name: Amsterdam UMC, Amsterdam, The Netherlands
    ror: 05grdyy37
    index: 1
  - name: The University of Manchester, Manchester, United Kingdom
    index: 2
date: November 2026
cito-bibliography: paper.bib
event: BH26EU
biohackathon_name: "BioHackathon Europe 2026"
biohackathon_url: "https://biohackathon-europe.org/"
biohackathon_location: "Barcelona, Spain, 2026"
group: Project 5
git_url: https://github.com/tabbassidaloii/elixir_biohackathon_2026_no5
authors_short: Nevruz, B. \emph{et al.}
---

# Abstract

Interoperability practices developed across research projects, institutes, and scientific communities are frequently scattered across presentation slides, internal wikis, spreadsheets, and email threads, making them difficult to discover, compare, and reuse. Project 5 at BioHackathon Europe 2026 addressed this gap by developing a self-service submission and visualisation workflow structured around the RDMkit Data Life Cycle.

The team mapped an existing 41-question lifecycle survey to controlled vocabularies, ontologies, registries, and community standards, and defined a dual-loop workflow combining structured dropdown selections with human-reviewed free-text terminology suggestions. The resulting practice objects support machine-readable export (JSON/YAML) and multi-level visual comparison while preserving original respondent descriptions. Deployed via GitHub Pages alongside public documentation, the platform provides an open, maintainable foundation for communities to submit, compare, and iteratively improve interoperability practices.

# Introduction

Research communities regularly encounter the same practical challenges: capturing data and metadata, choosing identifiers and file formats, selecting tools, depositing outputs, and maintaining reproducibility. Because these practices are typically documented informally for local audiences, researchers and data stewards struggle to see how comparable domains handle similar tasks or which standards they apply.

An ongoing ELIXIR Interoperability Platform activity has captured such practices through guided interviews structured around the RDMkit Data Life Cycle. While facilitation helps respondents interpret questions, reliance on interviewer time and manual synthesis limits scalability. Project 5 set out to translate this interview process into a lightweight, self-service resource that records practices in their community context and links them to relevant resources (including FAIRsharing, bio.tools, software repositories, repository registries, standards, and ontologies).

Specifically, the project pursued five objectives:

1. Convert the guided questionnaire into a self-service submission workflow.

2. Represent submitted practices as structured, machine-readable objects (JSON and YAML).

3. Ground applicable fields in ontologies, registries, standards, databases, and role vocabularies.

4. Define visualisation views spanning high-level community overviews to granular task, tool, input/output, and contributor details.

5. Establish a curator-reviewed feedback loop so validated free-text terms enrich dropdown options for future submissions.

Target users include researchers seeking domain reference workflows, data stewards documenting operational practices, community coordinators, and maintainers of RDM ecosystem resources.

# Approach

## Design inputs and workstreams

Building on the existing lifecycle questionnaire, manual synthesis spreadsheets, and mock examples, the team decomposed the workflow into four technical streams: (1) user interface and visualisation requirements, (2) question-to-terminology mapping, (3) a review-gated vocabulary feedback loop, and (4) ecosystem interlinking.

## Questionnaire and terminology mapping

Each survey question was evaluated to define the target concept, the expected resource category, and candidate controlled terms for dropdown selection or automated suggestion. Candidate mappings were designed as an enrichment layer subject to human verification rather than an automatic replacement for respondent wording.

## Submission and curation workflow

The architecture supports three data-entry routes: importing Data Stewardship Wizard (DSW) outputs, completing the questionnaire directly in the web interface, or loading an existing JSON practice record. A central master JSON file supplies reviewed dropdown options to both DSW and the web interface.

When no suitable dropdown option exists, respondents enter free text. An LLM-assisted suggestion step proposes candidate ontology or registry terms for user review prior to submission. Before new free-text terms are added to the master options file, administrators perform semantic quality control to prevent category mismatches (such as assigning a process description to a role question).

```
graph TD;
    subgraph Master Vocabulary Repository
        OPT[Master Options JSON File<br/>Central Controlled Vocabulary Store]
    end

    subgraph Data Entry & Ingestion
        OPT -->|Populates Dropdown Options| A1[Fill Questionnaire in DSW]
        OPT -->|Populates Dropdown Options| A2[Fill Questions Directly on UI]
        A1 -->|Export DSW File| B1[Upload DSW Output to UI]
        A2 --> C[Submission & Visualisation UI]
        A3[Existing Practice JSON] -->|Load JSON| C
        B1 --> C
    end

    subgraph Controlled Vocabulary & LLM Mapping
        C -->|Select Pre-mapped Choice| D1[Dropdown Options: T4FS / EDAM / bio.tools]
        C -->|Enter Free-Text Answer| D2[LLM Ontology Suggester]
        D2 -->|Review & Confirm Mapping| C
    end

    subgraph User Visualisation & Comparative Analysis
        C -->|Submit Practice Object| E1[Network Graph Visualisation]
        E1 -->|Cross-Comparison| E2[Compare with Historical Community Responses]
        E1 -->|Export Diagrams| E3[Download SVG / PNG / JSON]
    end

    subgraph Admin Validation & Feedback Loop
        C -->|Submit for Curation| F1[Admin Review & Semantic QC]
        F1 -->|Sanity Check: e.g. Role vs Process| F2[Validate Free-Text & Ontology Mappings]
        F2 -->|Add Approved New Options| OPT
    end

```

## Visualisation requirements

Interface requirements were structured around four progressive inspection levels:

1. **Community view:** Overview mapped onto the RDM lifecycle.

2. **Multi-community view:** Cross-domain comparison across topics or projects.

3. **Phase view:** Filtered inspection of one or more selected lifecycle stages.

4. **Task view:** Detail card exposing tools, inputs, outputs, roles, and original respondent text alongside mapped terms.

Additional design features include a lifecycle wheel, search by topic or data type, visual indicators for incomplete sections, and pivots by data type, role, community, or ELIXIR Node expertise.

# Results

## Questionnaire-to-resource mapping

All 41 survey items across seven lifecycle stages were classified into seven resource categories (`database`, `tool`, `ontology`, `registry`, `individual document`, `community standard`, and `roles`) and linked to target vocabularies (Table 1). Role questions were mapped to Terms4FAIRskills (e.g. `T4FS:0000178`) and the Contributor Role Ontology (`CRO`), while operations, formats, tools, repositories, and licences were aligned with domain registries.

Table: Summary of questionnaire sections mapped to resource types and candidate vocabularies or registries.

| Lifecycle section | Target concepts | Resource types | Candidate ontologies and registries | 
| ----- | ----- | ----- | ----- | 
| **1. Plan** | Authoring tools, storage, validation, guidance, roles | tool, database, registry, individual document, community standard, roles | DSW, FAIRsharing, RDMkit, `T4FS` | 
| **2. Collect** | Object types, formats, SOPs, persistent identifiers, QA roles | ontology, tool, community standard, registry, roles | EDAM (`format`), FAIRsharing, DataCite, `T4FS` | 
| **3. Process** | Cleaning steps, standardisation, integrity checks, QC roles | tool, community standard, roles | EDAM (`operation`), WorkflowHub, `T4FS` | 
| **4. Analyse** | Analytical methods, software, reproducibility, visualisation, training | ontology, tool, registry, community standard, roles | EDAM, SWO, bio.tools, ELIXIR TeSS, FAIR Cookbook | 
| **5. Preserve** | Repositories, backups, archiving policies, certification | database, tool, individual document, community standard, roles | re3data, FAIRsharing, CoreTrustSeal, `T4FS` | 
| **6. Share** | Shared objects, platforms, legal/ethical constraints, metadata | database, individual document, community standard, roles | Zenodo, ENA, DUO, Dublin Core, `T4FS` | 
| **7. Reuse** | Reuse planning, standards, licences, discoverability | community standard, individual document, registry, roles | SPDX, DataCite, Schema.org, `T4FS` | 

## Prototype implementation and GitHub-hosted deployment

An initial web prototype (`https://bh.bilgehan.nl/submit`) and aligned DSW templates were built around the shared JSON options file, supporting network graph comparison against prior submissions and diagram export in SVG, PNG, and JSON formats. Test responses were used to prototype the LLM-assisted terminology suggester and admin review workflow.

To enable open community contributions to both practice content and ongoing site maintenance via pull requests, a GitHub-hosted version of the submission interface (`https://biocorecrg.github.io/biohackathon-community-practices-2026/submit`) and accompanying documentation portal (`https://biocorecrg.github.io/biohackathon-community-practices-2026/docs/`) were deployed.

## Visualisation mockups and seed examples

UI mockups were created for the lifecycle wheel, search interface, incomplete-section masking, and task detail cards. Initial seed records from participating ELIXIR communities (biodiversity, toxicology, and rare diseases) were compiled to test the submission workflow and demonstrate the prototype during the hackathon poster session.

# Discussion

## Cross-project alignment

Discussions with parallel BioHackathon projects informed schema and integration choices:

* **Credit and recognition (Project #6):** Granular role annotations (`T4FS` and `CRO`) can support attribution for data stewards, bioinformaticians, and infrastructure contributors beyond traditional publications.

* **Tool registries and cross-resource linking (Project #20):** Exchanges with `bio.tools` and cross-document linking initiatives guided strategies for connecting practice records to registry entries, RDMkit Tool Assemblies, and GitHub repositories.

* **Semantic framework standards (Project #3):** Reviewed for alignment and identified as a prospective contributor of community records.

## Remaining decisions and next steps

1. **Automated ecosystem synchronisation:** Establish push/pull pipelines linking GitHub-hosted practice records and master vocabulary options with RDMkit Tool Assemblies, DSW knowledge models, and FAIRsharing.

2. **Production curation governance:** Deploy the LLM suggestion pipeline in production and formalise editorial criteria for reviewing pull requests and master vocabulary additions.

3. **Community validation and naming:** Expand seed examples across additional domains, evaluate usability with data stewards, and finalise a permanent tool name.

## Acknowledgements

We thank the BioHackathon Europe 2026 organisers, ELIXIR Europe, and the ELIXIR Interoperability Platform for supporting Project 5.

# References

```
\AtEndDocument{%
}

```
