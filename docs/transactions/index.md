# Transactions

The eMedication service exposes its own IHE endpoints.

Implemented transactions are ITI-18, ITI-41, ITI-43, ITI-57 and CH:PHARM-1.

* [ITI-18](iti18.md)
* [ITI-41](iti41.md)
* [ITI-43](iti43.md)
* [ITI-57](iti57.md)
* [CH:PHARM-1](chpharm1.md)

!!! tip

    MHD-equivalent transactions will be implemented in the future.

## Generic error codes


| XDS error code      | Details |
| ------------------- | ------- |
| XDSRegistryError | In case of business rule error, missing/invalid XUA (authentication errors), unexpected exception. |
| XDSUnknownPatientId | If the patient ID is unknown (i.e. the patient has not registered), if the subjects is missing rights to preform the action (authorization errors). |
