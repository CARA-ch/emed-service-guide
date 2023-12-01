# ITI-41

The [ITI-41 (Provide and Register Document Set-b)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html) transaction allows a [content sender](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2) to send documents to the eMedication service ([content receiver](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2)). 

Generic [DocumentEntry metadata](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.1.1) are described in the [documents section](documents.md). This page references additional constraints specific to ITI-41. 

See also IHE's documentation for [metadata in Document Sharing profiles](https://profiles.ihe.net/ITI/TF/Volume3/index.html) as well as [EPD by example's Provide and Register Document Set](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/ProvideAndRegister.md) page.

## Publishing documents
### Document types

Two types of documents might be published to the service:

* [CH-EMED-EPR](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/) documents ([MTP](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html), [PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html), [DIS](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html) and [PADV](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) only, [PML](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html) and [PMLC](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) can only be retrieved from the service and not published to it). These documents contain the eMedication data. See also the [corresponding page](../emed/index.md) in this guide.
* [APPC (Advanced Patient Privacy Consent)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf) documents. These documents are used to let patients define access rights to their data (who can publish and retrieve their data). See also the [corresponding page](../appc/index.md) in this guide.

### SubmissionSet and relations
The eMedication service does not support folders and can only receive ONE document at a time. Hence, the [metadata](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1) must always contain a [SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.1) with a single [DocumentEntry](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.3).

Only the following [associations](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2) are supported:

