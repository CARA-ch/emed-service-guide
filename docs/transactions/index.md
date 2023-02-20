# Transactions

The eMedication service exposes its own endpoints.

Implemented transactions are ITI-18, ITI-41, ITI-43, ITI-57 and CH:PHARM-1.

* publish documents: [XDS ITI-41](iti41.md), MHD ITI-65
* retrieve documents [XDS ITI-43](iti43.md), MHD ITI-68
* delete documents [XDS ITI-57](iti57.md)
* search documents [XDS ITI-18](iti18.md), MHD ITI-66 and ITI-67
* search pharmacy documents [XDS CH:PHARM-1](chpharm1.md), MHD CH:PHARM-5

For details about documents (content and metadata), see the [Documents page](documents.md).

!!! tip

    MHD-equivalent transactions will be implemented in the future.

## Generic error codes

| XDS error code      | Details                                                                                                                                             |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| XDSRegistryError    | In case of business rule error, missing/invalid XUA (authentication errors), unexpected exception.                                                  |
| XDSUnknownPatientId | If the patient ID is unknown (i.e. the patient has not registered), if the subjects is missing rights to preform the action (authorization errors). |
