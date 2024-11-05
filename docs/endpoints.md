# Endpoints
This page lists the different endpoints available to use the e-medication service.

## Base URL
| Environnement | Base URL |
| --- | --- |
| Integration | https://ws-pmp-int.cara.ch |
| Development | https://ws-pmp-dev.cara.ch |

## PIX EndPoints
| Transaction | Path | Documentation |
| --- | --- | --- |
|Patient Identity Feed HL7 V3 (ITI-44)|`/pmp/services/iti44`| [ITI-44](transactions/iti44.md). See also the [IHE documentation](https://profiles.ihe.net/ITI/TF/Volume2/ITI-44.html).|
|PIXV3 Query (ITI-45)|`/pmp/services/iti45`| [ITI-45](transactions/iti45.md). See also the [IHE documentation](https://profiles.ihe.net/ITI/TF/Volume2/ITI-45.html).|

## XDS EndPoints
[XDS transactions](https://profiles.ihe.net/ITI/TF/Volume1/ch-10.html) are available through the `/pmp/services` endpoints:

| Transaction | Path | Documentation |
| --- | --- | --- |
|Registry Stored Query (ITI-18)|`/pmp/services/xds/iti18`| [ITI-18](transactions/iti18.md)|
|Provide and Register Document Set-b (ITI-41)|`/pmp/services/xds/iti41`| [ITI-41](transactions/iti41.md)|
|Retrieve Document Set (ITI-43)|`/pmp/services/xds/iti43`| [ITI-43](transactions/iti43.md)|
|Update Document Set (ITI-57)|`/pmp/services/xds/iti57`| [ITI-57](transactions/iti57.md)|
|Query Pharmacy Documents (CH:PHARM-1)|`/pmp/services/xds/chpharm1`| [CH:PHARM-1](transactions/chpharm1.md)|

## MHD EndPoints
[MHD transactions](https://profiles.ihe.net/ITI/MHD/index.html) are still a work in progress. They will be available through the `/fhir` and `/mhd` endPoints:

| Transaction | Path | Comment |
| --- | --- | --- |
| Provide Document Bundle (ITI-65)|`/pmp/fhir/`|REST equivalent to [ITI-41](transactions/iti41.md). See also [official documentation](https://profiles.ihe.net/ITI/MHD/ITI-65.html).|
| Retrieve Document (ITI-68)|`/pmp/mhd/iti68`|MHD equivalent to [ITI-43](transactions/iti43.md). See also [official documentation](https://profiles.ihe.net/ITI/MHD/ITI-68.html).|
| Find Document Lists (ITI-66)|`/pmp/mhd/list`|MHD equivalent to [ITI-18](transactions/iti18.md). See also [official documentation](https://profiles.ihe.net/ITI/MHD/ITI-66.html).|
| Find Document References (ITI-67)|`/pmp/mhd/DocumentReference`|MHD equivalent to [ITI-18](transactions/iti18.md). See also [official documentation](https://profiles.ihe.net/ITI/MHD/ITI-67.html).|
| CH:PHARM-5|`/pmp/mhd/chpharm5`|MHD equivalent to [CH:PHARM-1](transactions/chpharm1.md). See also [official documentation](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf), § 3.2.|

## Audit Events
| Transaction | Path | Documentation |
| --- | --- | --- |
| Record Audit Event (syslog) | `:6514` | [ITI-20](transactions/iti20.md) |
| Audit Trail Consumption (CH ATC) | `/alpage/fhir/AuditEvent` | [CH ATC](transactions/chatc.md) |
