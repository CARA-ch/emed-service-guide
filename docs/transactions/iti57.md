# ITI-57

ITI-57: Update Document Set transaction is described in *IHE IT Infrastructure
Technical Framework Supplement XDS Metadata Update*.

## Update DocumentEntry Metadata

This transaction is implemented, but only for the *deletionStatus* attribute. See rules that are enforced for eMed and policy documents.

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
