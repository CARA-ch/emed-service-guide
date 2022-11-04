# ITI-41

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

| Document type | DocumentEntry.classCode               | DocumentEntry.formatCode            | DocumentEntry.typeCode                                    |
| ------------- | ------------------------------------- | ----------------------------------- | --------------------------------------------------------- |
| MTP           | 440545006 (Prescription record)       | urn:ch:ch-emed:mtp:2020             | 419891008 (Record artifact)                               |
| PRE           | 440545006 (Prescription record)       | urn:ch:ch-emed:pre:2020             | 761938008 Medicinal Prescription record (record artifact) |
| DIS           | 440545006 (Prescription record)       | urn:ch:ch-emed:dis:2020             | 419891008 (Record artifact)                               |
| PADV          | 440545006 (Prescription record)       | urn:ch:ch-emed:padv:2020            | 419891008 (Record artifact)                               |
| PML           | 422735006 (Summary clinical document) | urn:ch:ch-emed:pml:2020             | 721912009 (Medication summary document)                   |
| PMLC          | 422735006 (Summary clinical document) | urn:ch:ch-emed:medication-card:2020 | 721912009 (Medication summary document)                   |

## Publishing a CH-APPC document

| DocumentEntry    | APPC         |
| ---------------- | ------------ |
| classCode        | Single value |
| formatCode       | Single value |
| mimeType         | `text/xml`   |
| serviceStartTime | None         |
| serviceStopTime  | None         |
| typeCode         | Single value |
