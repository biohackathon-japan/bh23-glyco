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

GlyTouCan is the international glycan repository which assigns unique accession numbers to glycans; it serves an important role in the interoperability of glycan-related databases and Web resources.
GlyCosmos is a Web portal for glycoscience data, using semantic Web technologies to integrate heterogeneous data related to glycans.  It currently contains information about glycogenes, glycoproteins,
glycolipids, pathways, and diseases, in addition to providing various tools for glycan analysis.
PubChem is a chemical compound database ...[Evan]
UniProt is a protein database, providing the majority of data using semantic Web technologies. [Jerven]

The tasks of BH23 for the Glyco team were laid out as follows.
1. Integrate the glycan data in GlyTouCan and PubChem [Evan]
This involves the analysis of the glycan structures and the chemical representation of data in PubChem.

2. Integration of glycan data from GlyCosmos with UniProt [Jerven]

3. Investigation of glycogene variants and phenotypes to integrate with GlyCosmos
This involves the investigation of variants and phenotypes in the current life science database landscape.  The glycogenes in GlyCosmos are managed using HGNC symbols and NCBI Gene IDs.
So resources that could easily provide variants and phenotypes for a list of such genes would be the strongest candidates.  Comprehensiveness and accuracy are also important factors.

4. Update of Glyco-tools 
This involves updating of software used by the glycomics community.


# Outcomes

## Integration of glycan data from GlyTouCan with PubChem

### update GlyTouCan data

In order to smooth integration, we decided to commence with the forthcoming release of the GlyTouCan data. Then, we updated the GlyTouCan data by utilizing the latest WURCSFramework.

### Exhaustive analysis for PubChem structures to detect sugars using MolWURCS and Sugar'n'Splice
To integrate glycan data between PubChem and GlyTouCan, we have developed a software tool, MolWURCS, for extracting sugars, including monosaccharides and glycans, from chemical structure fomulae as WURCS. In this time, we performed exhaustive analysis for all data in PubChem and GlyTouCan using MolWURCS. To complement and compare the result of the sugar detection, we also used the result of Sugar'n'Splice which detects not only sugars but also the other biopolymers including nucleic acids and amino acids.

The analysis can be separated into the followings:
- MW vs SNS: Comparison of the results between MolWURCS (MW) and Sugar'n'Splice (SNS)
  - To updating the accuracy of sugar detection
- Identifying and analyzing failure cases in conversion between WURCS and MOL/SDF in PubChem and GlyTouCan
  - To improve MolWURCS performance

## Integration of glycan data from GlyCosmos with UniProt

UniProt has a long running curation effort arround glyco binding sites. However, these glycan binding sites do not deep link 
into glycomics resources such as GlyToucan etc. We demonstrated that glycosmos may be used to enrich UniProt glyco site (glycosylation annotations)
by mapping these on the fly taking into consideration the equivalent evidence source in UniProt and Glycosmos. In practice this means when the same glycan binding site is annotated in both GlyCosmos and in UniProt information from the same PubMed source they are considereded equivalent.

This mapping is done on the fly using a single SPARQL query and can be used from either the [UniProt sparql](https://sparlq.uniprot.org) endpoint or the GlyCosmos one.

## Investigation of glycogene variants and phenotypes to integrate with GlyCosmos
Three resources were evaluated for consideration of the criteria for integrating with the glycogene data in GlyCosmos.  Namely, being able to input HGNC and NCBI Gene IDs, being able to obtain variants and phenotype data, comprehensiveness and accuracy.
1. Human Phenotype Ontology
2. The Monarch Initiative
3. TogoVar

## Update of Glyco-tools

* GlycanBuilder2, a glycan structure drawing tool, and GlycoWorkbench, a semi-automatic interpretation and annotation tool for glycan mass spectra, have a problem that they do not work on AppleSilicon-based Mac. Since this problem was caused by the libraries used, I was able to get it to work by compiling on x86_64 and using Rosetta.
* In addition, by preparing the application in .app format, the software can be started with a double-click of the mouse without using the command line.

![image](https://github.com/biohackathon-japan/bh23-glyco/assets/2530360/79f94c14-f868-4690-a8aa-e5dfed50b716)

## Enabling to show upper anatomical concepts at GlyCosmos
GlyCosmos enables its users to search for glycans, and the result shows information regarding the hit glycans including tissues from which they are sampled.
Uber-anatomy ontology (Uberon) provides an integrated cross-species anatomy ontology where anatomical concepts such as heart are defined in the conceptually hierarchical structure using the RDFS vocabularies.
Therefore, if the user gets a result of a search that includes a glycan sampled from a heart, GlyCosmos can show that the glycan is sampled from a primary circulatory organ, too since it is a conceptually upper concept of heart.
This functionality is enabled by using the reasoning function defined by the RDFS specification and implemented in several RDF triple stores such as Virtuoso.
Having surveyed Uberon that has a common ancestry concept of material anatomical entity (UBERON:0000465) among ones used in GlyCosmos as tissues from which glycans are sampled, we obtained its subset where that concept is the root one.

___
![Caption for BioHackrXiv logo figure](./biohackrxiv.png)

# Future work

## Annotating entries in PubChem that contain glycans

## Integration of variant data into GlyCosmos

## Acknowledgements

We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project. We are grateful to the organizers for providing this platform and the developers of open source language models. Special thanks to our mentors, advisors, and colleagues for their guidance and support. Without their contributions, our project in linked data standardization with LLMs in bioinformatics would not have been possible.

## References

1.[@citesAsAuthority:Fujita2021]

2.[@citesAsAuthority:UniProtConsortium2021]
