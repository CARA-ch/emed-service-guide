# Transactions

The eMedication service exposes its own endpoints.

Implemented transactions are [XDS](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html) [ITI-18](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html), [ITI-41](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html), [ITI-43](https://profiles.ihe.net/ITI/TF/Volume2/ITI-43.html), [ITI-57](https://profiles.ihe.net/ITI/TF/Volume2/ITI-57.html) and [CH:PHARM-1](chpharm1.md). [MHD](https://profiles.ihe.net/ITI/MHD/index.html) equivalents are given here for information but are not supported yet by the eMedication service.

* publish documents: [XDS ITI-41](iti41.md), _MHD ITI-65_
* retrieve documents [XDS ITI-43](iti43.md), _MHD ITI-68_
* delete documents [XDS ITI-57](iti57.md)
* search documents [XDS ITI-18](iti18.md), _MHD ITI-66 and ITI-67_
* search pharmacy documents [XDS CH:PHARM-1](chpharm1.md), _MHD CH:PHARM-5_

For details about documents (content and metadata), see the [Documents page](documents.md).

!!! tip

    MHD-equivalent transactions will be implemented in the future.

## XDS vs. MHD
IHE provides different [profiles](https://profiles.ihe.net/ITI/TF/Volume1/index.html), among which XDS and MHD make it possible to exchange documents:

* [XDS (Cross Enterprise Document Sharing)](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html)
* [MHD (Mobile access to Health Documents)](https://profiles.ihe.net/ITI/MHD/index.html)

XDS is the profile prescribed by the [Swiss EPR ordinance](https://www.fedlex.admin.ch/eli/oc/2023/221/fr#annex_5/lvl_u1/lvl_1), but MHD is simpler to implement, as it doesn't require the complex XDS stack (SOAP, WSSE, MIME-Multipart, MTOM/XOP, ebRIM, and multi-depth XML), and relies on a lighter REST interface.

Even though the eMedication service doesn't support it yet, it is possible to use the MHD profile though a third party component named [mobile access gateway](https://www.mobileaccessgateway.ch/). This component is not affiliated with this service, but referenced here for information purpose.


## Generic error codes

| XDS error code      | Details                                                                                                                                             |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| XDSRegistryError    | In case of business rule error, missing/invalid XUA (authentication errors), unexpected exception.                                                  |
| XDSUnknownPatientId | If the patient ID is unknown (i.e. the patient has not registered), if the subjects is missing rights to preform the action (authorization errors). |
