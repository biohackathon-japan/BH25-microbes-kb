---
title: 'RDF integration of DSMZ microbial databases'
title_short: 'BioHackJP25: DSMZ microbial RDF'
tags:
  - Semantic web
  - Ontologies
  - Workflows
  - Microbiology
authors:
  - name: Julia Koblitz
    orcid: 0000-0002-7260-2129
    affiliation: 1
  - name: Shuichi Kawashima
    affiliation: 2
    orcid: 0000-0001-7883-3756
  - name: Yoko Okabeppu
    affiliation: 3
  - name: Takeru Nakazato
    affiliation: 4
    orcid: 0000-0002-0706-2867
  - name: Daisuke Sato
    affiliation: 5
  - name: Atsuko Yamaguchi
    affiliation: 6
  - name: Akira Kinjo
    affiliation: 7
  - name: Toshiaki Katayama
    affiliation: 2
affiliations:
  - name: Leibniz Institute DSMZ-German Collection of Microorganisms and Cell Cultures
    index: 1
  - name: Database Center for Life Science, Joint Support-Center for Data Science Research, Research Organization of Information and Systems
    index: 2
  - name: OKBP Inc.
    index: 3
  - name: Biological Resource Center, National Institute of Technology and Evaluation
    index: 4
  - name: Kamonohashi Inc.
    index: 5
  - name: Tokyo City University
    index: 6
  - name: Anima Machina G.K.
    index: 7
  
date: 15 September 2025
cito-bibliography: paper.bib
event: BH25JP
biohackathon_name: "DBCLS BioHackathon 2025"
biohackathon_url:   "https://2025.biohackathon.org/"
biohackathon_location: "Mie, Japan, 2025"
group: Microbial Knowledgebases
git_url: https://github.com/biohackathon-japan/BH25-microbes-kb
authors_short: Koblitz, Kawashima et al.
---

# Abstract

The integration of microbial life science databases into the Semantic Web is a crucial step toward interoperable and machine-actionable research data. During the DBCLS BioHackathon 2025 in Japan, we focused on DSMZ resources, including Bac*Dive*, BRENDA, and Media*Dive*, to establish RDF-based access and interoperability. We created RDF-config files for these databases, made their RDF data available through the RDF Portal SPARQL endpoint, explored natural language querying interfaces, and engaged in quality monitoring via YummyData. Our work demonstrates that lightweight RDF-configs can significantly enhance reusability and accessibility of microbial data across infrastructures, paving the way for improved data integration and AI-driven discovery.

# Introduction

Life science research increasingly relies on integrating heterogeneous datasets to address complex biological questions. Databases of microbial strains, enzymes, and growth media contain essential, often unique data that underpin studies in microbiology, biotechnology, ecology, and medicine. However, many of these resources are provided through web portals or bespoke APIs, where differing data models, incomplete metadata, and lack of semantic interoperability hamper reuse, federated querying, and linkage with other datasets.

Semantic Web technologies, in particular RDF (Resource Description Framework) and SPARQL, offer means to represent data in a standardized, machine-readable form, enabling data from disparate sources to be interlinked and queried in a unified manner. The Linked Data principles, outlined by Tim Berners-Lee and others, emphasize use of URIs, dereferenceable HTTP identifiers, standard formats, and linking to external resources, forming the basis of interoperable data publication [@Heath2009; @Bizer2009]. Ontologies and schema resources such as those in the OBO Foundry provide further structure and allow logical definitions, consistency, and shared understanding of biological entities and relations [@Smith2007; @Hoehndorf2015].

At DSMZ, several key databases contain rich microbial data: Bac*Dive* (the Bacterial Diversity Metadatabase) offers extensive phenotypic and genotypic information on bacterial and archaeal strains [@citesAsDataSource:BacDive2025]; BRENDA is a comprehensive enzyme information system [@citesAsDataSource:BRENDA2021]; and Media*Dive* provides detailed recipes and conditions for microbial culture media [@citesAsDataSource:MediaDive2022]. While these resources are invaluable, their current web-based access methods limit interoperability and machine-actionability. After the last Biohackthon in 2024, we started to publish all three databases in RDF format and provide SPARQL endpoints, which was further advanced during this hackathon. This semantic publication opens up possibilities: facilitating cross-database search, enabling combination of phenotype, media, enzymology, taxonomy data; supporting AI/LLM-based querying; allowing monitoring of service quality; and promoting reuse by external groups. 

