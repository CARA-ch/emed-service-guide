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

| Document type | DocumentEntry.classCode                 | DocumentEntry.formatCode                   | DocumentEntry.typeCode                                        |
| ------------- | --------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| MTP           | `440545006` (Prescription record)       | `urn:che:epr:ch-emed:mtp:2022`             | `419891008` (Record artifact)                                 |
| PRE           | `440545006` (Prescription record)       | `urn:che:epr:ch-emed:pre:2022`             | `761938008` (Medicinal Prescription record (record artifact)) |
| DIS           | `440545006` (Prescription record)       | `urn:che:epr:ch-emed:dis:2022`             | `419891008` (Record artifact)                                 |
| PADV          | `440545006` (Prescription record)       | `urn:che:epr:ch-emed:padv:2022`            | `419891008` (Record artifact)                                 |
| PML           | `422735006` (Summary clinical document) | `urn:che:epr:ch-emed:pml:2022`             | `721912009` (Medication summary document)                     |
| PMLC          | `422735006` (Summary clinical document) | `urn:che:epr:ch-emed:medication-card:2022` | `721912009` (Medication summary document)                     |

## Publishing a PMP-APPC document

| DocumentEntry    | APPC                                           |
| ---------------- | ---------------------------------------------- |
| classCode        | `371537001` (Consent report (record artifact)) |
| formatCode       | `urn:ihe:iti:appc:2016:consent`                |
| mimeType         | `text/xml`                                     |
| serviceStartTime | None                                           | <!-- TODO: now? -->
| serviceStopTime  | None                                           |
| typeCode         | `419891008` (Record artifact)                  |
