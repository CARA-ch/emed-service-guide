# ITI-18

The [ITI-18 (Registry Stored Query)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html) transaction allows a [Document Consumer](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.2) to query a [Document Registry](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.2), using [Registry Stored Queries](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1).

This transaction has been implemented in order to respect the Swiss EPR regulation. However, it is very generic, and [CH:PHARM-1](chpharm1.md) should be favored, as it offers emedication-specific query parameters.

See also [EPD by example's Registry Stored Query](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/AuthenticateUser.md) page.

## Stored queries
### Generic rules specific to the eMedication service
* The folders option is not supported.
* The only [association](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2) supported is [RPLC (replace)](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#t4.2.2-1).
* Documents with `deletionStatus=deletionRequested` are ignored.
* Objects belonging to a patient other than the patient of the provided XUA token are ignored.


### Common query parameters

* `$MetadataLevel`: If present, the attribute shall be equal to `1`, as per [*Nationale Anpassungen der Integrationsprofile nach Artikel 5 Absatz 1 Buchstabe b EPDV-EDI*].
<!--* `homeCommunityId`: If present, it shall be [CARA's community root OID](oids.md). <!--TODO replace by emedication service root oid?-->

### FindDocuments

This stored query allows to search for APPC and CH EMED EPR documents. CH EMED EPR documents could also be searched through the [PHARM-1](Transactions/PHARM-1) transaction (it's a specialized ITI-18 transaction). The parameters are matched against the corresponding `XDSDocumentEntry` metadata attributes of the documents in the registry.

All the [parameters defined in the IHE profile](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.7.1) are supported:

| Parameter Name                                | Required | Remarks               |
| --------------------------------------------- | -------- | --------------------- |
| `$XDSDocumentEntryPatientId`                  | yes      | The patient's PMP-PID |
| `$XDSDocumentEntryClassCode`                  | no       | See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.5, for possible values |
| `$XDSDocumentEntryTypeCode`                   | no       | See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.6, for possible values|
| `$XDSDocumentEntryPracticeSettingCode`        | no       | See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.4, for possible values |
| `$XDSDocumentEntryCreationTimeFrom`           | no       | see [dates processing](date_processing.md) |
| `$XDSDocumentEntryCreationTimeTo`             | no       | see [dates processing](date_processing.md) |
| `$XDSDocumentEntryServiceStartTimeFrom`       | no       | Ignored for APPC documents. see [dates processing](date_processing.md) |
| `$XDSDocumentEntryServiceStartTimeTo`         | no       | Ignored for APPC documents. see [dates processing](date_processing.md) |
| `$XDSDocumentEntryServiceStopTimeFrom`        | no       | Ignored for APPC documents. see [dates processing](date_processing.md) |
| `$XDSDocumentEntryServiceStopTimeTo`          | no       | Ignored for APPC documents. see [dates processing](date_processing.md) |
| `$XDSDocumentEntryHealthcareFacilityTypeCode` | no       | See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.3, for possible values |
| `$XDSDocumentEntryEventCodeList`              | no       | See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.8, for possible values |
| `$XDSDocumentEntryConfidentialityCode`        | no       | All documents in the eMedication service are set to `Normal`. See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.8 |
| `$XDSDocumentEntryAuthorPerson`               | no       | -                     |
| `$XDSDocumentEntryFormatCode`                 | no       | See the [ITI-41 section in this guide](iti41.md#metadata-codes-per-document-type) |
| `$XDSDocumentEntryStatus`                     | yes      | -                     |
| `$XDSDocumentEntryType`                       | no       | If empty, `Stable` will be assumed |
| `$XDSDocumentEntryDocumentAvailability`       | no       | All documents in the eMedication service are set to `Online` |

### FindSubmissionSets

This stored query allows to search for submission sets in the eMedication service registry. Query parameters are matched against the corresponding submission set metadata attributes in the registry. All the [parameters defined in the IHE profile](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.7.2) are supported.

| Parameter Name                        | Required | Remarks               |
| ------------------------------------- | -------- | --------------------- |
| `$XDSSubmissionSetPatientId`          | yes      | The patient's PMP-PID |
| `$XDSSubmissionSetSourceId`           | no       | -                     |
| `$XDSSubmissionSetSubmissionTimeFrom` | no       | see [dates processing](date_processing.md) |
| `$XDSSubmissionSetSubmissionTimeTo`   | no       | see [dates processing](date_processing.md) |
| `$XDSSubmissionSetAuthorPerson`       | no       | -                     |
| `$XDSSubmissionSetContentType`        | no       | All submission sets in the eMedication service are of `Procedure` content type. See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.15 |
| `$XDSSubmissionSetStatus`             | yes      | All submission sets in the eMedication service are `Approved` |


### FindFolders

This query is not supported by the eMedication service.

### GetAll

This stored query allows search for all registry content for a given patient and returns:

- `XDSSubmissionSet` and `XDSDocumentEntry` objects matching the query parameters.
- `Association` objects with `sourceObject` or `targetObject` attribute matching one of the above objects.

All the [parameters defined in the IHE profile](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.7.4) are supported.

| Parameter Name                         | Required | Remarks               |
| -------------------------------------- | -------- | --------------------- |
| `$patientId`                           | yes      | The patient's PMP-PID |
| `$XDSDocumentEntryStatus`              | yes      | -                     |
| `$XDSSubmissionSetStatus`              | yes      | All submission sets in the eMedication service are `Approved` |
| `$XDSFolderStatus`                     | yes      | Ignored by the eMedication service |
| `$XDSDocumentEntryFormatCode`          | no       | See the [ITI-41 section in this guide](iti41.md#metadata-codes-per-document-type) |
| `$XDSDocumentEntryConfidentialityCode` | no       | All documents in the eMedication service are set to `Normal`. See the [Annex 3 of the EPR Act](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf), section 2.8 |
| `$XDSDocumentEntryType`                | no       | If empty, `Stable` will be assumed |
| `$XDSAssociationStatus`                | no       | Associations in the eMedication service can only be `Approved`. If empty, `Approved` will be assumed |

### GetDocuments

This stored query allows to fetch a collection of `XDSDocumentEntry` objects. All the [IHE profile parameters](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.7.5) are supported.

| Parameter Name               | Required | Remarks               |
| ---------------------------- | -------- | --------------------- |
| `$XDSDocumentEntryEntryUUID` | no<sup>*</sup> | - |
| `$XDSDocumentEntryUniqueId`  | no<sup>*</sup> | - |
| `$homeCommunityId`           | no       | - |

<sup>*</sup> Either `$XDSDocumentEntryEntryUUID` or `$XDSDocumentEntryUniqueId` shall be specified. This transaction shall return an `XDSStoredQueryParamNumber` error (see [error codes](#error-codes)) if both parameters are specified.

### GetFolders

The response is empty, as folders are not implemented. A warning is added to the response.

### GetAssociations


### GetDocumentsAndAssociations


### GetSubmissionSets


### GetSubmissionSetAndContents


### GetFolderAndContents

The response is empty, as folders are not implemented. A warning is added to the response.

### GetFoldersForDocument

The response is empty, as folders are not implemented. A warning is added to the response.

### GetRelatedDocuments


### FindDocumentsByReferenceId

The response is empty, as this option is not implemented. A warning is added to the response.


## Error codes

Specific codes not covered by generic codes.

| XDS error code               | Details                                             |
| ---------------------------- | --------------------------------------------------- |
| `XDSStoredQueryMissingParam` | If a required search parameter is missing           |
| `XDSStoredQueryParamNumber`  | If too many/too much search parameters are provided |
| `XDSUnknownStoredQuery`      | If the search query is unknown                      |
| `XDSRegistryError`           | If the search query is known but unsupported        |
| `XDSRegistryMetadataError`   | If $MetadataLevel is not "1"                        |
