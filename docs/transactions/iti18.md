# ITI-18

The [ITI-18 (Registry Stored Query)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html) transaction allows a [Document Consumer](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.2) to query a [Document Registry](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.2), using [Registry Stored Queries](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1).

This transaction has been implemented in order to respect the Swiss EPR regulation. However, it is very generic, and [CH:PHARM-1](chpharm1.md) should be favored, as it offers emedication-specific query parameters.

See also [EPD by example's Registry Stored Query](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/AuthenticateUser.md) page.

## Stored queries
### Generic rules
* Folders option not supported
* The only [association](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2) supported is [RPLC (replace)](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#t4.2.2-1)
* Documents with `deletionStatus=deletionRequested` are ignored


### Common query parameters

* `$MetadataLevel`: If present, the attribute shall equal to `1`, as per *Nationale Anpassungen der Integrationsprofile nach Artikel 5 Absatz 1 Buchstabe b EPDV-EDI*.
<!--* `homeCommunityId`: If present, it shall be [CARA's community root OID](oids.md). <!--TODO replace by emedication service root oid?-->

### FindDocuments

This stored query allows to search for APPC and CH-EMED-EPR documents. CH-EMED-EPR documents could also be searched through the [PHARM-1](Transactions/PHARM-1) transaction (it's a specialized ITI-18 transaction). The parameters are matched against the corresponding `XDSDocumentEntry` metadata attributes of the documents in the registry.

All the [parameters defined in the IHE profile](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.7.1) are supported:

| Parameter Name                                | Required | Remarks               |
| --------------------------------------------- | -------- | --------------------- |
| `$XDSDocumentEntryPatientId`                  | yes      | The patient's PMP-PID |
| `$XDSDocumentEntryClassCode`                  | no       | -                     |
| `$XDSDocumentEntryTypeCode`                   | no       | -                     |
| `$XDSDocumentEntryPracticeSettingCode`        | no       | -                     |
| `$XDSDocumentEntryCreationTimeFrom`           | no       | see [dates processing](date_processing) |
| `$XDSDocumentEntryCreationTimeTo`             | no       | see [dates processing](date_processing) |
| `$XDSDocumentEntryServiceStartTimeFrom`       | no       | Ignored for APPC documents. see [dates processing](date_processing) |
| `$XDSDocumentEntryServiceStartTimeTo`         | no       | Ignored for APPC documents. see [dates processing](date_processing) |
| `$XDSDocumentEntryServiceStopTimeFrom`        | no       | Ignored for APPC documents. see [dates processing](date_processing) |
| `$XDSDocumentEntryServiceStopTimeTo`          | no       | Ignored for APPC documents. see [dates processing](date_processing) |
| `$XDSDocumentEntryHealthcareFacilityTypeCode` | no       | -                     |
| `$XDSDocumentEntryEventCodeList`              | no       | -                     |
| `$XDSDocumentEntryConfidentialityCode`        | no       | All documents in the eMedication service are set to `Normal` as per the [EPR metadata specs](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_3_epdv_edi_ausgabe_3.pdf.download.pdf/EPDV-EDI_Anhang_3_DE_Ausgabe_3.pdf) |
| `$XDSDocumentEntryAuthorPerson`               | no       | -                     |
| `$XDSDocumentEntryFormatCode`                 | no       | See the [ITI-41 section in this guide](iti41#metadata-codes-per-document-type) |
| `$XDSDocumentEntryStatus`                     | yes      | -                     |
| `$XDSDocumentEntryType`                       | no       | If empty, `Stable` will be assumed |
| `$XDSDocumentEntryDocumentAvailability`       | no       | All documents in the eMedication service are set to `Online` |

### FindSubmissionSets


### FindFolders

The response is empty, as folders are not implemented.

### GetAll

The response won't contain folders, as they're not implemented.

### GetDocuments


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
