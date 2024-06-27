## Currently Deployed

  - dev:
    - PMP (aggregator) v0.4.8 (deployed 2024-06-27, DB recreated with v0.4.0 deployed 2024-04-23), works with [CH EMED EPR 1.0.0](https://fhir.ch/ig/ch-emed-epr/index.html).
    - ALPAGE v0.0.3 (deployed 2024-04-23 due to VM migration, same version as prev. VM, DB recreated)
  - int:
    - PMP (aggregator) v0.3.0 (deployed ~2024-04-15, DB recreated), works with [CH EMED EPR 1.0.0](https://fhir.ch/ig/ch-emed-epr/index.html).
    - ALPAGE v0.0.3 (deployed ~2024-04-15 due to VM migration, same version as prev. VM, DB recreated)


## Next Release Dates

- Next CH EMED EPR version release: *TBD*
- Next aggregator release: *TBD*
- Next aggregator deployment: *TBD*

## Relevant Changes
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


!!! warning 

    Note that possible changes on Husky or other libraries/dependencies are not listed here and it is up to integrators to keep track of them.
