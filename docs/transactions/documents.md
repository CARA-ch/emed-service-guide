# Documents

## Content

The eMedication service only support [CH-APPC](../chappc/index.md) and [CH-EMED](../chemed/index.md) documents.

## Metadata

When two cardinalities are given, the first one is for Content Sender actors and the second one for the Document Repository actor.

| DocumentEntry                  | Cardinality | APPC         | CH-EMED                                                      | Comment                                           |
| ------------------------------ | :---------: | ------------ | ------------------------------------------------------------ | ------------------------------------------------- |
| author                         |      M      |              |                                                              |                                                   |
| author.authorInstitution       |      M      |              | Mapping                                                      |                                                   |
| author.authorPerson            |      1      |              | Mapping                                                      |                                                   |
| author.authorRole              |      1      |              |                                                              |                                                   |
| author.authorSpecialty         |    0..?     |              |                                                              |                                                   |
| author.authorTelecommunication |      M      |              | Mapping                                                      |                                                   |
| availabilityStatus             |  0..1 / 1   |              |                                                              | Only "Approved" for Content Senders               | <!-- Reviewed -->
| classCode                      |      1      | Single value | Mapping                                                      |                                                   |
| comments                       |    0..1     |              |                                                              |                                                   |
| confidentialityCode            |      1      |              |                                                              | Only "Normal"                                     | <!-- Reviewed -->
| creationTime                   |      1      |              |                                                              |                                                   |
| deletionStatus                 |  0..1 / 1   |              |                                                              | Only "deletion not requested" for Content Senders |
| documentAvailability           |  0..1 / 1   |              |                                                              | Only "Online"                                     | <!-- Reviewed -->
| entryUUID                      |      1      |              | `Bundle.identifier.value` and `Composition.identifier.value` |                                                   |
| eventCodeList                  |    0..*     |              | Mapping                                                      |                                                   |
| formatCode                     |      1      | Single value | Mapping                                                      |                                                   |
| hash                           |  0..1 / 1   |              |                                                              |                                                   | <!-- Reviewed -->
| healthcareFacilityTypeCode     |      1      |              |                                                              |                                                   |
| homeCommunityId                |  0..1 / 1   |              |                                                              |                                                   |
| languageCode                   |      1      |              | `Composition.language`                                       |                                                   |
| legalAuthenticator             |    0..1     |              |                                                              |                                                   |
| limitedMetadata                |      0      |              |                                                              |                                                   |
| logicalID                      |  0..1 / 1   |              | `Bundle.identifier.value` and `Composition.identifier.value` | Same value as the `entryUUID`                     |
| mimeType                       |      1      | `text/xml`   | `application/fhir+xml` or `application/fhir+json`            | Single value                                      |
| objectType                     |      1      |              |                                                              | Only "stable" for Content Senders                 | <!-- Reviewed -->
| originalProviderRole           |    TODO     |              |                                                              |                                                   |
| patientId                      |      1      |              |                                                              | MPI-PID or EPR-SPID?                              |
| practiceSettingCode            |      1      |              |                                                              |                                                   |
| referenceIdList                |  0..* / 0   |              |                                                              | Ignored by the eMedication service                | <!-- Reviewed -->
| repositoryUniqueId             |  0..1 / 1   |              |                                                              |                                                   |
| serviceStartTime               |    0..1     | None         | Mapping                                                      |                                                   |
| serviceStopTime                |    0..1     | None         | Mapping                                                      |                                                   |
| size                           |  0..1 / 1   |              |                                                              |                                                   | <!-- Reviewed -->
| sourcePatientId                |      1      |              | Mapping                                                      |                                                   |
| sourcePatientInfo              |  0..1 / 0   |              |                                                              | Ignored by the eMedication service                | <!-- Reviewed -->
| title                          |      1      |              |                                                              |                                                   |
| typeCode                       |      1      | Single value | Mapping                                                      |                                                   |
| uniqueId                       |      1      |              | Mapping. UUID.                                               |                                                   |
| URI                            |    0 / 1    |              |                                                              |                                                   | <!-- TODO MHD download link? -->
| version                        |  0..1 / 1   |              |                                                              | Only "1"                                          | <!-- Reviewed -->

**Value sets**:

1. [healthcareFacilityTypeCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.healthcareFacilityTypeCode.html)
2. [languageCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.languageCode.html)
4. deletionStatus <!-- TODO https://github.com/hl7ch/ch-epr-term/issues/11 -->
5. [eventCodeList](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.eventCodeList.html)
6. [practiceSettingCode](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.practiceSettingCode.html)
7. [originalProviderRole](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.originalProviderRole.html)
8. [author.authorRole](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.authorRole.html)
9. [author.authorSpecialty](http://fhir.ch/ig/ch-epr-term/ValueSet-DocumentEntry.authorSpeciality.html)
