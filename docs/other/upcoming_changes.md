!!! warning 

    Note that possible changes on Husky or other libraries/dependencies are not listed here and it is up to integrators to keep track of them.

## Currently Deployed

  - dev:
    - PMP (aggregator) v0.4.13 (deployed 2024-07-26, DB recreated with v0.4.0 deployed 2024-04-23), works with [CH EMED EPR 1.0.0](https://fhir.ch/ig/ch-emed-epr/index.html).
    - ALPAGE v0.0.6 (deployed 2024-07-26)
  - int:
    - PMP (aggregator) v0.3.0 (deployed ~2024-04-15, DB recreated), works with [CH EMED EPR 1.0.0](https://fhir.ch/ig/ch-emed-epr/index.html).
    - ALPAGE v0.0.3 (deployed ~2024-04-15 due to VM migration, same version as prev. VM, DB recreated)


## Next Release Dates

- Next CH EMED EPR version release: *TBD*
- Next aggregator release: *TBD*.
- Next aggregator deployment: 0.5.0 to be deployed in both dev and int environments:
	- int: 2025-05-21
	- dev: 2025-05-26

## Relevant Changes
### PMP v0.5.0 (released 2025-05-21)
- Works with the newly released [CH EMED EPR 2.0.0](https://fhir.ch/ig/ch-emed-epr/2.0.0), see the CH EMED EPR 2.0.0 [changelog](https://fhir.ch/ig/ch-emed-epr/2.0.0/changelog.html) for detailed changes:
	- Units: 
		- Added `Bq`, `kBq`, `MBq`, `GBq`, `nmol`, `413568008` (_Application_) and `246205007` (_Quantity_).
		- `{Piece}` has been removed, please use the SNOMED `246205007` (_Quantity_) code instead.
		- Added [concept maps](https://fhir.ch/ig/ch-emed-epr/2.0.0/artifacts.html#5) to provide guidance to map to/from HCI's CdTyp9 unit codes. Please bear in mind that these are provided on a best effort basis and that they are by no means of normative value and in no way sanctioned by eHealthSuisse or any other entity.
		- A new invariant has been added to all medication statement, medication request and medication dispense profiles to enforce that all dosage elements within each one of said resources uses the same unit. _E.g._ if the base dosage element specifies a dose of 1 tablet for the morning, then a split dosage element cannot say 2g in the evening.
	- PMLC:
		- Medication statements now contain a new `lastConsideredDocument` extension, always filled by the aggregator. This extension contains the identifier of the last document that was used for aggregating data resulting in this PMLC medication statement. This includes PADV comments. This extension might be of use for implementers wishing to _detect_ whether new changes or comments have been performed on a treatment line since last import.
	- PML:
		- Changed medication request resources and changed medication statement resources added by the aggregator to a PML now conform to the new [`CHEMEDEPRMedicationRequestChangedList`](https://fhir.ch/ig/ch-emed-epr/2.0.0/StructureDefinition-ch-emed-epr-medication-request-changed-list.html) and [`CHEMEDEPRMedicationStatementChangedList`](https://fhir.ch/ig/ch-emed-epr/2.0.0/StructureDefinition-ch-emed-epr-medicationstatement-changed-list.html) profiles.
	- Misc:
		- Added a new warning invariant to all composition profiles that will check whether the composition title is correct according to the specified composition's language.
		- Added a constraint to all base dosages ([CHEMEDEPRDosage](https://fhir.ch/ig/ch-emed-epr/2.0.0/StructureDefinition-ch-emed-epr-dosage.html) and [CHEMEDEPRDosageMedicationRequest](https://fhir.ch/ig/ch-emed-epr/2.0.0/StructureDefinition-ch-emed-epr-dosage-medicationrequest.html)) that will result in a warning if the `.text` element is missing or blank.
		- Examples have been added and some updated.
		- Added a new guidance page on [comments/notes](https://fhir.ch/ig/ch-emed-epr/2.0.0/guidance_comments.html).
		- Fixed the `time-quantity-only-integer` constraint on the [CHEMEDEPRTimeQuantity](https://fhir.ch/ig/ch-emed-epr/2.0.0/StructureDefinition-ch-emed-epr-time-quantity.html) profile that was effectively preventing a structured max dose (`maxDosePerPeriod`) from being used.
- Added support for the following transactions:
	- CH EPR FHIR (MHD) (XUA token only, for now):
		- [ITI-65 (Provide Document Bundle)](../transactions/iti65.md)
		- [ITI-67 (Find Document References)](../transactions/iti67.md)
		- [ITI-68 (Retrieve Document)](../transactions/iti68.md)
		- [CH:MHD-1 (Update Document Metadata)](../transactions/chmhd1.md)
		- [CH:ATC (Audit Trail Consumption)](../transactions/chatc.md)
	- [CH:PHARM-5](../transactions/chpharm5.md) (XUA token only, for now)
	- PIXm (no token):
		- [ITI-83 (Mobile Patient Identifier Cross-reference Query)](../transactions/iti83.md)
		- [ITI-104 (Patient Identity Feed)](../transactions/iti104.md)
	- [RESTful ATNA Feed](../transactions/iti20.md)
- PML documents are now properly generated for treatments containing changed medication requests or changed medication statements.
- PMLC PDF:
	- The aggregator allows clients to request the PMLC PDF to be in eMediplan format. See the [CH:PHARM-1](../transactions/chpharm1.md) page for more information on this.
	- CARA's PDF format:
		- Improved display of dosage when quantity is provided but not the when.
		- Added display of active ingredients (if provided) under the medication name.
- Updated codes from the Swiss EPR to use the values from the 202406.2-stable release. See [ITI-41](../transactions/iti41.md) to verify some of the values.
- Added antivirus scan capabilities. PDFs attached to files uploaded to the eMedication service might now be scanned for threats before being accepted. This will work as follows depending on the different environments. For both ws-pmp-int and ws-pmp-dev, a local ClamAV service will be used for scanning. If the binary does not pass the scan or if the scan fails for whatever reason, the submission will be rejected.
- Misc. improvements, fixes and better audit log generation.

### PMP v0.4.13
Minor fixes and improved audit log generation.

### PMP v0.4.12
No relevant changes. Version deployed with a workaround for a problem with the routes of administration on PMLC generation while waiting for the next release of Husky to fix the issue. See https://github.com/project-husky/husky/issues/153

### PMP v0.4.9
- Fixed several bugs affecting the application of matching APPC policy sets (problems with access rights).
- All the notes received with any resource are now aggregated (e.g. a note provided with a `MedicationStatement` resource within an MTP document).
- All the aggregated comments/notes, for both medication treatment and medication treatment instance(s), will be now added to the PMLC medication statements. Previously only treatment comments were added to the PMLC.
- Fixed bug on aggregation of PADV CANCEL targeting a PRE, preventing the transaction from completing.
- Improved audit trail log generation for ITI-18 and PHARM-1 transactions (WIP for all transactions).

### PMP v0.4.8
The aggregator can enable and disable the application of APPC rules (for debugging purposes) with a restart of the service, no redeployment needed.

### PMP v0.4.5
The aggregator has again disabled the application of APPC rules to unblock integration tests in dev and until better debugging information is added to the affected code.

### PMP v0.4.4
The aggregator has reactivated the application of APPC rules to grant or deny access rights to the PMP. An exception has been kept for TCUs to always allow TCUs right of publication:

  - TCU access rules have not yet been defined by CARA.
  - Some systems like Presco have yet to transition from TCU publication to HCP publication (to be done before pilot phase).

### PMP v0.4.0
The PMP is abandoning the use of CARA's MPI-PID as XAD-PID and with the v0.4.0 starts a transition period towards the use of a PMP-PID (*PMP assigned patient id*) as XAD-PID in order to pave the road to support systems with patients from other reference communities. What this entails for PMP v0.4.0:

  - Patient registration:
    - Query: an ITI-45 query (added with v0.3.0) allows a system to know if a patient has a PMP registration (whether active or not) and to fetch the PMP-PID.
	- Add: to register a patient, the following steps will be needed:
	    1. Perform an ITI-44 *Patient Registry Record __Added__* (PIXV3 feed) to add the new patient to the PMP.
	    2. Fetch the PMP-PID with an ITI-45 query to the PMP.
	    3. Perform an ITI-41 with the APPC document to activate the registration. Until this is done, no other transaction for providing, fetching or searching documents will be accepted.
  - All requests (other than PIX) expect now the use of PMP-PID ids (SubmissionSet.patientId and DocumentEntry.patientId). Systems can continue to use CARA's MPI-PIDs for this and the aggregator will perform a translation but include a warning with the response. The grace period for transitioning towards PMP-PIDs has not been defined.
Note that PMP-PIDs will not be the same for the same patients in different environments, see [OIDs](oids.html) for the each deployed platform's patient identification domain id.

### PMP v0.3.0
Relevant changes from (upcoming) CH EMED EPR 1.0.0 based on CH EMED 4.0.0 (the latter should be published before the end of the current year):

- Authorship and authorship timestamps. Please refer to the [CH EMED authorship guidance page](https://build.fhir.org/ig/hl7ch/ch-emed/authorship.html) for further reading:
    - All eMed entries will **require** the relevant authorship and authorship timestamp fields to be filled (i.e. `1..1` cardinality):
        - `MedicationStatement` resources (MTP, PADV, PML, PMLC docs):
		    - `.informationSource` shall refer to the author of the medical decision.
		    - `.dateAsserted` shall specify the date of said medical decision (treatment plan).
	    - All `MedicationRequest` resources (PRE, PADV, PML docs):
		    - `.requester` shall refer to the author of the medical decision.
		    - `.authoredOn` shall specify the date of said medical decision (prescription).
	    - All `MedicationDispense` resources (DIS, PML docs):
		    - `.performer.actor` shall refer to the author of the medical decision (dispense).
		    - `.whenHandedOver` shall specify the date of the dispense.
	    - All `Observation` resources (PADV, PML docs):
		    - `.performer` shall refer to the author of the observation.
		    - `.issued` shall specify the date of the observation.
	- All `Composition` resources:
	    - No changes to the composition `.author`: this shall contain the author of the **document**, which may match any eMed entry's author but not necessarily (e.g. an assistant adding an MTP to the PMP on behalf of a practitioner would be the composition author while the practitioner would be the entry's author).
	    - eMed sections of `Composition` resources will no longer support the `.author` element, which previously would optionally specify a medical author if different from the document (i.e. composition) author.
	- PML and PMLC eMed entries shall continue (as it is already the case) to include the `authorDocument` extension if for the last aggregated entry the document author differs from the entry's author. Note that as per the rules above, the author of the entry (i.e. medical author) is always included in the entry.
- Changes to CH EMED's `UnitCode` value set:
	- Several UCUM annotations have been replaced by equivalent SNOMED codes, please refer to [CH EMED `UnitCode`](https://fhir.ch/ig/ch-emed/ValueSet-UnitCode.html)
        - `{Piece}` remains valid for now in the value set because the equivalent SNOMED term is missing, this will be solved for the next version of CH EMED when it should be replaced by a SNOMED code.
	- The UnitCode-based [`CHEMEDEPRAmountQuantityUnitCode`](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/ValueSet-ch-emed-epr-amount-quantity-unit-code.html) value set reflects those changes.
- Removed `N` from the [`CHEMEDEprActSubstanceAdminSubstitutionCode`](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/ValueSet-ch-emed-epr-amount-quantity-unit-code.html) value set. This affects `MedicationDispense` resources only:
	- The value set now contains only `E` as allowed code.
	- New CH EMED constraint: if no substitution has been performed, the `MedicationDispense.substitution.type` element SHALL NOT be present (see constraint `ch-emed-dis-1` on either the  CH EMED or CH EMED EPR resource definition).
- Te CH EMED EPR IG reflects now that the `prescription` extension of the `MedicationDispense` resource is supported. 
	- If the treatment has been prescribed, the aggregator's logic will continue to enforce that this extension is filled and that it references a valid prescription for the same treatment.
- For all eMed resources, `note.time` shall not be supported any more: it is assumed that the time is the same as for the entry's authorship.
	- With the exception of the PMLC `CHEMEDEPRMedicationStatementCard` that will continue to include `note.time` for each note.
- `MedicationRequest.validityPeriod` is now optional, i.e. cardinality is `0..1`.
    - If not specified, start is assumed to be the `MedicationRequest.authoredOn` date.
	- If not specified, end is assumed to be *the end of time*, i.e. the request remains valid indefinitely to the aggregator.


Other aggregator changes:

- `FindMedicationCard` queries will support the `ServiceStartFrom`, `ServiceStartTo`, `ServiceEndFrom`, `ServiceEndTo` parameters and apply them as follows:
    - The consolidated (i.e. aggregated) start date of returned treatments must be between the specified `ServiceStartFrom` and `ServiceStartTo` parameters.
        - If `ServiceStartFrom` is not specified,  any treatment starting before the specified `ServiceStartTo` will meet the service start criterium.
	    - If `ServiceStartTo` is not specified, any treatment starting at or after the specified `ServiceStartFrom` will meet the service start criterium.
	    - If neither `ServiceStartFrom` nor `ServiceStartTo` are specified, all treatments starting at any date will meet the service start criterium.
	    - Note that if a treatment does not have an explicit start date provided by a published document, it is assumed by the aggregator to be the date of creation of the MTP.
    - The consolidated end date of returned treatments must be between the specified `ServiceEndFrom` and `ServiceEndTo` parameters.
        - If `ServiceEndFrom` is not specified,  any treatment ending before the specified `ServiceEndTo` will meet the service end criterium.
	    - If `ServiceEndTo` is not specified, any treatment ending at or after the specified `ServiceEndFrom` will meet the service end criterium.
	    - If neither `ServiceEndFrom` nor `ServiceEndTo` are specified, all treatments ending at any date will meet the service end criterium.
	    - Note that if a treatment does not have an explicit end date provided by a published document, it is assumed by the aggregator to be *the end of time*, i.e. will have no end date.
- Implementation of XDS `ServiceStartFrom`, `ServiceStartTo`, `ServiceEndFrom`, `ServiceEndTo` criteria for all ITI-18 and PHARM-1 queries.
    - `ITI-18` queries will match the criteria ranges against the stored metadata `XDSDocumentEntry.serviceStartTime` and `XDSDocumentEntry.serviceStopTime` as provided at ITI-41 time. See [date processing](transactions/date_processing.md).
    - `PHARM-1` subqueries:
        - `FindMedicationTreatmentPlans`, `FindMedicationList` and `FindMedicationCard` follow the same logic explained on the previous point.
	    - `FindPrescriptions`, `FindPrescriptionsForValidation` and `FindPrescriptionsForDispense` will apply the criteria against the consolidated prescription's validity period. 
		    - The prescription validy start date shall match if:
			    - Greater or equal than `ServiceStartFrom` if specified.
			    - Lesser than `ServiceStartTo` if specified.
		    - The prescription validty end date shall match if:
			    - Greater of equal than `ServiceEndFrom` if specified.
			    - Lesser than `ServiceEndTo` if specified.
		    - If a prescription does not specify a validity period start date, it is assumed to be the date of medical authorship (i.e. `authoredOn`).
		    - If a prescription does not specify a validity period end date, it is assumed to not have an end date and hence any specified *...To* parameter should match.
	    - `FindDispenses` will apply the criteria against a single date, which is the date of dispense (i.e. `whenHandedOver`).
- Support for a new custom parameter for `FindMedicationCard` queries, `$PMLCIncludeNonActive` has been added. If specified and `true`, it will include all the (aggregated) treatments (constrained by the specified dates criteria) for the patient, whether they are active or not (i.e. it will include also suspended or cancelled treatments). These treatments will be included as medication statements in the content of the PMLC document but will not be added to the generated PDF. Support for this parameter is already included since version (`2.1.2`) of [Husky](https://github.com/project-husky/husky).
- Support for ITI-45 (PIXV3 queries) requests. The aggregator recognizes 3 possible data sources: CARA's MPI's assigning authority OID, the Swiss Central Compensation Office's and the PMP's own patient identity domain.
- A new PMP-PID is generated by the PMP and assigned to the patient upon registration (i.e. new APPC submission).