|Association Type|Usage|
|-------------------|-------|
|[urn:oasis:names:tc:ebxml-regrep:AssociationType:HasMember](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.2.1.1)|[To link a DocumentEntry to the SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.1).|
|[urn:ihe:iti:2007:AssociationType:RPLC](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#t4.2.2-1)|[To replace a previously published document](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2). Replaced document becomes deprecated.|

### Uniqueness
Documents can be sent to the eMedication service only once. Uniqueness is checked against [DocumentEntry's unique Id](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.26), that must be an UUID and [DocumentEntry's entryUUID](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.7).

### Metadata
This section describes the rules applicable for any document's type metadata.
#### SubmissionSet metadata
See also the [SubmissionSet Metadata Attributes diagram](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.1.2).

|Metadata|Opt|Rules|
|--------|---------|-----|
|`uniqueId`|R|Must be a UUID. Can be used only once and must be globally unique.|
|`author`|R|Multiple values accepted.|
|`author.authorRole`|R|Possible values are defined in [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorRole.html). For APPC, only `PAT` (patient) and `REP` (Representative) are allowed.|
|`author.authorSpecialty`|O|Possible values are defined in [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorSpeciality.html).|
|`availabilityStatus`|O|The value set in the metadata is always ignored, and replaced by `approved`.|
|`comments`|O|-|
|`contentTypeCode`|R|Possible values are defined in [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.mimeType.html). Use `application/fhir+json` or `application/fhir+xml` for CH-EMED-EPR documents, and `text/xml` for APPC.|
|`entryUUID`|R|Must be globally unique.|
|`homeCommunityId`|O|Not used. Can be left blank or omitted.|
|`intendedRecipient`|O|Not used. Can be left blank or omitted.|
|`limitedMetadata`|O|Must be left bank or omitted.|
|`patientId`|R|Must be a XAD-PID.|
|`sourceId`|R|Globally unique and immutable OID identifier of the source.|
|`submissionTime`|R|[HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) value in UTC.|
|`title`|O|-|

#### DocumentEntry metadata
See also the [DocumentEntry Metadata Attributes diagram](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.1.1).

|Metadata|Opt|Rules|
|--------|---------|-----|
|`author.authorRole`|R|Possible values are defined in [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorRole.html). For APPC, only `PAT` (patient) and `REP` (Representative) are allowed. Must be aligned with document content author ([see below](#metadata-values-check)).|
|`author.authorSpecialty`|O|Possible values are defined in [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorSpeciality.html). Must be aligned with document content author ([see below](#metadata-values-check)).|
|`availabilityStatus`|O|The value set in the metadata is always ignored, and replaced by `approved`.|
|`classCode`|R|See [CH-EMED-EPR](#classcode-metadata-to-use-for-each-document-type) and [APPC](#publishing-a-pmp-appc-document) sections below.|
|`comments`|O|-|
|`confidentialityCode`|R|[Confidentiality level](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.5) in the DocumentEntry metadata is forced to [`Normal`](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.confidentialityCode.html).|
|`creationTime`|R|[HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) value in UTC. Must match document content creation time ([see below](#metadata-values-check)).|
|`deletionStatus`|O|If present, deletionStatus must be `urn:e-health-suisse:2019:deletionStatus:deletionNotRequested`.|
|`documentAvailability`|O|If present should be `Online`.|
|`entryUUID`|R|Must be globally unique.|
|`eventCodeList`|O|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.eventCodeList.html).|
|`formatCode`|R|See [CH-EMED-EPR](#classcode-metadata-to-use-for-each-document-type) and [APPC](#publishing-a-pmp-appc-document) sections below.|
|`hash`|O|Hash of the document content.|
|`healthcareFacilityTypeCode`|R|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.healthcareFacilityTypeCode.html).|
|`homeCommunityId`|O|-|
|`languageCode`|R|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.languageCode.html).|
|`legalAuthenticator`|O|-|
|`limitedMetadata`|O|Must be left bank or omitted.|
|`logicalId`|O|Shall be present when replacing a document.|
|`mimeType`|O|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.mimeType.html). Use `application/fhir+json` or `application/fhir+xml` for CH-EMED-EPR documents, and `text/xml` for APPC.|
|`objectType`|R|Value is ignored and set to `Stable`.|
|`patientId`|R|Must match `SubmissionSet.patientId` and be a XAD-PID ([see above](#submissionset-metadata)).|
|`practiceSettingCode`|R|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.practiceSettingCode.html).|
|`referenceIdList`|O|Must be omitted or empty as referencing external documents is not permitted (see [Referencing external documents](#referencing-external-documents) section).|
|`repositoryUniqueId`|O|If the attribute `repositoryUniqueId` is present and does not correspond to the `repositoryUniqueId` containing the documents the request is refused.|
|`serviceStartTime`|O|Given as an [HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) in UTC. Shall be empty for APPC|
|`serviceStopTime`|O|Given as an [HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) in UTC. Shall be empty for APPC|
|`size`|O|-|
|`sourcePatientId`|R|Any id known or unknown to the local MPI. EPR-SPID and SSN are forbidden.|
|`sourcePatientInfo`|O|Ignored by the service.|
|`title`|R|-|
|`typeCode`|R|See [section](#classcode-metadata-to-use-for-each-document-type) below.|
|`uniqueId`|R|Must be globally unique and be a UUID. Must match the document id in the case of CH-EMED-EPR documents.|
|`URI`|O|-|
|`version`|O|If present shall be empty, the value is ignored and set by the document registry.|

## Publishing a CH-EMED-EPR document
This section details the specific rules to follow when publishing [CH-EMED-EPR documents](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/).

 Only [MTP](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html), [PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html), [DIS](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html) and [PADV](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) docs can be published. [PML](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html) and [PMLC](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) can only be retrieved from the service.

### Referencing external documents
When publishing CH-EMED-EPR documents, all references (from the key elements) shall be known to the eMedication service. All non-key elements are ignored by the eMedication service. 

This implies that the metadata cannot contain [Reference ids](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.28). This option is not supported, and submission of a reference to an existing document is forbidden.

### Creation times
[Creation time](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-composition-medicationtreatmentplan-definitions.html#Composition.date) of a document must be equal or greater than the creation time of the last document of the treatment chain, ie. the last published document targeting the treatment (either the original MTP if no other document, or the last document published referencing this treatment).

* Creation time of the document has to be in the past.
* All references to other documents or items have to refer to existing approved non deleted documents.

### Rules for each type of document
#### MTP
* The [medication referenced in the MTP](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationstatement-treatmentplan.html) (`CHEMEDEPRMedicationStatement`) should follow some rules. These are detailed in the implementation guide's [treatment guidance page](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/guidance_treatment.html).
#### PRE
* Each PRE item should [refer to an MTP item](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-document-medicationprescription.html) (`CHEMEDEPRMedicationRequest.treatmentplan`) that must:
    * Exist (has already been published, and never deleted)
    * Be [approved](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.2), ie. `DocumentEntry.availabilityStatus = approved`, 
    * Preferably be valid, i.e. the submission date should be within the [MTP `dosage`'s](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-dosage.html) `boundsPeriod` (`CHEMEDEPRDosage.repeat.bounds`).
        * If the `boundsPeriod` is not defined, the entry is considered to be always active.
        * If only a [`startDate`](http://hl7.org/fhir/R4/datatypes-definitions.html#Period.start) is specified, the MTP is considered to be active if the current date is after that date.
    * Active, ie. `CHEMEDEPRDocumentMedicationTreatmentPlan.CHEMEDEPRMedicationStatement.status = active`
    * For the same patient.
* Code of [medication](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medication.html) should match the code of medication of the [MTP referenced by the PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-document-medicationprescription.html) (`CHEMEDEPRMedicationRequest.treatmentplan`). _This rule is not enforced and might be extended in the future_.    
* If [lot number](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medication.html) (`CHEMEDEPRMedication.batch`) is specified in referenced MTP, [the one in the PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationrequest.html) (`CHEMEDEPRMedicationRequest.medication[x]:medicationReference.batch`) should match it.  _This rule is not enforced and might be extended in the future_.
#### PADV
##### PADV against a provisional PRE
A "provisional PRE" is a prescription for which the [`CHEMEDEPRMedicationRequest.status`](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationrequest.html) is `SUBMITTED` or `PROVISIONAL` and for which a `PADV` `OK` has not been completed yet.

* Medication cannot be changed. _This rule is not enforced and might be extended in the future_.
* Dosage instructions cannot be changed.  _This rule is not enforced and might be extended in the future_.
* Substitution cannot be disallowed if the existing dispense includes one.  _This rule is not enforced and might be extended in the future_.
* Observation code `REFUSE` cannot be issued.
##### PADV CANCEL
* Sets status to `CANCELED`.
* Not allowed against `DIS` or other `PADV` documents.
###### Against MTP
* Also set related prescriptions' status to `CANCELED` if their status was not already `REFUSED` or `CANCELED`.
* Treatment plan status must be `ACTIVE` or `SUSPENDED`.
###### Against PRE
* Not allowed for prescriptions whose status is already `CANCELED` or `REFUSED`.
* Not allowed to target anything else than `MTP` or `PRE`
##### PADV OK 
* Sets targeted `MTP` or `PRE` status to `ACTIVE`.
* Against `MTP`: only allowed for treatment plans with status `ACTIVE` or `SUSPENDED`.
* Against `PRE`: only allowed for prescriptions with status `SUBMITTED` or `PROVISIONAL`.
* `PADV OK` cannot target anything else than `MTP` or `PRE`.
##### PADV CHANGE 
* Changes the treatment plan or prescription.
* Can only target `MTP` or `PRE`.
###### Against MTP
* The treatment status must be `ACTIVE` or `SUSPENDED`.
* The treatment must not have been prescribed.
* Additionally, the treatment's status is set to `ACTIVE` as a result of the aggregation.
##### Against PRE
* The prescription's status must not be `CANCELED` or `REFUSED`.
* If the prescription's status is `SUBMITTED`, the aggregation sets it to `ACTIVE` as a result.
##### PADV REFUSE 
* Sets status to `REFUSED`.
###### Against MTP
* Sets also related prescriptions' status to `REFUSED` if the status was not already `REFUSED` or `CANCELED`.
* Treatment's status must be `ACTIVE` or `SUSPENDED`.
###### Against PRE
* The prescription's status must not be `REFUSED`, `CANCELED` or `PROVISIONAL`.
* Cannot target anything else than `MTP` or `PRE`.
##### PADV SUSPEND 
* Sets status to `SUSPENDED`
* Only allowed to target `MTP` entries
* The treatment status must be `ACTIVE`
##### PADV COMMENT: 
* Can target `MTP`, `PRE` and `DIS` entries only.

#### DIS
* If [lot number](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medication.html) (`batch`) is specified in referenced [MTP(`treatmentPlan`) or PRE (`prescription`)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationdispense.html), then it should match the one in the DIS if substitution is not allowed.  _This rule is not enforced and might be extended in the future_.
##### DIS against MTP or PRE + MTP:
* Medication should not be different if substitution is not allowed by PRE. _This rule might be extended in the future_. 
* Dispense should not occur after the end of validity of the PRE document. _This rule might be extended in the future_.
* Dispense should not occur after the end of the treatment if specified in the prescription item. _This rule might be extended in the future_.
* If referenced PRE is not provisional, it should be `dispensable` (that is have status either `PROVISIONAL` or `ACTIVE`). _This rule is not enforced and might be extended in the future_.
* List of active [ingredients](https://build.fhir.org/ig/hl7ch/ch-emed//StructureDefinition-ch-emed-medication-medicationdispense-definitions.html#Medication.ingredient) should be the same than the one in the referenced [PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationdispense.html) (`prescription`). _This rule is not enforced and might be extended in the future_.
* If the treatment has been prescribed (at least one PRE document has been successfully submitted for it), then any DIS document targeting this treatment must reference one of its prescriptions as well.

### Roles
* [Patients](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-PAT) and their [representatives](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-REP) are allowed to publish only the following type of docs:
    * MTP
    * PADV with a `COMMENT` observation code with no restriction.
    * PADV with any other observation code only when referencing a document published by the same patient or a representative of the same patient.     
* [Healthcare professionals](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-HCP) are allowed to publish only the following types of docs:
    * MTP
    * PRE
    * DIS
    * PADV		


### Metadata values check
Some values from the DocumentEntry are checked against the document (values in metadata and document must be equal):

| DocumentEntry | CH-EMED-EPR                                                  |
| ------------- | ------------------------------------------------------------ |
| author        | `Composition.author`                                         |
| classCode     | `Composition.type`.  Must be consistent with the document type (see [below](#classcode-metadata-to-use-for-each-document-type))|
| creationTime  | `Composition.date`                                           |
| formatCode    | Must be consistent with the document type (see [below](#classcode-metadata-to-use-for-each-document-type))|
| languageCode  | `Composition.language`                                       |
| mimeType      | `application/fhir+xml` or `application/fhir+json`            |
| typeCode      |  Must be consistent with the document type (see [below](#classcode-metadata-to-use-for-each-document-type))|
| uniqueId      | `Bundle.identifier.value` and `Composition.identifier.value` |

### ClassCode metadata to use for each document type
| Document type | DocumentEntry.classCode                   | DocumentEntry.formatCode                   | DocumentEntry.typeCode      |
| ------------- | ----------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| MTP           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:mtp:2022`             |`761931002` _Medication treatment plan report (record artifact)_ |
| PRE           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:pre:2022`             | `761938008` _Medicinal Prescription record (record artifact)_ |
| DIS           | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:dis:2022`             | `294121000195110` _Medication dispense document (record artifact)_ |
| PADV          | `440545006` _Prescription record_       | `urn:che:epr:ch-emed:padv:2022`            | `419891008` _Record artifact_                                 |
| PML           | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:pml:2022`             | `721912009` _Medication summary document_                     |
| PMLC          | `422735006` _Summary clinical document_ | `urn:che:epr:ch-emed:medication-card:2022` |`736378000` _Medication management plan (record artifact)_ |

The class and type codes are from the SNOMED CT system: `2.16.840.1.113883.6.96`. The system for the [`formatCode`](https://fhir.ch/ig/ch-epr-term/CodeSystem-2.16.756.5.30.1.127.3.10.10.html) is `2.16.756.5.30.1.127.3.10.10`

## Replacing a CH-EMED-EPR document

The following rules must be observed when replacing a document:

* [Patients](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-PAT) and their [representatives](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-REP) can only replace a document published by the same patient.
	* representatives are not supported by the service yet.
* [Healthcare professionals](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-HCP) (HCP) can only replace a document published by himself or by another HCP of same community of affiliation.
    * Current implementation: an HCP can replace a document published by any other HCP. Further rules and possible implementations are under discussion.
* [Document administrator](https://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html) may only replace a document published by an HCP of the same community of affiliation.
	* Not implemented in PMP, to be discussed, for now a document admin can replace any document.
* Replacing and replaced documents have to be of the same type (e.g. both MTP documents).
* Replacing and replaced documents have to refer to the same patient.
* Only documents at the end of a treatment chain can be replaced.
* Replaced document must exist in the repository.
* A replacement document must can be linked only to elements of the same medication chain. In case a PRE document is replaced, it may be linked only to elements of the medication chain of the referenced MTP. 
* [Document administrator](https://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html) can replace any document regardless of who published it.
* Replaced document must be approved (although this is always the case after an initial ITI-41 transaction, documents might become `deprecated` after another ITI-41 transaction with a [replacement association]((https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2))).
* Replaced document must not have `deletionStatus = deletionRequested` (this is relevant since although it is not the case at the moment with the current implementation, there might be files in the repository flagged for deletion but not deleted yet).

## Publishing a PMP-APPC document
This section details the rules applicable to metadata when publishing [APPC documents](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf).

### Rules for APPC
* Only  [Patients](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-PAT) and their [representatives](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-REP) or policy administrators can publish a new APPC document.
	* representatives are not supported by the service yet.
*  Only [Patients](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-PAT) and their [representatives](http://fhir.ch/ig/ch-epr-term/2.0.9/CodeSystem-2.16.756.5.30.1.127.3.10.6.html#2.16.756.5.30.1.127.3.10.6-REP) can replace their own APPC documents (if it exists).
	* representatives are not supported by the service yet.
* Policy administrators of the reference community of the patient can replace any existing APPC document.
* No APPC for the specified patient exists in the system (unless the published document is a replacement), otherwise the document is refused.
* As for [CH-EMED-EPR](#replacing-a-ch-emed-epr-document), APPC documents to be replaced must:
    * Be approved (as before, this is always the case for documents in the PMP repository when they are published, but their status might change to deprecated after a replacement).
    * Not have `deletionStatus = deletionRequested`.
* APPC document structure is described in [Implementation Guide for eMedication architecture in the context 
of the EPR](https://www.e-health-suisse.ch/fileadmin/user_upload/Dokumente/E/Implementation_Guide_eMedication_Architecture_Specs_013_Anhoerung.pdf), section §7.5
    * `1 Description` section for free text description of the policy set
    * `1 Target` section containing:
		* `1 Resources` section containing `1 Resource` object pointing to the patient by indicating the `EPR-SPID`.
		* `* Subjects` sections each one containing themselves `Subject` entries each defining 1 "who". Subject cannot be the patient.
		* `* Environments` sections each one containing one `Environment` object limiting the validity period of the policy.
		    * Required if at least one `Subject` refers to an organization or group.
				* Specs state "This section will not be used within this context."
		* A list of `PolicyIdReference` or `PolicySetIdReference` defining the level of access granted to the users identified by the Subjects section.
			* See 7.5 for the list of policies that can be used.
		* `*` embedded `PolicySet` sections containing:
			* `1..* Subjects` sections containing themselves `Subject entries` and each defines one "who".
			* `* Environments` (same as above)

### APPC metadata
| DocumentEntry    | APPC                                           |
| ---------------- | ---------------------------------------------- |
| classCode        | `371537001` _Consent report (record artifact)_ |
| formatCode       | `urn:ihe:iti:appc:2016:consent`                |
| mimeType         | `text/xml`                                     |
| serviceStartTime | None                                           | <!-- TODO: now? -->
| serviceStopTime  | None                                           |
| typeCode         | `419891008` _Record artifact_                  |
