---
title: 'BioHackJP 2023 Report R4: integration of glyco data with chemo-, geno-, lipid-omics and pathway data'
title_short: 'BioHackJP 2023 Glyco'
tags:
  - Glycoscience
  - Chemoinformatics
  - Proteins
  - Lipids
  - Gene variants
authors:
  - name: Kiyoko F. Aoki-Kinoshita
    orcid: 0000-0002-6662-8015
    affiliation: 1
  - name: Evan Bolton
    orcid: 0000-0002-5959-6190
    affiliation: 2
  - name: Issaku Yamada
    orcid: 0000-0001-9504-189X
    affiliation: 3
  - name: Akihiro Fujita
    orcid: 0000-0003-3748-7791
    affiliation: 1
  - name: Masaaki Matsubara
    orcid: 0000-0001-6809-1568
    affiliation: 3
  - name: Jerven Bolleman
    orcid: 0000-0002-7449-1266
    affiliation: 4
  - name: Yasunori Yamamoto
    orcid: 0000-0002-6943-6887
    affiliation: 5
affiliations:
  - name: Soka University
    index: 1
  - name: National Center for Biotechnolog Information (NCBI), PubChem
    index: 2
  - name: The Noguchi Institute
    index: 3
  - name: SIB Swiss Institute of Bioinformatics
    index: 4
  - name: Database Center for Life Science
    index: 5
date: 30 June 2023
cito-bibliography: paper.bib
event: BH23JP
biohackathon_name: "BioHackathon Japan 2023"
biohackathon_url:   "https://2023.biohackathon.org/"
biohackathon_location: "Kagawa, Japan, 2023"
group: R4
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh23-glyco
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Kiyoko F. Aoki-Kinoshita \emph{et al.}
---

# Background

Glycans, or carbohydrate sugar chains, are branched biopolymers of monosaccharides, often found attached to proteins and lipids on the cell surface.  They are found on virtually every cell in the body.
They are biosynthesized via a complex interaction of multiple enzymes (glycosyltransferases, glycosidases, sulfotransferases, etc.) which, together with sugar nucleotides, generate a wide variety of
glycans that are co-translationally attached to proteins and lipids.  These glycoproteins and glycolipids (together called glycoconjugates) then reach the cell surface to often act as receptors for
glycan-binding proteins (lipids), viruses, bacteria, etc.  They may also be secreted into the extracellular matrix.


Because there is no sequencing technology for glycans, the glycome is characterized using technologies such as mass spectrometry (MS) and nuclear magnetic resonance (NMR).  MS is a widely used technology
now, but only produce monosaccharide compositions of glycans at the lowest level of detail.  

GlyTouCan (Fujita et al., 2021) is the international glycan repository which assigns unique accession numbers to glycans; it serves an important role in the interoperability of glycan-related databases and Web resources.
GlyCosmos (Yamada et al., 2020)] is a Web portal for glycoscience data, using semantic Web technologies to integrate heterogeneous data related to glycans.  It currently contains information about glycogenes, glycoproteins,
glycolipids, pathways, and diseases, in addition to providing various tools for glycan analysis.
PubChem (Kim et al., 2023) is an open repository for chemical substances and their biological activities.  It is provided by the National Center for Biotechnology Information, which is part of the National Library of Medicine, an institute of the National Institutes of Health located in the United States of America.  PubChem contains more than 300 million chemical substance descriptions, and a similar number of biological activities.  PubChem contents are provided by more than 925 data contributors and, with millions of monthly users, represents a key resource for scientific researchers.
UniProt (UniProtConsortium, 2021) is a protein database, providing the majority of data using semantic Web technologies.

The tasks of BH23 for the Glyco team were laid out as follows.
1. Integrate the glycan data in GlyTouCan and PubChem

This involves the analysis of the glycan structures and the chemical representation of data in PubChem.  The MolWURCS and SugarNSplice applications are to be used to determine and isolated glycan containing structures in PubChem using a custom made workflow.  Comparison of the MolWURCS and SugarNSplice output will help to enhance agreement of "what is a glycan?" for future efforts.  Key to this is determining appropriate glycans to put into GlyTouCan.

2. Integration of glycan data from GlyCosmos with UniProt.

While glycosylation site annotations do exist in UniProt, they are simple text.  The glycans binding to many of these sites are already known, but there are no links to the glycan structures (GlyTouCan IDs).  GlyCosmos provides this information in RDF,so it should be possible to re-assess the glycosylation annotations. 

