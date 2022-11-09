# CH:PHARM-1: Query Pharmacy Documents

CH:PHARM-1 (Query Pharmacy Documents) is an extension of PHARM-1, a transaction defined by IHE Pharmacy in [the CMPD profile](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf), §3.1.

The eMedication service doesn't support CMA documents and these won't be present in any response.

## Stored queries

CH:PHARM-1 defines a new stored query, _FindMedicationCard_.

**Common query parameters**

* *$MetadataLevel*: If present, the attribute shall equal to "1", as per *Nationale Anpassungen der Integrationsprofile nach Artikel 5 Absatz 1 Buchstabe b EPDV-EDI*.
* *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindMedicationTreatmentPlans

Implemented, as per IHE specs.

### FindPrescriptions

Implemented, as per IHE specs.

**Query parameters**

* *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindDispenses

Implemented, as per IHE specs.

### FindMedicationAdministrations

🚫 Not implemented, CMA documents are not supported.

### FindPrescriptionsForValidation

❓ Return only prescriptions that are temporary?

### FindPrescriptionsForDispense

Return only prescriptions that are actually "open|valid" (i.e. dispensable).

### FindMedicationList

Only documents that are linked to an MTP are considered in this query.

### FindMedicationCard (🇨🇭)

This transaction is a Swiss extension and is copied from the _FindMedicationList_ stored query.

**Stored query ID**: `urn:uuid:a8fc04c1-5fb0-45a9-bc59-7a59958beb38`

**Query parameters**

| Parameter Name                    | Attribute                     | Opt | Mult |
| --------------------------------- | ----------------------------- | --- | ---- |
| $XDSDocumentEntryPatientId        | XDSDocumentEntry.patientId    | R   | --   |
| $XDSDocumentEntryStatus           | XDSDocumentEntry.objectType   | R   | M    |
| $XDSDocumentEntryFormatCode       | XDSDocumentEntry.formatCode   | O   | M    |
| $XDSDocumentEntryLanguageCode     | XDSDocumentEntry.languageCode | O   | --   |
| $XDSDocumentEntryServiceStartFrom | N/A                           | O   | --   |
| $XDSDocumentEntryServiceStartTo   | N/A                           | O   | --   |
| $XDSDocumentEntryServiceEndFrom   | N/A                           | O   | --   |
| $XDSDocumentEntryServiceEndTo     | N/A                           | O   | --   |
| $XDSDocumentEntryType             | N/A                           | O   | M    |


* *$XDSDocumentEntryStatus*: TODO
* *$XDSDocumentEntryFormatCode*: this parameter is used to determine what will be the type of the generated medication card:

  | Supported format code                      | Processing                                     |
  | ------------------------------------------ | ---------------------------------------------- |
  | `urn:che:epr:ch-emed:medication-card:2022` | The medication card is a CH-EMED-EPR document. |
  | `urn:che:epr:EPR_Unstructured_Document`    | The medication card is a PDF document.         |


* *$XDSDocumentEntryLanguageCode*: the language that will be used to generate the medication card. If not specified, the eMedication service uses the default language (french).

  | Supported language | code    |
  | ------------------ | ------- |
  | Français (default) | `fr-CH` |
  | Deutsch            | `de-CH` |
  | Italiano           | `it-CH` |
  | English            | `en`    |

* *$XDSDocumentEntryServiceStartFrom*, *$XDSDocumentEntryServiceStartTo*: TODO
* *$XDSDocumentEntryServiceEndFrom*, *$XDSDocumentEntryServiceEndTo*: TODO
* *$XDSDocumentEntryType*: stable and/or on-demand. Stable documents are PML documents that have been generated and saved, on-demand are documents that can be generated. There's no stable PML in the eMedication service (the response is empty for requests that only specify stable types). If absent, all types are returned. A single on-demand document is returned.
