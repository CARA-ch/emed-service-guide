# ITI-41

The [ITI-41 (Provide and Register Document Set-b)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html) transaction allows a [content sender](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2) to send documents to the eMedication service ([content receiver](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2)). 

Generic DocumentEntry metadata are described in the [documents section](documents.md). This page references additional constraints specific to ITI-41.

## Publishing documents
### SubmissionSet and relations
The eMedication service does not support folders and can only receive ONE document at a time. Hence, the [metadata](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1) must always contain a [SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.1) with a single [DocumentEntry](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.3).

Only the following [associations](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2) are supported :
|Association Type|Usage|
|-------------------|-------|
|[urn:ihe:iti:2007:AssociationType:RPLC](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#t4.2.2-1)|[To replace a previously published document]((https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2)).|
|[urn:oasis:names:tc:ebxml-regrep:AssociationType:HasMember](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.2.1.1)|[To link a DocumentEntry to the SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.1).|

### Uniqueness
Documents can be send to the eMedication service only once. Uniqueness is checked against [DocumentEntry's unique Id](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.26), that must be an UUID.

## Publishing a CH-EMED-EPR document
### Referencing external documents

When publishing a CH-EMED-EPR documents, all references (from the key elements) shall be known to the eMedication service. All non-key elements are ignored by the eMedication service. 

This implies that the metadata cannot contain [Reference ids](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.28). This option is not supported, and submission of a reference to an existing document is forbidden.

### Metadata values check
Some values from the DocumentEntry are checked against the document (values in metadata and document must be equal):

| DocumentEntry | CH-EMED-EPR                                                  |
| ------------- | ------------------------------------------------------------ |
| author        | `Composition.author`                                         |
| creationTime  | `Composition.date`                                           |
| languageCode  | `Composition.language`                                       |
| mimeType      | `application/fhir+xml` or `application/fhir+json`            |
| uniqueId      | `Bundle.identifier.value` and `Composition.identifier.value` |

### Metadata to use for each document type
| Document type | DocumentEntry.classCode                   | DocumentEntry.formatCode                   | DocumentEntry.typeCode      |
| ------------- | ----------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| MTP           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:mtp:2022`             |`761931002` _Medication treatment plan report (record artifact)_ |
| PRE           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:pre:2022`             | `761938008` _Medicinal Prescription record (record artifact)_ |
| DIS           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:dis:2022`             | `294121000195110` _Medication dispense document (record artifact)_ |
| PADV          | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:padv:2022`            | `419891008` _Record artifact_                                 |
| PML           | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:pml:2022`             | `721912009` _Medication summary document_                     |
| PMLC          | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:medication-card:2022` |`736378000` _Medication management plan (record artifact)_ |

The class and type codes are from the SNOMED CT system: `2.16.840.1.113883.6.96`.

### Confidentiality
[Confidentiality level](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.5) in the DocumentEntry metadata is [normal](https://wiki.hl7.de/index.php/2.16.840.1.113883.5.25) by default, and can be specified otherwise.

## Publishing a PMP-APPC document

| DocumentEntry    | APPC                                           |
| ---------------- | ---------------------------------------------- |
| classCode        | `371537001` _Consent report (record artifact)_ |
| formatCode       | `urn:ihe:iti:appc:2016:consent`                |
| mimeType         | `text/xml`                                     |
| serviceStartTime | None                                           | <!-- TODO: now? -->
| serviceStopTime  | None                                           |
| typeCode         | `419891008` _Record artifact_                  |
