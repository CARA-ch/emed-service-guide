# CH:PHARM-1: Query Pharmacy Documents

CH:PHARM-1 (Query Pharmacy Documents) is an extension of PHARM-1, a transaction defined by IHE Pharmacy in [the CMPD profile](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf).

The eMedication service doesn't support CMA (Community Medication Administration) documents and these won't be present in any response.

## Stored queries

In relatio to IHE's PHARM-1, CH:PHARM-1 defines three additional queries: `FindMedicationCard`, `FindPrescriptionsForValidation` and `FindPrescriptionsForDispense`.

## Common query parameters

The following parameters are supported by all the PHARM-1 (and CH:PHARM-1) queries received by the eMedication service:

* `$XDSDocumentEntryPatientId` (required): the patient's PMP-PID.
* `$XDSDocumentEntryService(Start|Stop)Time(From|To)`: the behaviour of these parameters for the PHARM-1 queries implemention of the eMedication service does not match the behaviour of the ITI-18 queries. Instead of matching the parameters against the `DocumentEntry` metadata in the registry, PHARM-1 queries will test the parameters against consolidated treatment data, see each transaction's own section in this page to know exactly how these parameters are interpreted in such cases.
* `$XDSDocumentEntryStatus` (required).

### Find Stable Documents
A subset of the supported PHARM-1 queries allow to recover stable documents. These have the following parameters in common:

* `$XDSDocumentEntryPracticeSettingCode`: See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.4, for possible values.
* `$XDSDocumentEntryHealthcareFacilityTypeCode`: See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.3, for possible values.
* `$XDSDocumentEntryEventCodeList`: See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.8, for possible values.
* `$XDSDocumentEntryConfidentialityCode`: All documents in the eMedication service are set to `Normal`. See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.8
* `$XDSDocumentEntryUniqueId`.
* `$XDSDocumentEntryEntryUUID`.
* `$XDSCreationTime(From|To)`: see [date processing](date_processing.md). These are matched agains the available `DocumentEntry` metadata in the registry.
* `$XDSAuthorPerson`.

Either `$XDSDocumentEntryEntryUUID` or `$XDSDocumentEntryUniqueId` shall be specified.

#### FindMedicationTreatmentPlans

Implemented, as per IHE specs.

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the treatments' __consolidated__ start and end times:

- The consolidated start date of returned treatments must be between the specified `$XDSDocumentEntryServiceStartFrom` and `$XDSDocumentEntryServiceStarTo` parameters:
    - If `$XDSDocumentEntryServiceStartFrom` is not specified, any treatment starting before the specified `$XDSDocumentEntryServiceStarTo` will meet the service start criterium.
    - If `$XDSDocumentEntryServiceStartTo` is not specified, any treatment starting at or after the specified `$XDSDocumentEntryServiceStartFrom` will meet the service start criterium.
    - If neither `$XDSDocumentEntryServiceStartFrom` nor `$XDSDocumentEntryServiceStartTo` are specified, all treatments starting at any date will meet the service start criterium.
    - Note that if a treatment does not have an explicit start dat eprovided by a published document, it is assumed to be the date of creation fo the MTP document.
- The consolidated end date of returned treatments must be between the specified `$XDSDocumentEntryServiceEndFrom` and `$XDSDocumentEntryServiceEndTo` parameters.
    - If `$XDSDocumentEntryServiceEndFrom` is not specified, any treatment ending before the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If `$XDSDocumentEntryServiceEndTo` is not specified, any treatment ending at or after the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If neither `$XDSDocumentEntryServiceEndFrom` nor `$XDSDocumentEntryServiceEndTo` are specified, all treatments ending at any date will meet the service end criterium.
    - Note that if a treatment does not have an explicit end date provided by a published document, it is assumed by the aggregator to be _the end of time_, i.e. it will have no end date.

#### FindPrescriptions

Implemented, as per IHE specs. 

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the prescriptions' __consolidated__ validity period:

- The prescription validity start date shall match if:
    - Greater or equal than `ServiceStartFrom` if specified.
    - Lesser than `ServiceStartTo` if specified.
- The prescription validity end date shall match if:
    - Greater or equal than `ServiceEndFrom` if specified.
    - Lesser than `ServiceEndTo` if specified.
- If a prescription does not specify a validity period start date, if is assumed to be the date of medical authorship (i.e. the `authoredOn` field within the CH EMED EPR content).
- If a prescription does not specify a validity period end date, it is assumed to not have an end date and hence any specified `...To` parameter should match.

#### FindDispenses

Implemented, as per IHE specs.

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be all matched against the date of dispense, that is, the `whenHandedOver` field of the CH EMED EPR content.

#### FindMedicationAdministrations

🚫 Not supported.

#### FindPrescriptionsForValidation

Implemented, as per IHE specs.

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the prescriptions' consolidated validity period:

- The prescription validity start date shall match if:
    - Greater or equal than `ServiceStartFrom` if specified.
    - Lesser than `ServiceStartTo` if specified.
- The prescription validity end date shall match if:
    - Greater or equal than `ServiceEndFrom` if specified.
    - Lesser than `ServiceEndTo` if specified.
- If a prescription does not specify a validity period start date, if is assumed to be the date of medical authorship (i.e. the `authoredOn` field within the CH EMED EPR content).
- If a prescription does not specify a validity period end date, it is assumed to not have an end date and hence any specified `...To` parameter should match.

#### FindPrescriptionsForDispense

Implemented, as per IHE specs.
Return only prescriptions that are actually provisional or active (i.e. dispensable).

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the prescriptions' consolidated validity period:

- The prescription validity start date shall match if:
    - Greater or equal than `ServiceStartFrom` if specified.
    - Lesser than `ServiceStartTo` if specified.
