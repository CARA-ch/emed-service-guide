# ITI-41

DocumentEntry metadata are described in [documents](documents.md). Additional constraints are described here.

## Publishing a CH-EMED-EPR document

When publishing a CH-EMED-EPR documents, all references (from the key elements) shall be known to the eMedication service.
All non-key elements are ignored by the eMedication service.

Some values from the DocumentEntry are checked against the document:

| DocumentEntry | CH-EMED-EPR                                                  |
| ------------- | ------------------------------------------------------------ |
| author        | `Composition.author`                                         |
| creationTime  | `Composition.date`                                           |
| languageCode  | `Composition.language`                                       |
| mimeType      | `application/fhir+xml` or `application/fhir+json`            |
| uniqueId      | `Bundle.identifier.value` and `Composition.identifier.value` |

| Document type | DocumentEntry.classCode                   | DocumentEntry.formatCode                   | DocumentEntry.typeCode      |
| ------------- | ----------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| MTP           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:mtp:2022`             | ~~`419891008` Record artifact~~<br>`761931002` _Medication treatment plan report (record artifact)_ |
| PRE           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:pre:2022`             | `761938008` _Medicinal Prescription record (record artifact)_ |
| DIS           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:dis:2022`             | ~~`419891008` Record artifact~~<br>`294121000195110` _Medication dispense document (record artifact)_ |
| PADV          | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:padv:2022`            | `419891008` _Record artifact_                                 |
| PML           | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:pml:2022`             | `721912009` _Medication summary document_                     |
| PMLC          | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:medication-card:2022` | ~~`721912009` _Medication summary document_~~<br>`736378000` _Medication management plan (record artifact)_ |

The class and type codes are from the SNOMED CT system: `2.16.840.1.113883.6.96`.

## Publishing a PMP-APPC document

| DocumentEntry    | APPC                                           |
| ---------------- | ---------------------------------------------- |
| classCode        | `371537001` _Consent report (record artifact)_ |
| formatCode       | `urn:ihe:iti:appc:2016:consent`                |
| mimeType         | `text/xml`                                     |
| serviceStartTime | None                                           | <!-- TODO: now? -->
| serviceStopTime  | None                                           |
| typeCode         | `419891008` _Record artifact_                  |
