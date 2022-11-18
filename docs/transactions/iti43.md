# ITI-43

Nothing special about the transaction, implemented as per the spec. All documents are retrievable with an ITI-43 transaction (with the proper access rights).

## Stable documents

## On-demand documents

Generations rules for CH-EMED-EPR [PML (Medication List)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html) and [PMLC (Medication Card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) documents are described in the CH-EMED-EPR IG.

## Error codes

All of these can be errors or warnings.

| Error code | Description |
| ------ | ------ |
| XDSDocumentUniqueIdError | The document associated with the uniqueId is not available. This could be because the document is not available, the requestor is not authorized to access that document or the document is no longer available. |
| XDSMissingHomeCommunityId | A value for the homeCommunityId is required and has not been specified. |
| XDSRegistryBusy XDSRepositoryBusy | Too much activity. (Not used now) |
| XDSRegistryError XDSRepositoryError | Internal Error. The error codes XDSRegistryError or XDSRepositoryError shall be returned if and only if a more detailed code is not available. |
| XDSRegistryOutOfResources XDSRepositoryOutOfResources | Resources are low. (Not used now) |
| XDSResultNotSinglePatient | This error signals that a single request would have returned content for multiple Patient Ids. TODO: that would leak that the ID exists and belongs to another patient. For privacy reasons, would it be treated like a non-existing document? If yes, this error wouldn't be possible. |
| XDSUnavailableCommunity | A community that was queried was not available. |
| XDSUnknownCommunity | A value for the homeCommunityId is not recognized. |
| XDSUnknownRepositoryId | The repositoryUniqueId value could not be resolved to a valid document repository or the value does not match the repositoryUniqueId. |
