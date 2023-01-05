# Documents

## Content

The eMedication service only support [CH-APPC](../chappc/index.md) and [CH-EMED](../chemed/index.md) documents.

## Metadata

When two cardinalities are given, the first one is for Content Sender actors and the second one for the Document Repository actor.

| DocumentEntry                  | Cardinality | Comment                                           |
| ------------------------------ | :---------: | ------------------------------------------------- |
| author                         |    0..*     | It's 1..* for eMed documents                      |
| author.authorInstitution       |      M      |                                                   |
| author.authorPerson            |      1      |                                                   |
| author.authorRole              |      1      |                                                   |
| author.authorSpecialty         |    0..?     |                                                   |
| author.authorTelecommunication |      M      |                                                   |
| availabilityStatus             |  0..1 / 1   | Only "Approved" for Content Senders               | <!-- Reviewed -->
| classCode                      |      1      |                                                   |
| comments                       |    0..1     |                                                   |
| confidentialityCode            |      1      | Only "Normal"                                     | <!-- Reviewed -->
| creationTime                   |      1      |                                                   |
| deletionStatus                 |  0..1 / 1   | Only "deletion not requested" for Content Senders |
| documentAvailability           |  0..1 / 1   | Only "Online"                                     | <!-- Reviewed -->
| entryUUID                      |      1      | Random value                                      |
| eventCodeList                  |    0..*     |                                                   |
| formatCode                     |      1      |                                                   |
| hash                           |  0..1 / 1   |                                                   | <!-- Reviewed -->
| healthcareFacilityTypeCode     |      1      |                                                   |
| homeCommunityId                |  0..1 / 1   |                                                   |
| languageCode                   |      1      |                                                   |
| legalAuthenticator             |    0..1     |                                                   |
| limitedMetadata                |      0      |                                                   |
| logicalID                      |  0..1 / 1   | Same value as the `entryUUID`                     |
| mimeType                       |      1      | Single value                                      |
| objectType                     |      1      | Only "stable" for Content Senders                 | <!-- Reviewed -->
| originalProviderRole           |      1      |                                                   | <!-- Reviewed -->
| patientId                      |      1      | The XAD-PID                                       | <!-- Reviewed -->
| practiceSettingCode            |      1      |                                                   |
| referenceIdList                |  0..* / 0   | Ignored by the eMedication service                | <!-- Reviewed -->
| repositoryUniqueId             |  0..1 / 1   |                                                   |
| serviceStartTime               |    0..1     |                                                   |
| serviceStopTime                |    0..1     |                                                   |
| size                           |  0..1 / 1   |                                                   | <!-- Reviewed -->
| sourcePatientId                |      1      |                                                   |
| sourcePatientInfo              |  0..1 / 0   | Ignored by the eMedication service                | <!-- Reviewed -->
| title                          |      1      |                                                   |
| typeCode                       |      1      |                                                   |
| uniqueId                       |      1      |                                                   |
| URI                            |    0 / 1    | It contains the MHD download link                 |
| version                        |  0..1 / 1   | Only "1"                                          | <!-- Reviewed -->

Mapping per document type are given in [the ITI-41 page](iti41.md).

**Value sets**:

1. [author.authorRole](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.authorRole.html)
1. [author.authorSpecialty](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.authorSpeciality.html)
1. [deletionStatus](https://fhir.ch/ig/ch-epr-mhealth/ValueSet-ch-ehealth-valueset-deletionstatus.html) <!-- TODO https://github.com/hl7ch/ch-epr-term/issues/11 -->
1. [eventCodeList](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.eventCodeList.html)
1. [healthcareFacilityTypeCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.healthcareFacilityTypeCode.html)
1. [languageCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.languageCode.html)
1. [originalProviderRole](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.originalProviderRole.html)
1. [practiceSettingCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.practiceSettingCode.html)
