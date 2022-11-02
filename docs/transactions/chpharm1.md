# CH:PHARM-1: Query Pharmacy Documents

CH:PHARM-1 (Query Pharmacy Documents) is an extension of PHARM-1, a transaction defined by IHE Pharmacy in [the CMPD profile](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf), §3.1.

The eMedication service doesn't support CMA documents and these won't be present in any response.

## Stored queries

CH:PHARM-1 defines a new stored query, _FindMedicationCard_.

### FindMedicationTreatmentPlans

Implemented, as per IHE specs.

### FindPrescriptions

Implemented, as per IHE specs.

**Query parameters**

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindDispenses

Implemented, as per IHE specs.

**Query parameters**

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindMedicationAdministrations

🚫 Not implemented, CMA documents are not supported.

### FindPrescriptionsForValidation

❓ Return only prescriptions that are temporary?

**Query parameters**

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindPrescriptionsForDispense

Return only prescriptions that are actually "open|valid" (i.e. dispensable).

**Query parameters**

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.

### FindMedicationList

Only documents that are linked to an MTP are considered in this query.

**Query parameters**

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.

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

1. *$XDSDocumentEntryPatientId*: The patient XAD-PID.
2. *$XDSDocumentEntryStatus*: TODO
3. *$XDSDocumentEntryFormatCode*: TODO
4. *$XDSDocumentEntryLanguageCode*: the language that will be used to generate the medication card. If not specified, the eMedication service uses the default language (french).

  | Supported language | code    |
  | ------------------ | ------- |
  | Français (default) | `fr-CH` |
  | Deutsch            | `de-CH` |
  | Italiano           | `it-CH` |
  | English            | `en`    |

5. *$XDSDocumentEntryServiceStartFrom*, *$XDSDocumentEntryServiceStartTo*: TODO
6. *$XDSDocumentEntryServiceEndFrom*, *$XDSDocumentEntryServiceEndTo*: TODO
7. *$XDSDocumentEntryType*: stable and/or on-demand. Stable documents are PML documents that have been generated and saved, on-demand are documents that can be generated. There's no stable PML in the eMedication service (the response is empty for requests that only specify stable types). If absent, all types are returned. A single on-demand document is returned.