Motivated by these potential benefits, our project set out with the following aims: define RDF-config (schema) files for selected DSMZ resources; load and publish RDF data into RDF-config to integrate them into an interoperable RDF ecosystem; explore access interfaces that lower the barrier to entry (including natural language queries); and integrate these published endpoints into monitoring services to ensure long-term usability and reliability.

# Methods

We developed RDF-config files for Bac*Dive*, BRENDA, and Media*Dive* using the existing RDF-config repository at DBCLS (https://github.com/dbcls/rdf-config). These configuration files define mappings from original database schemas to RDF constructs, including URIs, classes, properties, and constraints. Once configurations were tested, we converted data for Bac*Dive* and Media*Dive* into RDF form, and loaded them into the RDF Portal infrastructure, making them accessible through a public SPARQL endpoint.

To test usability beyond standard SPARQL querying, we combined the Claude desktop large language model with a tool called RDFPortal-MCP (Mapping, Composition, and Processing), setting up a server that accepts natural language queries and translates or maps them to SPARQL queries over our published RDF data. We kept a working log to record successes, failures, edge cases, and to guide further improvements.

In order to promote quality, sustainability, and discoverability, we registered the DSMZ SPARQL endpoints at YummyData (also known as UmakaData), which monitors RDF endpoints for metadata completeness, uptime, response behaviour, and compliance with Linked Data best practices.

# Results

We produced RDF-config files for the three DSMZ resources and committed them to the RDF Config repository. These were reused by other projects during the hackathon, indicating they fulfill general needs beyond our immediate datasets. Bac*Dive* and Media*Dive* RDF data were successfully loaded into the RDF Portal and made queryable via its SPARQL endpoint. Through the LLM + MCP server approach, we implemented natural language querying; our working log highlights both promising cases (e.g., simple queries retrieving strain metadata or media composition) and challenges (ambiguous terms, mismatches between user phrasing and data schema, ontology term selection). 

As a concrete example, we queried MediaDive to retrieve media formulations under specific conditions. The query required that the medium contained carbon dioxide gas (CO₂) but no organic compounds. Such media are typically used for the cultivation of chemolithoautotrophic microorganisms, and therefore extracting them can help identify potential candidates for chemolithoautotrophy. For the initial query, “List MediaDive media containing carbon dioxide gas (CO₂) but not containing organic compounds,” the MCP server repeatedly executed SPARQL queries against the RDF Portal in an attempt to extract such media, but it did not seem to return a clear answer. We then suggested the use of ChEBI, because compounds in ChEBI are annotated with categories such as “inorganic compounds,” which enables discrimination of inorganic from organic substances. With this additional hint, the MCP server was ultimately able to extract the appropriate media. Among the microorganisms that can grow in these media, we identified two strains that were not explicitly described as chemolithoautotrophic in the databases, and that were also absent from our separately curated list of known chemolithoautotrophic organisms.
This example demonstrates the potential of combining natural language interaction with ontology-based reasoning to reveal previously overlooked microbial candidates. Such an approach could significantly accelerate the discovery and annotation of novel functional traits across large-scale biological databases.

Finally, the DSMZ endpoints were added to YummyData, where they are now subject to ongoing monitoring.

# Discussion

The work confirms that semantic publication of microbial databases via RDF is feasible using lightweight configuration approaches. The reusability of the RDF schema files suggests that similar strategies can be extended to other biomedical or microbial datasets. Natural language interfaces show potential to lower barriers for users unfamiliar with SPARQL, but require more development in mapping domain vocabulary, disambiguation, and feedback loops to handle failure modes. Embedding endpoints in monitoring infrastructures like YummyData contributes to sustainability, by making service issues visible and pushing for best practices.

Limitations observed include variability in data quality and metadata completeness, uneven alignment with external ontology terms, and performance issues under complex or large queries. Future work should include extending RDF publication to additional DSMZ data collections, improving schema alignment (e.g., stronger use of OBO ontologies, mapping to external vocabularies), refining natural language → SPARQL translation tools, and optimizing endpoint performance.

## Acknowledgements

We thank the DBCLS BioHackathon 2025 organizers for hosting the event and providing support. We acknowledge the contributions of all team members and collaborators who participated in discussions and testing.
