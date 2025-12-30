# Processing of dates

## XDS

HL7's DTM shall be encoded in the format `YYYY[MM[DD[HH[MM[SS[.S[S[S[S]]]]]]]]][+/-ZZZZ]`. It allows various precision levels and the choice of time zone.

DocumentEntries fields *creationTime*, *serviceStartTime* and *serviceStopTime* shall be specified in UTC (DTM's time zone part is forbidden).

ITI-18 query parameters can be expressed in any time zone.

Parameters _$XDSDocumentEntry(Creation|ServiceStart|ServiceStop)TimeFrom_ are inclusive, parameters _$XDSDocumentEntry(Creation|ServiceStart|ServiceStop)TimeTo_ are exclusive.

When encountering an incomplete (i.e. a date that is not precise to the second) date, the date is set to the earliest instant covered by the partial date.

The PMP recommends to:
1. always give dates in UTC;
2. always give dates if possible at least at the second precision.

## FHIR

In FHIR, [periods](https://www.hl7.org/fhir/datatypes.html#Period) are given with inclusive boundaries.

The [dateTime](https://www.hl7.org/fhir/datatypes.html#dateTime) format is different: YYYY, YYYY-MM, YYYY-MM-DD, YYYY-MM-DDThh:mm:ss+zz:zz or YYYY-MM-DDThh:mm:ss.sssZ. When using the time precision, the timezone is mandatory.

<!-- When filtering, the following [search keywords](https://www.hl7.org/fhir/search.html#date) are available:

| Keyword | Comment |
| ------ | ------ |
| eq | Equal. Possible in XDS. |
| ne | Not equal. Impossible in XDS. What does XDS on FHIR say? | 
| lt | Less than. Possible in XDS. |
| gt | Greater than. Possible in XDS. | 
| ge | Greater than or equal to. Possible in XDS. |
| le | Less than or equal to. Possible in XDS. | 
| sa | ? |
| eb | ? | 
| ap | ? |
-->
