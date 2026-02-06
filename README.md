# OPPO is an ontology that captures fine-grained data practices in the privacy policies of online social networks.

This ontology is a domain specific ontology that aims at capturing detailed data practices descibed in OSNs' privacy policies. 

The OPPO mailing lists are as follows: sanonda.datta.gupta@drexel.edu and torsten.hahmann@maine.edu. Feel free to reach out if you have any queries. 

The development version of OPPO (OPPO_Ontology_Full_Import.owl (or OPPO_Ontology_Full_Import.ttl)) imports all ontologies/files present in the OPPO-Ontology/Ontology directory. It includes the following ontologies:

- BFO.owl
- OPPO-IAO-Ontology.ttl
- OPPO-OBI-Ontology.ttl
- OPPO-OMRSE-Ontology.ttl
- dpvo-Ontology.ttl

The OPPO ontology has also been extended to map GDPR's sensitive data with respect to OPPO's personal data category which can be found in OPPO-GDPR-Extension_Core.ttl.

OPPO was evaluated by creating a dataset from Telegram's privacy policy. The Competency questions and its corresponding SPARQL query will be found in [CQ and SPARQL Query](https://github.com/SanondaDattaGupta/OPPO-Ontology/blob/main/OPPO-Evaluation/CQ%20and%20Sparql%20Query.md).

<u>It is to be noted that,  downloading the (OPPO_Ontology_Full_Import.owl (or OPPO_Ontology_Full_Import.ttl)) file and opening it through Protege may result in 19000 concepts as it does not try to import the other files which are smaller and only includes concepts which are related to OPPO.</u>

`` All validations using external tools (e.g., OOPS! and RDF validators) were conducted on the OPPO_Ontology_Minimal_Import_RDF file, as the full ontology caused performance limitations in web-based validation tools. To get a better impression of the ontology or to casually explore the core concepts of the OPPO, please refer to the OPPO_Ontology_Minimal_Import.ttl.``

## License

The OPPO ontology is licensed under the Creative Commons Attribution 4.0 International (CC BY 4.0) License.  
https://creativecommons.org/licenses/by/4.0/

