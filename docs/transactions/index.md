# Transactions

The eMedication service exposes its own endpoints.

Implemented transactions are [XDS](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html) [ITI-18](https://profiles.ihe.net/ITI/TF/Volume2/ITI-18.html), [ITI-41](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html), [ITI-43](https://profiles.ihe.net/ITI/TF/Volume2/ITI-43.html), [ITI-57](https://profiles.ihe.net/ITI/TF/Volume2/ITI-57.html) and [CH:PHARM-1](chpharm1.md). [MHD](https://profiles.ihe.net/ITI/MHD/index.html) equivalents are given here for information but are not supported yet by the eMedication service.

* publish documents: [XDS ITI-41](iti41.md), _MHD ITI-65_
* retrieve documents [XDS ITI-43](iti43.md), _MHD ITI-68_
* update (delete only) documents [XDS ITI-57](iti57.md)
* search documents [XDS ITI-18](iti18.md), _MHD ITI-66 and ITI-67_
* search pharmacy documents [XDS CH:PHARM-1](chpharm1.md), _MHD CH:PHARM-5_

For details about documents (content and metadata), see the [Documents page](documents.md).

## Generic rules about transactions
* In every transaction, the referenced patient has to match the same patient as the one in the XUA assertion.
* XACML are checked but ignored. This will be re-enabled in the future.
* Patients are allowed to perform every transactions
* Healthcare Professionals are allowed to:
    * [Search documents (registry stored query XDS ITI-18)](iti18.md), 
    * [Retrieve document set (XDS ITI-43)](iti43.md), 
    * [Provide and register document set (XDS ITI-41)](iti41.md), 
    * [Update document set (XDS ITI-57)](iti57.md)
* Healthcare Professionals cannot do anything related to [APPC documents (Advanced Patient Privacy Consent)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf).
* Technical User (TCU)  can only [provide and register document set (XDS ITI-41)](iti41.md).
* Anything else denied.

!!! tip

    MHD-equivalent transactions will be implemented in the future.

## XDS vs. MHD
IHE provides different [profiles](https://profiles.ihe.net/ITI/TF/Volume1/index.html), among which XDS and MHD make it possible to exchange documents:

* [XDS (Cross Enterprise Document Sharing)](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html)
* [MHD (Mobile access to Health Documents)](https://profiles.ihe.net/ITI/MHD/index.html)

XDS is the profile prescribed by the [Swiss EPR ordinance](https://www.fedlex.admin.ch/eli/cc/2017/205/fr), but MHD is simpler to implement, as it doesn't require the complex XDS stack (SOAP, WSSE, MIME-Multipart, MTOM/XOP, ebRIM, and multi-depth XML), and relies on a lighter REST interface.

Even though the eMedication service doesn't support it yet, it is possible to use the MHD profile though a third party component named [mobile access gateway](https://www.mobileaccessgateway.ch/). This component is not affiliated with this service, but referenced here for information purpose.


## Generic error codes

| XDS error code      | Details                                                                                                                                             |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| XDSRegistryError    | In case of business rule error, missing/invalid XUA (authentication errors), unexpected exception.                                                  |
| XDSUnknownPatientId | If the patient ID is unknown (i.e. the patient has not registered), if the subjects is missing rights to preform the action (authorization errors). |