- The prescription validity end date shall match if:
    - Greater or equal than `ServiceEndFrom` if specified.
    - Lesser than `ServiceEndTo` if specified.
- If a prescription does not specify a validity period start date, if is assumed to be the date of medical authorship (i.e. the `authoredOn` field within the CH EMED EPR content).
- If a prescription does not specify a validity period end date, it is assumed to not have an end date and hence any specified `...To` parameter should match.

### FindMedicationList

Implemented, as per IHE specs.

Entry selection (via the search query execution) is done at document retrieval time, not search time.

On top of the common PHARM-1 parameters, the following parameters are supported:

- `$XDSDocumentEntryType` (required): If empty, `Stable` will be assumed.
- `$XDSFormatCode`: See the [ITI-41 section in this guide](iti41.md#metadata-codes-per-document-type).

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the treatments' __consolidated__ start and end times:

- The consolidated start date of returned treatments must be between the specified `$XDSDocumentEntryServiceStartFrom` and `$XDSDocumentEntryServiceStarTo` parameters:
    - If `$XDSDocumentEntryServiceStartFrom` is not specified, any treatment starting before the specified `$XDSDocumentEntryServiceStarTo` will meet the service start criterium.
    - If `$XDSDocumentEntryServiceStartTo` is not specified, any treatment starting at or after the specified `$XDSDocumentEntryServiceStartFrom` will meet the service start criterium.
    - If neither `$XDSDocumentEntryServiceStartFrom` nor `$XDSDocumentEntryServiceStartTo` are specified, all treatments starting at any date will meet the service start criterium.
    - Note that if a treatment does not have an explicit start dat eprovided by a published document, it is assumed to be the date of creation fo the MTP document.
- The consolidated end date of returned treatments must be between the specified `$XDSDocumentEntryServiceEndFrom` and `$XDSDocumentEntryServiceEndTo` parameters.
    - If `$XDSDocumentEntryServiceEndFrom` is not specified, any treatment ending before the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If `$XDSDocumentEntryServiceEndTo` is not specified, any treatment ending at or after the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If neither `$XDSDocumentEntryServiceEndFrom` nor `$XDSDocumentEntryServiceEndTo` are specified, all treatments ending at any date will meet the service end criterium.
    - Note that if a treatment does not have an explicit end date provided by a published document, it is assumed by the aggregator to be _the end of time_, i.e. it will have no end date.

### FindMedicationCard (🇨🇭)

This transaction is a Swiss extension. 

**Stored query ID**: `urn:uuid:a8fc04c1-5fb0-45a9-bc59-7a59958beb38`

Treatment selection (via the search query execution) is done at document retrieval time, not search time.

**Query parameters**

On top of the common PHARM-1 parameters, the following parameters are supported:

- `$XDSDocumentEntryType` (required): should be `On-demand`. A query for `Stable` documents only will yield an empty result set.
- `$XDSFormatCode`: this parameter is used to specify whether the query should return only a PDF (`urn:che:epr:EPR_Unstructured_Document`) or a PMLC document as specified in CH EMED EPR (`urn:che:epr:ch-emed:medication-card:2022`), containing the PDF as well as original representation of the FHIR document. If not specified, PMLC document is assumed.
- `$XDSDocumentEntryLanguageCode`: the language that will be used to generate the medication card. If not specified, the eMedication service uses the default language (French). At present, the eMedicationService supports only French language (`fr-CH`).
- `$PMLCIncludeNonActive`: either `true` or `false`. If ommitted, `false` will be assumed. When `false`, the query will return only active treatments (plus the last treatment to be added or modified to the patient's eMedication even if it is no-longer active). When true, all treatments matching the query's criteria will be returned, whether active or not at the moment.
- `$PMLCPaperFormat`: all values from the [CHEMEDEPRPaperFormatCS](http://fhir.ch/ig/ch-emed-epr/CodeSystem/ch-emed-epr-paper-format-code-system) are supported. This parameter, by default `cara-pmp` allows a client to request that PMLC PDF be generated following a specific format. For now only CARA's own format and eMediplan format are supported.

The `$XDSDocumentEntryService(Start|Stop)Time(From|To)` parameters, if present, will be matched against the treatments' __consolidated__ start and end times:

- The consolidated start date of returned treatments must be between the specified `$XDSDocumentEntryServiceStartFrom` and `$XDSDocumentEntryServiceStarTo` parameters:
    - If `$XDSDocumentEntryServiceStartFrom` is not specified, any treatment starting before the specified `$XDSDocumentEntryServiceStarTo` will meet the service start criterium.
    - If `$XDSDocumentEntryServiceStartTo` is not specified, any treatment starting at or after the specified `$XDSDocumentEntryServiceStartFrom` will meet the service start criterium.
    - If neither `$XDSDocumentEntryServiceStartFrom` nor `$XDSDocumentEntryServiceStartTo` are specified, all treatments starting at any date will meet the service start criterium.
    - Note that if a treatment does not have an explicit start dat eprovided by a published document, it is assumed to be the date of creation fo the MTP document.
- The consolidated end date of returned treatments must be between the specified `$XDSDocumentEntryServiceEndFrom` and `$XDSDocumentEntryServiceEndTo` parameters.
    - If `$XDSDocumentEntryServiceEndFrom` is not specified, any treatment ending before the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If `$XDSDocumentEntryServiceEndTo` is not specified, any treatment ending at or after the specified `$XDSDocumentEntryServiceEndTo` will meet the service end criterium.
    - If neither `$XDSDocumentEntryServiceEndFrom` nor `$XDSDocumentEntryServiceEndTo` are specified, all treatments ending at any date will meet the service end criterium.
    - Note that if a treatment does not have an explicit end date provided by a published document, it is assumed by the aggregator to be _the end of time_, i.e. it will have no end date.
