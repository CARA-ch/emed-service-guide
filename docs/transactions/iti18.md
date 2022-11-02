# ITI-18

## Stored queries

### FindDocuments

This stored query allows to search for APPC and eMed documents. eMed documents could also be searched through the [PHARM-1](Transactions/PHARM-1) transaction (it's a specialized ITI-18 transaction).

When processing requests, the following rules are applied:
* $MetadataLevel: If present, the attribute shall equal to "1", as per *Nationale Anpassungen der Integrationsprofile nach Artikel 5 Absatz 1 Buchstabe b EPDV-EDI*.
* $XDSDocumentEntryClassCode: N/A
* $XDSDocumentEntryTypeCode: N/A
* $XDSDocumentEntryCreationTimeFrom: see [dates processing](DatesProcessing).
* $XDSDocumentEntryCreationTimeTo: see [dates processing](DatesProcessing).
* $XDSDocumentEntryServiceStartTimeFrom: not considered for APPC documents
* $XDSDocumentEntryServiceStartTimeTo: not considered for APPC documents
* $XDSDocumentEntryServiceStopTimeFrom: not considered for APPC documents
* $XDSDocumentEntryServiceStopTimeTo: not considered for APPC documents
* $XDSDocumentEntryFormatCode: N/A
* $XDSDocumentEntryDocumentAvailability: if specified, everything else than "Online" will yield no result.
* $XDSDocumentEntryPatientId: The patient XAD-PID.

### FindSubmissionSets

* $XDSDocumentEntryPatientId: The patient XAD-PID.

### FindFolders

The response is empty, as folders are not implemented.

### GetAll

The response won't contain folders, as they're not implemented.

* $patientId: The patient XAD-PID.

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

<!-- Use of homeCommunityId https://ihe.github.io/publications/ITI/TF/Volume2/ITI-18.html#3.18.4.1.2.3.8 -->

## Error codes

Specific codes not covered by generic codes.

| XDS error code             | Details                                             |
| -------------------------- | --------------------------------------------------- |
| XDSStoredQueryMissingParam | If a required search parameter is missing           |
| XDSStoredQueryParamNumber  | If too many/too much search parameters are provided |
| XDSUnknownStoredQuery      | If the search query is unknown                      |
| XDSRegistryError           | If the search query is known but unsupported        |
| XDSRegistryMetadataError   | If $MetadataLevel is not "1"                        |
