# ITI-41

The [ITI-41 (Provide and Register Document Set-b)](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html) transaction allows a [content sender](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2) to send documents to the eMedication service ([content receiver](https://profiles.ihe.net/ITI/TF/Volume2/ITI-41.html#3.41.2)). 

Generic DocumentEntry metadata are described in the [documents section](documents.md). This page references additional constraints specific to ITI-41. See also IHE's documentation for [metadata in Document Sharing profiles](https://profiles.ihe.net/ITI/TF/Volume3/index.html).

## Publishing documents
### Document types
Two types of documents might be published to the service :
- [CH-EMED-EPR](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/) documents. These documents contain the eMedication data. See also the [CH-EMED-EPR](../emed/index.md) in this guide.
- [APPC (Advanced Patient Privacy Consent)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf) documents. These documents are used to let patients define who can see their data. See also the [APPC page](../appc/index.md) in this guide.
### SubmissionSet and relations
The eMedication service does not support folders and can only receive ONE document at a time. Hence, the [metadata](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1) must always contain a [SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.1) with a single [DocumentEntry](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.1.3).

Only the following [associations](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2) are supported :
|Association Type|Usage|
|-------------------|-------|
|[urn:oasis:names:tc:ebxml-regrep:AssociationType:HasMember](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.2.1.1)|[To link a DocumentEntry to the SubmissionSet](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.1).|
|[urn:ihe:iti:2007:AssociationType:RPLC](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#t4.2.2-1)|[To replace a previously published document]((https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2)). Replaced document becomes deprecated.|

### Uniqueness
Documents can be send to the eMedication service only once. Uniqueness is checked against [DocumentEntry's unique Id](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.26), that must be an UUID.

### Metadata
This section describes the rules applicable for any document's type metadata.
#### SubmissionSet metadata
See also the [SubmissionSet Metadata Attributes diagram](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.1.2).
|Metadata|Mandatory|Rules|
|--------|---------|-----|
|`uniqueId`|yes|Must be a UUID. Can be used only once and must be globally unique.|
|`author.authorRole`|yes|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorRole.html). For APPC, only `PAT` (patient) and `REP` (Representative) are allowed.|
|`author.authorSpecialty`|no|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorSpeciality.html).|
|`availabilityStatus`|no|The value set in the metadata is always ignored, and replaced by `approved`.|
|`comments`|no|-|
|`contentTypeCode`|yes|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.mimeType.html). Use `application/fhir+json` or `application/fhir+xml` for CH-EMED-EPR documents, and `text/xml` for APPC.|
|`entryUUID`|yes|Must be globally unique.|
|`homeCommunityId`|no|Not used. Can be left blank or omitted.|
|`intendedRecipient`|no|Not used. Can be left blank or omitted.|
|`limitedMetadata`|no|Must be left bank or omitted.|
|`patientId`|yes|Must be a XAD-PID.|
|`sourceId`|yes|Globally unique and immutable OID identifier of the source.|
|`submissionTime`|yes|[HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) value in UTC.|
|`title`|no|-|

#### DocumentEntry metadata
See also the [DocumentEntry Metadata Attributes diagram](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.1.1).

|Metadata|Mandatory|Rules|
|--------|---------|-----|
|`author.authorRole`|yes|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorRole.html). For APPC, only `PAT` (patient) and `REP` (Representative) are allowed.|
|`author.authorSpecialty`|no|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.authorSpeciality.html).|
|`availabilityStatus`|no|The value set in the metadata is always ignored, and replaced by `approved`.|
|`classCode`|yes|See [CH-EMED-EPR](#ClassCode-metadata-to-use-for-each-document-type) and [APPC](#Publishing-a-PMP-APPC-document) sections below.|
|`comments`|no|-|
|`confidentialityCode`|yes|[Confidentiality level](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.5) in the DocumentEntry metadata is forced to [`Normal`](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.confidentialityCode.html).|
|`creationTime`|yes|[HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) value in UTC.|
|`deletionStatus`|no|If present, deletionStatus must be `urn:e-health-suisse:2019:deletionStatus:deletionNotRequested`.|
|`documentAvailability`|no|If present should be `Online`.|
|`entryUUID`|yes|Must be globally unique.|
|`eventCodeList`|no|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.eventCodeList.html).|
|`formatCode`|yes|See [CH-EMED-EPR](#ClassCode-metadata-to-use-for-each-document-type) and [APPC](#Publishing-a-PMP-APPC-document) sections below.|
|`hash`|no|Hash of the document content.|
|`healthcareFacilityTypeCode`|yes|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.healthcareFacilityTypeCode.html).|
|`homeCommunityId`|no|-|
|`languageCode`|yes|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.languageCode.html).|
|`legalAuthenticator`|no|-|
|`limitedMetadata`|no|Must be left bank or omitted.|
|`logicalId`|no|Shall be present when replacing a document.|
|`mimeType`|no|Possible values are defined [ch-epr-term IG](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.mimeType.html). Use `application/fhir+json` or `application/fhir+xml` for CH-EMED-EPR documents, and `text/xml` for APPC.|
|`objectType`|yes|Value is ignored and set to `Stable`.|
|`patientId`|yes|-|
|`practiceSettingCode`|yes|See the [list of available codes](http://fhir.ch/ig/ch-epr-term/2.0.9/ValueSet-DocumentEntry.practiceSettingCode.html).|
|`referenceIdList`|no|Must be omitted or empty as referencing external documents is not permitted (see [Referencing external documents](#Referencing-external-documents) section).|
|`repositoryUniqueId`|no|If the attribute `repositoryUniqueId` is present and does not correspond to the `repositoryUniqueId` containing the documents the request is refused.|
|`serviceStartTime`|no|Given as an [HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) in UTC. Shall be empty for APPC|
|`serviceStopTime`|no|Given as an [HL7 DTM](http://www.hl7.eu/refactored/dtDTM.html) in UTC. Shall be empty for APPC|
|`size`|no|-|
|`sourcePatientId`|yes|Any id known or unknown to the local MPI, EPR-SPID and SSN are forbidden.|
|`sourcePatientInfo`|no|Ignored by the service.|
|`title`|yes|-|
|`typeCode`|yes|See [section](#ClassCode-metadata-to-use-for-each-document-type) below.|
|`uniqueId`|yes|Must be globally unique and be a UUID. Must match the document id in the case of CH-EMED-EPR documents.|
|`URI`|no|-|
|`version`|no|If present shall be empty, the value is ignored and set by the document registry.|
|`DocumentEntry.patientId`|yes|Must match `SubmissionSet.patientId` and be a XAD-PID ([see above](#SubmissionSet-metadata)).|
|`DocumentEntry.creationTime`|yes|Must match document content creation time ([see below](#Metadata-values-check)).|
|`DocumentEntry.author`|yes|Must be aligned with document content author ([see below](#Metadata-values-check)).|


## Publishing a CH-EMED-EPR document
This section details the specific rules to follow when publishing [CH-EMED-EPR documents](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/).

 Only [MTP](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html), [PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html), [DIS](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html) and [PADV](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) docs can be published. [PML](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html) and [PMLC](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) can only be retrieved from the service. As a result, `Iti41QueryConverter.process->EmedFhirSpec.getDocumentType` returns `null` for `PML` and `PMLC` docs.

### Referencing external documents
When publishing CH-EMED-EPR documents, all references (from the key elements) shall be known to the eMedication service. All non-key elements are ignored by the eMedication service. 

This implies that the metadata cannot contain [Reference ids](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.28). This option is not supported, and submission of a reference to an existing document is forbidden.

### Rules for each type of document
#### MTP
* Treatment duration (defined in [dosage](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-dosage.html)) should be within the period of validity of the document (defined by [`DocumentEntry.serviceStatTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.19) and [`DocumentEntry.serviceStopTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.20) metadata).
#### PRE
* Effective time of [`documentationOf`](#FIXME) and [`serviceEvent`](#FIXME) has to be aligned with [`DocumentEntry.serviceStatTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.19) and [`DocumentEntry.serviceStopTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.20).
* Treatment duration (defined in [dosage](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-dosage.html)) should be within the period of validity of the document (defined by [`DocumentEntry.serviceStatTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.19) and [`DocumentEntry.serviceStopTime`](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3.2.20) metadata).
* Each PRE item should refer to an MTP item that must be a [available](#FIXME) [approved](#FIXME) [valid](#FIXME) [active](#FIXME) and for the same patient
    * Not blocking issues should not prevent publication but should be notified to service administration to address the resulting ad-hoc issue    
* [Validity of document](#FIXME) has to be after the [date of the PRE document](#FIXME) and the [start date of duratio](#FIXME)n of treatment of parent MTP 
* [Code of medication](#FIXME) should match the code of medication of referenced MTP.      
* If [lot number](#FIXME) is specified in referenced MTP, the PRE item mast match it.
#### PADV
##### PADV against a [provisional PRE](#FIXME)
* Medication cannot be changed.
* Dosage instructions cannot be changed.
* Substitution cannot be disallowed if the existing dispense includes a substitution.
* Observation code `REFUSE` cannot be issued.
#####  PADV `CANCEL`
* Not allowed against DIS or other PADV documents.

#### DIS
* Dispense [status](#FIXME) has to be coherent with previous DIS documents for the same prescription.
* If [lot number](#FIXME) is specified in parent MTP or PRE, lot number must match.
##### DIS against PRE:
* Medication cannot be different if substitution is not allowed by PRE. 
    * Verification Is performed on the [GTIN](#FIXME) of the medication (not of the package). 
    * If a substitution other than `none` is specified, no validation is performed.
* Dispense cannot occur after the [end of validity of the PRE](#FIXME) document.
* Dispense cannot occur after the [end of the treatment](#FIXME) if specified in the [prescription item](#FIXME).
* If referenced PRE is not [provisional](#FIXME), it should be [dispensable](#FIXME).
* List of active ingredients should be the same than the one in the [parent PRE](#FIXME).
* If substitution occurs, the [medication code (GTIN)](#FIXME) should be the same than the medication code from the [parent PRE](#FIXME).
##### Against MTP:
* Medication cannot be different if [substitution](#FIXME) is not allowed by the MTP item. Verification is done on medication's [GTIN](#FIXME).
* Dispense cannot occur after the [end of the treatment](#FIXME) if specified in the MTP item.
* Referenced MTP must be [approved](#FIXME) and [active](#FIXME) and for the same patient.
* [List of active ingredients](#FIXME) should be the same than the one in the [parent MTP](#FIXME).
* If substitution occurs, the [medication code (GTIN)](#FIXME) should be the same than the medication code from the [parent MTP](#FIXME).
### Roles
* [Patients](#FIXME) and their [representatives](#FIXME) are allowed to publish only the following type of docs:
    * MTP
    * DIS
    * PADV with a COMMENT observation code with no restriction.
    * PADV with any other observation code only when referencing a document published by the same patient or a representative of the same patient.     
* [Healthcare providers](#FIXME) are allowed to publish only the following types of docs:
    * MTP
    * PRE
    * DIS
    * PADV		
* [Creation time of the document](#FIXME) must be equal or greater than the creation time of the last document of the [treatment chain](#FIXME).
    * Creation time of the document has to be in the past.
    * All references to other documents or items have to refer to existing approved non deleted documents.

### Metadata values check
Some values from the DocumentEntry are checked against the document (values in metadata and document must be equal):

| DocumentEntry | CH-EMED-EPR                                                  |
| ------------- | ------------------------------------------------------------ |
| author        | `Composition.author`                                         |
| classCode     | ``                                           |
| creationTime  | `Composition.date`                                           |
| creationTime  | `Composition.date`                                           |
| formatCode    | ``                                           |
| languageCode  | `Composition.language`                                       |
| mimeType      | `application/fhir+xml` or `application/fhir+json`            |
| typeCode      | ``            |
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

The class and type codes are from the SNOMED CT system: `2.16.840.1.113883.6.96`.

## Publishing a PMP-APPC document
This section details the rules applicable to metadata when publishing APPC documents.

### Rules for APPC:
* Only patients and their representatives or policy administrators can publish a new APPC document.
	* representatives are not supported yet by the service.
* No APPC for the specified patient exists in the system, otherwise refusal.
* APPC document structure described in specs §7.5
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
