# ITI-57

The [ITI-57 (Update Document Set)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_XDS_Metadata_Update.pdf) transaction makes it possible to update existing documents. 

## Update DocumentEntry Metadata

This transaction is implemented, but only for the *deletionStatus* attribute. This implies that the only possible update in the service is the deletion of a document.

### Deleting a CH-EMED-EPR document
The rules are similar to [ITI-41](iti41.md) replacement.

* Patient / representative can only request the removal of a document published by the same patient. Hence a patient cannot request the removal of a document published by a healthcare professional.
    * Representatives are not supported by the service yet, but will be in the future.
* Healthcare professionals can only request the removal of a document published by themselves or by another healthcare professional of the same community of affiliation.
	  * Deletion rights related to HCP are not implemented yet and will be subject to further discussion before implementation. For now, patients are allowed to delete documents provided by HCPs and HCPs are not allowed to delete any documents.
* Document administrator may only request the removal of any document published by either a patient, representative, or healthcare professional.
* Only approved documents at the end of a treatment chain can be requested to be deleted.
* Requested deletion of a CH-EMED-EPR document will also flag for deletion the chain of replaced documents by this document, if it exists.
* Documents to be deleted must exist in the repository.
* Currently the service just permanently deletes the requested documents, no archival or flagging is done.

### Deleting an APPC document
* Deleting an [APPC](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf) document means that the patient is opting out. It triggers an archive and removal of all the documents for this patient: [CH-EMED-EPR](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/) and APPC (including and any deprecated ones) documents are both concerned.
* Only the patients or their representatives can request the deletion of their own APPC documents.
* A policy administrator can request the removal of any patient's APPC of their own reference community.
* The APPC document has to be approved
    * This implies that direct deletion of a deprecated (replaced) APPC document is not possible.
    * Unapproved APPC document deletion can only occur at opt-out.
* Currently the service just permanently deletes the requested APPC document and all the CH-EMED-EPR documents and any other information stored by the PMP concerning this patient, no archival or flagging is done.

## Update DocumentEntry availabilityStatus

This transaction is unsupported for eMed and policy documents.

## Update Folder Metadata

This is unsupported, as the service doesn't support folders.

## Update Folder availabilityStatus

This is unsupported, as the service doesn't support folders.

## Update Association availabilityStatus

This is unsupported.

## Submit Associations

This is unsupported.

## Error codes

The resulting status shall be Success or Failure. PartialSuccess shall not be used.

| Error Code | Reasons |
| ------ | ------ |
| XDSMetadataUpdateError | General metadata update error. |
| XDSPatientIDReconciliationError | Update encountered error where Patient IDs did not match. Not allowed. |
| XDSMetadataUpdateOperationError | Document Registry cannot decode the requested metadata update. |
| XDSMetadataVersionError | The version number included in the update request did not match the existing object. |

Common errors and proposed error codes:

* The operation is unsupported: (not seen in the spec) XDSMetadataUpdateOperationError
* Trying to set the deletionStatus to deletionProhibited: Error or OperationError? <!-- TODO: -> error -->

