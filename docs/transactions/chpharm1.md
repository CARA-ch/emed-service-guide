# CH:PHARM-1: Query Pharmacy Documents

CH:PHARM-1 (Query Pharmacy Documents) is an extension of PHARM-1, a transaction defined by IHE Pharmacy in [the CMPD profile](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf), §3.1.

The eMedication service doesn't support CMA documents and these won't be present in any response.

## Stored queries

CH:PHARM-1 defines three new stored queries, _FindMedicationCard_, _FindConsolidatedPrescriptionsForValidation_ and _FindConsolidatedPrescriptionsForDispense_.

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

Entry selection (via the search query execution) is done at document retrieval time, not search time.

### FindMedicationCard (🇨🇭)

This transaction is a Swiss extension and is copied from the _FindMedicationList_ stored query.

**Stored query ID**: `urn:uuid:a8fc04c1-5fb0-45a9-bc59-7a59958beb38`

Treatment selection (via the search query execution) is done at document retrieval time, not search time.

**Query parameters**

  | Parameter Name                    | Attribute                     | Opt | Mult |
  | --------------------------------- | ----------------------------- | --- | ---- |
  | $XDSDocumentEntryPatientId        | XDSDocumentEntry.patientId    | R   | --   |
  | $XDSDocumentEntryStatus           | XDSDocumentEntry.objectType   | R   | M    |
  | $XDSDocumentEntryFormatCode       | XDSDocumentEntry.formatCode   | O   | --   |
  | $XDSDocumentEntryLanguageCode     | XDSDocumentEntry.languageCode | O   | --   |
  | $XDSDocumentEntryServiceStartFrom | N/A                           | O   | --   |
  | $XDSDocumentEntryServiceStartTo   | N/A                           | O   | --   |
  | $XDSDocumentEntryServiceEndFrom   | N/A                           | O   | --   |
  | $XDSDocumentEntryServiceEndTo     | N/A                           | O   | --   |
  | $XDSDocumentEntryType             | N/A                           | O   | M    |


  * *$XDSDocumentEntryStatus*: This parameter is not used.
  * *$XDSDocumentEntryFormatCode*: this parameter is used to determine what will be the type of the generated medication card:

    | Supported format code                      | Processing                                                 |
    | ------------------------------------------ | ---------------------------------------------------------- |
    | `urn:che:epr:ch-emed:medication-card:2022` | By default. The medication card is a CH-EMED-EPR document. |
    | `urn:che:epr:EPR_Unstructured_Document`    | The medication card is a PDF document.                     |


  * *$XDSDocumentEntryLanguageCode*: the language that will be used to generate the medication card. If not specified, the eMedication service uses the default language (french).

    | Supported language | code    |
    | ------------------ | ------- |
    | Français (default) | `fr-CH` |
    | Deutsch            | `de-CH` |
    | Italiano           | `it-CH` |
    | English            | `en`    |

    !!! warning

        The service currently only supports french.

  * *$XDSDocumentEntryServiceStartFrom*, *$XDSDocumentEntryServiceStartTo*: This parameter is not used.
  * *$XDSDocumentEntryServiceEndFrom*, *$XDSDocumentEntryServiceEndTo*: This parameter is not used.
  * *$XDSDocumentEntryType*: stable and/or on-demand. Stable documents are PML documents that have been generated and saved, on-demand are documents that can be generated. There's no stable PML in the eMedication service (the response is empty for requests that only specify stable types). If absent, all types are returned. A single on-demand document is returned.

### Draft: FindConsolidatedPrescriptionsForValidation (🇨🇭)

  This transaction is a Swiss extension and is copied from the _FindPrescriptionsForValidation_ stored query.

  **Stored query ID**: `urn:uuid:b4f7e093-d2f1-4278-918e-ada92b55d39e`

  **Query parameters**
  
  | Parameter Name                              | Attribute                             | Opt | Mult |
  | ------------------------------------------- | ------------------------------------- | --- | ---- |
  | $XDSDocumentEntryPatientId                  | XDSDocumentEntry.patientId            | R   | --   |
  | $XDSDocumentEntryEntryUUID                  | XDSDocumentEntry.entryUUID            | O   | M    |
  | $XDSDocumentEntryUniqueId                   | XDSDocumentEntry.uniqueId             | O   | M    |
  | $XDSDocumentEntryPracticeSettingCode        | XDSDocumentEntry. practiceSettingCode | O   | M    |
  | $XDSDocumentEntryCreationTimeFrom           | XDSDocumentEntry.creationTime         | O   | --   |
  | $XDSDocumentEntryCreationTimeTo             | XDSDocumentEntry.creationTime         | O   | --   |
  | $XDSDocumentEntryServiceStartTimeFrom       | N/A                                   | O   | --   |
  | $XDSDocumentEntryServiceStartTimeTo         | N/A                                   | O   | --   |
  | $XDSDocumentEntryServiceStopTimeFrom        | N/A                                   | O   | --   |
  | $XDSDocumentEntryServiceStopTimeTo          | N/A                                   | O   | --   |
  | $XDSDocumentEntryHealthcareFacilityTypeCode |                                       | O   | M    |
  | $XDSDocumentEntryEventCodeList              |                                       | O   | M    |
  | $XDSDocumentEntryConfidentialityCode        | Keep?                                 | O   | M    |
  | $XDSDocumentEntryAuthorPerson               | Keep?                                 | O   | M    |
  | $XDSDocumentEntryStatus                     | No reason to keep it                  | O   | M    |

### Draft: FindConsolidatedPrescriptionsForDispense (🇨🇭)

  This transaction is a Swiss extension and is copied from the _FindPrescriptionsForDispense_ stored query.

  **Stored query ID**: `urn:uuid:c9d8c2d3-69b9-4b5c-9bf8-d40fa7d8bf11`

  **Query parameters**
  