# Transactions

The eMedication service exposes its own [endpoints](../../references/endpoints/index.md).

The transactions supported by these exposed endpoints can be classified as either belonging to the eMedication service content, that is, document-based (XDS or XDS-like transactions), or of a more infrastructure-like nature (e.g. PIX queries).

## XDS and XDS-like transactions

Implemented transactions:

* publish documents: [XDS ITI-41](iti41.md), [MHD ITI-65](iti65.md)
* retrieve documents [XDS ITI-43](iti43.md), [MHD ITI-68](iti68.md)
* update (to request deletion only) document metadata: [XDS ITI-57](iti57.md), [MHD CH:MHD-1](chmhd1.md)
* search documents [XDS ITI-18](iti18.md), [MHD ITI-67](iti67.md)
* search pharmacy documents [XDS CH:PHARM-1](chpharm1.md), [MHD CH:PHARM-5](chpharm5.md)

For details about documents (content and metadata), see the [Documents page](./documents/index.md).

### Generic rules about transactions
* In every transaction where a security token is required, the referenced patient has to match the same patient as the one in the XUA assertion.
* Patients are allowed to perform all types of transactions
* Healthcare Professionals are allowed to:
    * [Search documents (registry stored query XDS ITI-18)](iti18.md) and MHD/PHARM equivalents, 
    * [Retrieve document set (XDS ITI-43)](iti43.md) and MHD equivalent, 
    * [Provide and register document set (XDS ITI-41)](iti41.md) and MHD equivalent, 
    * [Update document set (XDS ITI-57)](iti57.md) and MHD equivalent
* Healthcare Professionals cannot do anything related to [APPC documents (Advanced Patient Privacy Consent)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf).
* Technical User (TCU)  can only [provide and register document set (XDS ITI-41)](iti41.md) (and MHD equivalent).
* Anything else denied.

!!! tip

    MHD transactions, as profiled by [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.html), are provided as an early-stage-support. Bugs and profile changes/improvements are to be expected and all feedback is welcome.

### XDS vs. MHD
IHE provides different [profiles](https://profiles.ihe.net/ITI/TF/Volume1/index.html), among which XDS and MHD make it possible to exchange documents:

* [XDS (Cross Enterprise Document Sharing)](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html)
* [MHD (Mobile access to Health Documents)](https://profiles.ihe.net/ITI/MHD/index.html)

XDS is the profile prescribed by the [Swiss EPR ordinance](https://www.fedlex.admin.ch/eli/cc/2017/205/fr), but MHD is simpler to implement, as it doesn't require the complex XDS stack (SOAP, WSSE, MIME-Multipart, MTOM/XOP, ebRIM, and multi-depth XML), and relies on a lighter REST interface.

Even though the eMedication service now supports MHD transactions, it is also possible to use the MHD profile through a third party component named [mobile access gateway](https://www.mobileaccessgateway.ch/). This component is not affiliated with this service, but referenced here for informational purposes.


### Generic error codes

| XDS error code      | Details                                                                                                                                             |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| XDSRegistryError    | In case of business rule error, missing/invalid XUA (authentication errors), unexpected exception.                                                  |
| XDSUnknownPatientId | If the patient ID is unknown (i.e. the patient has not registered), if the subjects is missing rights to preform the action (authorization errors). |

## Other transactions

Besides the document-based transactions, other transactions are supported as part of the eMedication service.

- [ITI-20: Record Audit Event](iti20.md) (both syslog and FHIR feed options supported)
- [ITI-44: Patient Identity Feed HL7 V3](iti44.md) and the PIXm equivalent [ITI-83](iti83.md)
- [ITI-45: PIXV3 Query](iti45.md) and the PIXm equivalent [ITI-104](iti104.md)
- [CH ATC: Audit Trail Consumption](chatc.md)

* ATNA logs: [ITI-20](iti20.md), [CH:ATC][chatc.md]
* PIX: [PIXv3 ITI-45](iti45.md), [PIXv3 ITI-44](iti44.md), [PIXm ITI-83](iti83.md), [PIXm ITI-104](iti104.md)

## Other links of interest
In addition to the [XDS](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html) transactions implemented by the service, implementers may find it useful to check out the following profiles and transactions:

* [Cross Enterprise User Assertion (XUA)](https://profiles.ihe.net/ITI/TF/Volume1/ch-13.html) profile, and the [Provide X-User Assertion (XUA ITI-40)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-40.html#3.40) transaction.
    * The XUA profile defines the format of assertions inserted in transactions, that contain information about the users and their roles.
    * The ITI-40 transaction is used to obtain the assertions from an assertion provider.
* [Healthcare Provider Directory (HPD)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_HPD.pdf) profile, and the [Provider Information Query (HPD ITI-58)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-58.html) transaction.
    * The HPD profile defines the management of healthcare provider information in a directory structure.
    * The ITI-58 transaction can be used to lookup healthcare provider information from a healthcare provider directory.
* [Audit Trail and Node Authentication (ATNA)](https://profiles.ihe.net/ITI/TF/Volume1/ch-9.html) profile, and the [record audit event (ATNA ITI-20)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-20.html#3.20) transaction.
    * The ATNA profile might be used by implementers to record audit events through the ITI-20 transaction.

The [EPD-by-example](https://github.com/ehealthsuisse/EPD-by-example/) github project provides guidance and examples about these transactions and others, especially:

* [Get X-User Assertion](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/GetXAssertion.md) and [Provide X-User Assertion](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/ProvideXAssertion.md)
* [Authenticate User](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/AuthenticateUser.md)
* [PIX Query](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/PIXQuery.md)