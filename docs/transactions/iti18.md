# ITI-18

## Error codes

Specific codes not covered by generic codes.

| XDS error code      | Details |
| ------------------- | ------- |
| XDSStoredQueryMissingParam | If a required search parameter is missing |
| XDSStoredQueryParamNumber | If too many/too much search parameters are provided |
| XDSUnknownStoredQuery | If the search query is unknown |
| XDSRegistryError | If the search query is known but unsupported |
| XDSRegistryMetadataError | If $MetadataLevel is not "1" |

## FindDocuments

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