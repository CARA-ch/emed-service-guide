# Audit Trail Consumption [CH ATC]

The eMedication service supports the retrieval of __patient__ audit trail events, in FHIR format as specified by the [implementation guide](https://fhir.ch/ig/ch-atc/index.html), and in compliance with the [Supplement 2.2 to the Annex 5 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/Anhang_5_Ergaenzung_2.2_EPDV_EDI_20190624.pdf.download.pdf/Anhang%205%20Erg%C3%A4nzung%202.2%20der%20EPDV-EDI_Fassung%20vom%2024.%20Juni%202019.pdf) and itself based on IHE's Retrieve ATNA Audit Event [ITI-81] transaction as specified by the [Add RESTful ATNA (Query and Feed)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_RESTful-ATNA.pdf) supplement to IHE IT Infrastructure Technical Framework.

This transaction is implemented by the eMedication service as specified by the resources linked in the previous paragraph with the following particularities:

- All the query parameters defined by the IHE supplement are supported, but with the following limitations:
    - `address` testing does not take into account modifiers, only case sensitive exact matches.
    - `agent.identifier` testing does not take into account modifiers and is always case sensitive.
    - `patient.identifier` testing is done without taking into account modifiers and it is always case sensitive. Matching does not additionally filter by type: it is assumed that if the identifier matches, that is good enough.
    - `entity.identifier` testing is done without taking modifiers into account and it is always case sensitive. `OR` criteria for this parameter is not supported.
    - `entity.type` testing is done without taking modifiers inot account and it is always case sensitive.
    - `entity.role` testing is done without taking modifiers into account and it is always case sensitive.
    - `source.identifier` testing is done without taking modifiers into account and it is always case sensitive. `OR` criteria for this parameter is not supported.
    - `type` testing is done without taking modifiers into account and it is always case sensitive.
    - `subtype` testing is done without taking modifiers into account and it is always case sensitive.
    - `outcome` testing is done without taking modifiers into account and it is always case sensitive.
    - The [FHIR standard parameters that apply to all resources](http://hl7.org/fhir/R4/search.html#all) (`_content`, `_id`, `_lastUpdated`) are not supported.
    - [FHIR search parameters](http://hl7.org/fhir/R4/search.html#return) are not supported.
    