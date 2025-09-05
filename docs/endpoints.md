# Endpoints
This page lists the different endpoints available to use the e-medication service.

## Base URL
| Environnement | Base URL | [EPRIK](https://ahdis.github.io/epr-integration-cara/) Base URL |
| --- | --- | --- |
| Integration | https://ws-pmp-int.cara.ch | http(s?)://test.ahdis.ch/eprik-cara/camel/pmp-int |
| Development | https://ws-pmp-dev.cara.ch | http(s?)://test.ahdis.ch/eprik-cara/camel/pmp-dev |

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
|Query Pharmacy Documents (CH:PHARM-1)|`/pmp/services/cmpd/chpharm1`| [CH:PHARM-1](transactions/chpharm1.md)|

## MHD EndPoints
[MHD transactions](https://profiles.ihe.net/ITI/MHD/index.html) are now supported, as profiled by [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.htmls), and available through the `/pmp/fhir` and `/pmp/mhd` endPoints:

| Transaction | Path | Comment |
| --- | --- | --- |
| [Provide Document Bundle (ITI-65)](transactions/iti65.md)|`/pmp/fhir/Bundle`|MHD equivalent to [ITI-41](transactions/iti41.md).|
| [Find Document References (ITI-67)](transactions/iti67.md)|`/pmp/fhir/DocumentReference`|MHD equivalent to [ITI-18](transactions/iti18.md).|
| [Retrieve Document (ITI-68)](transactions/iti68.md)|`/pmp/mhd/iti68`|MHD equivalent to [ITI-43](transactions/iti43.md).|
| [Update Document Metadata (CH:MHD-1)](transactions/chmhd1.md) | `/pmp/fhir/DocumentReference` |MHD equivalent to [ITI-57](transactions/iti57.md).|
| [CH:PHARM-5](transactions/chpharm5.md)|`/pmp/fhir/DocumentReference/<operation>`|MHD equivalent to [CH:PHARM-1](transactions/chpharm1.md). The endpoint changes depending on the CH:PHARM-5 operation, see the [transaction page](transactions/chpharm5.md) for details.|

## PIXm EndPoints
|Transaction|Path|Comment|
|---|---|---|
| [Mobile Patient Identifier Cross-reference Query (ITI-83)](transactions/iti83.md) | `/pmp/fhir/Patient` | REST equivalent to [ITI-45](transactions/iti45.md) |
| [Patient Identity Feed (ITI-104)](transactions/iti104.md) | `/pmp/fhir/Patient` | REST equivalent to [ITI-44](transactions/iti44.md) |

## Audit Events
| Transaction | Path | Documentation |
| --- | --- | --- |
| Record Audit Event (syslog) | `:6514` | [ITI-20](transactions/iti20.md) |
| Record Audit Event (RESTful) | `/alpage/fhir/AuditEvent` | [ITI-20](transactions/iti20.md) | 
| Audit Trail Consumption (CH:ATC) | `/alpage/fhir/AuditEvent` | [CH:ATC](transactions/chatc.md) |
