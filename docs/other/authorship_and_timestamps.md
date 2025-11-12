# Authorship & Timestamps

This page contains a summary on authorship and timestamps as described by [CH EMED](https://fhir.ch/ig/ch-emed/authorship.html)

## Authorship

Document | Author of the document (person / device) | Author of the medical decision (person) | Author original document; if different from the author of the medical decision (person) |
| :-: | --- | --- | --- |
| MTP | Composition.author 1..<br>(person 1.. \| device 0..) | MedicationStatement.informationSource 1.. | - |
| PRE | Composition.author 1..<br>(person 1.. \| device 0..) | MedicationRequest.requester 1.. | - |
| DIS | Composition.author 1..<br>(person 1.. \| device 0..) | MedicationDispense.performer.actor 1..<br>MedicationAdministration.performer.actor 1.. | - |
| PADV | Composition.author 1..<br>(person 1.. \| device 0..) | Observation.performer 1.. | - |
| LIST | Composition.author 1..<br>(device, which dynamically generates the document) | MedicationStatement.informationSource 1..<br>MedicationRequest.requester 1..<br>MedicationDispense.performer.actor 1..<br>MedicationAdministration.performer.actor 1.. | Observation.performer 1..<br>MedicationStatement.extension:authorDocument 0..<br>MedicationRequest.extension:authorDocument 0..<br>MedicationDispense.extension:authorDocument 0..<br>MedicationAdministration.extension:authorDocument 0..<br>Observation.extension:authorDocument 0.. |
| CARD | Composition.author 1..<br>(person or device) | MedicationStatement.informationSource* 1.. | MedicationStatement.extension:authorDocument* 0.. |

*: The CARD is an aggregation of all medications, respectively all documents, which may have had different authors. The “last author” (author of the last input for this treatment) is indicated in each case.

## Timestamps

The document's creation date and time is mapped for all documents in `Bundle.timestamp` and `Composition.date`.

It can also be specified at entry level :

| Name | Cardinality | Description | Default value
| --- | --- | --- | --- |
| [MedicationStatement.dateAsserted](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-medicationstatement.html) | (1..1) | When the statement was asserted | - |
| [MedicationRequest.authoredOn](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-medicationrequest.html) | (1..1) | When request was initially authored | - |
| [MedicationDispense.whenHandedOver](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-medicationdispense.html) | (1..1) | When product was given out | - |
| [Observation.issued](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-observation.html) | (1..1) | Date/Time this version was made available | - |
| [Dosage.timing.boundsPeriod (Dosage - MedicationRequest)](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-dosage-medicationrequest.html) | (0..1) | Start and/or end of treatment | By default, the start value will be set to now and the end value will be null (valid ad infinitum) |
| [Dosage.timing.boundsPeriod (Dosage - MedicationStatement / MedicationDispense)](https://fhir.ch/ig/ch-emed/StructureDefinition-ch-emed-dosage.html) | (0..1) | Start and/or end of treatment | By default, the start value will be set to now and the end value will be null (valid ad infinitum) |