3. Investigation of glycogene variants and phenotypes to integrate with GlyCosmos

This involves the investigation of variants and phenotypes in the current life science database landscape.  The glycogenes in GlyCosmos are managed using HGNC symbols and NCBI Gene IDs.
So resources that could easily provide variants and phenotypes for a list of such genes would be the strongest candidates.  Comprehensiveness and accuracy are also important factors.

4. Update of Glyco-tools

This involves updating software used by the glycomics community.

5. Semantic inferencing to enhance the knowledge in GlyCosmos.

This involves organizing the ontologies used in GlyCosmos to enable inferencing, and incorporation of inferencing rules to generate new knowledge from existing data.

# Outcomes

## Integration of glycan data from GlyTouCan with PubChem

### Update GlyTouCan data
In order to enable smooth integration, we commenced with the release of the GlyTouCan data. We updated the GlyTouCan data by utilizing the latest [WURCSFramework](https://gitlab.com/glycoinfo/wurcsframework).

### Exhaustive analysis for PubChem structures to detect sugars using MolWURCS and Sugar'n'Splice
To integrate glycan data between PubChem and GlyTouCan, we have developed a software tool, MolWURCS, for extracting sugars, including monosaccharides and glycans, from chemical structure formulae.  These sugars are extracted in WURCS format (Matsubara et al., 2017). During the biohackathon, we performed exhaustive analysis for all data in PubChem and GlyTouCan using MolWURCS. To complement and compare the result of the sugar detection, we also used the result of Sugar'n'Splice which detects not only sugars but also the other biopolymers including nucleic acids and amino acids.

After the sugar detection using MolWURCS across all of 115,068,739 PubChem entries, sugars could be detected from 1,236,644 entries, and 354,183 unique sugars (WURCSs) were extracted. On the other hand, the sugar detection using Sugar'n'Splice indicated that 949,636 entries contain sugars and that 93,106 entries contain sugars in terms of biologics. Through this analysis, it was confirmed by both MolWURCS and Sugar'n'Splice-biologic that 86,732 entries contained sugars.

## Integration of glycan data from GlyCosmos with UniProt

UniProt has a long running curation effort around glycosylation (glycan-binding) sites on protein sequences. However, these glycan binding sites do not deeply link
into glycomics resources such as GlyTouCan, etc. We demonstrated that GlyCosmos may be used to enrich UniProt glycosylation sites (glycosylation annotations)
by mapping these on-the-fly taking into consideration the equivalent evidence source in UniProt and GlyCosmos. In practice this means when the same glycosylation site is annotated in both GlyCosmos and in UniProt information from the same PubMed source they are considered equivalent.

This mapping is done on the fly using a single SPARQL query and can be used from either the [UniProt sparql](https://sparlq.uniprot.org) or GlyCosmos endpoint.

## Investigation of glycogene variants and phenotypes to integrate with GlyCosmos
Three resources were evaluated for consideration of the criteria for integrating with the glycogene data in GlyCosmos.  Namely, being able to input HGNC and NCBI Gene IDs, being able to obtain variants and phenotype data, comprehensiveness and accuracy.

1. Human Phenotype Ontology (HPO)
  - HPO has a comprehensive list of phenotypes available at https://hpo.jax.org/app/data/annotations.
  - There is also a downloadable list of [genes to phenotypes](http://purl.obolibrary.org/obo/hp/hpoa/genes_to_phenotype.txt) on the same page.  This file provides a link between genes and HPO terms. All phenotype terms associated with any disease that is associated with variants in a gene are assigned to that gene in this file.
  * This file was in the exact format we were looking for, so we started incorporating this information into the GlyCosmos glycogene entry pages and disease entry pages.
2. The Monarch Initiative
  - This initiative provides an integrative data and analytic platform connecting phenotypes to genotypes across species, bridging basic and applied research with semantics-based analysis. The correlation of phenotypic outcomes and disease with genetic variation and environmental factors is a core pursuit in biology and biomedicine.  Thus, the data was very important to take into consideration.
  - The user interface is easy to use and one can obtain variants, diseases, etc. for any gene specified in any format.
  - The RDF data could be downloaded from their homepage, but there were many files, and an API or SPARQL endpoint was preferred.
  * While the data was very rich, we could not find an easy way to obtain a list of variants and phenotypes from a list of genes during the hackathon.
3. TogoVar
  - TogoVar (Mitsuhashi et al, 2022) is a resource providing an integrated view of variants and clinical significance and outcome data from a variety of resources, including Japanese cohorts.  
  - The user interface, while slow at times, could be used to easily obtain variants and outcomes from any gene specified in any format.
  - The [API](https://grch38.togovar.org/api) provided could also be used in a sequential manner.  (1) Obtain the ID for a gene symbol, (2) obtain the variants for each ID, and (3) obtain phenotypes for any variant.
  - A SPARQL endpoint was also available, but the schema describing the endpoint data needed to be updated.
  * A simple Python script could be developed to perform the sequence above for individual genes, but bulk processing options were unavailable.

## Update of Glyco-tools

* GlycanBuilder2, a glycan structure drawing tool, and GlycoWorkbench, a semi-automatic interpretation and annotation tool for glycan mass spectra, have a problem that they do not work on AppleSilicon-based Mac. Since this problem was caused by the libraries used, I was able to get it to work by compiling on x86_64 and using Rosetta.
* In addition, by preparing the application in .app format, the software can be started with a double-click of the mouse without using the command line.

![image](https://github.com/biohackathon-japan/bh23-glyco/assets/2530360/79f94c14-f868-4690-a8aa-e5dfed50b716)

## Semantic inferencing to enhance the knowledge in GlyCosmos
At this biohackathon, we focused on enabling the display of higher anatomical concepts in GlyCosmos when referring to anatomical information such as heart tissues, etc.
GlyCosmos enables users to search for glycans, and the result shows information regarding the given glycans including tissues from which they are sampled.
Uber-anatomy ontology (Uberon) provides an integrated cross-species anatomy ontology where anatomical concepts such as heart are defined in the conceptually hierarchical structure using the RDFS vocabularies.
Therefore, if the user obtains a result of a search that includes a glycan sampled from a heart, GlyCosmos can show that the glycan is sampled from a primary circulatory organ, too, since it is a conceptually higher concept of heart.
This functionality is enabled by using the reasoning function defined by the RDFS specification and implemented in several RDF triple stores such as Virtuoso.
Having surveyed Uberon that has a common ancestry concept of material anatomical entity (UBERON:0000465) among ones used in GlyCosmos as tissues from which glycans are sampled, we obtained its subset where that concept is the root one.

_____

# Future work

## Annotating entries in PubChem that contain glycans
During this BioHackathon, we focused on matching glycans with those in PubChem and mapping between exactly matching structures.
However, many entries in PubChem contain glycans as a portion of them. At times, multiple glycans may be contained within the same entry in PubChem, connected by linkers or other aglycones.
MolWURCS is able to extract the glycan portion of such data.  By adding annotations indicating the GlyTouCan ID and linking to GlyCosmos into PubChem entries that have contain such glycan information, the following use cases could be addressed.  
1. How is the glycan inside this glycoconjugate synthesized?
2. What other biopolymers does the glycan in this compound bind to?  i.e., what other core proteins does this glycan glycosylate?
3. What lectins recognize this glycan on this glycoconjugate?


## Integration of variant data into GlyCosmos
GlyCosmos releases updates of the data and user interface every four months.  In order to update the variants and phenotype data, the latest data of each resource evaluated would need to be retrieved.
HPO data could be easily obtained via their homepage.
The Monarch Initiative data need to be further assessed and an easy system for obtaining a list of variants and phenotypes from a list of genes will need to be developed.
They also provide an API called [BioLink](https://api.monarchinitiative.org/api/) which could be of use in developing this system.
Once the TogoVar schema is updated, scripts using SPARQL could be developed to perform the sequence of gene -> id -> variant -> phenotype in bulk for all glycogenes in GlyCosmos.
Moreover, the orthologous mouse variants linked during this biohackathon can also be integrated.  This would be of great utility for glycoscientists who wish to experiment on such variants to study their effects.

## Incorporation of WikiPathways
We are developing a glyc-related pathway registration system called GlycoPathwayRepo.  Part of this system could utilize WikiPathways and the PFOCR functionality developed at this biohackathon.  Therefore, this will be evaluated for integration into GlycoPathwayRepo.

## Acknowledgements
We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project.
We would also like to acknowledge Tamiko Ono and Masaaki Shiota of Soka University for their collaboration remotely during the biohackathon.
We are grateful to the organizers for providing this platform and a great environment for collaboration and camaraderie.

## References
 
