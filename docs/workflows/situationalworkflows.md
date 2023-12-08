# Situational workflows
This page describes possible interactions between third party applications and the eMedication service in different situations (medical consultation, medication dispense at a pharmacy, visit at a hospital etc.). 

For each situation, and corresponding types of eMedication service clients, it details the documents that can be exchanged and the corresponding transactions.

A detailed documentation on the documents is available on the [ch-emed-epr FHIR implementation guide (IG)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/index.html).

The supported documents are:

* MTP (Medication Treatment Plan) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_MTP.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html): introducing a treatment in the plan.
* PRE (Prescription) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_PRE.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html): prescribing a medication.
* DIS (Dispense) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_DIS.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html): dispensing a medication.
* PADV (Pharmaceutical advice) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_PADV.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html): modifying the state of the treatment or adding a comment.
* PML (Medication list) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_PML.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html): listing multiple medication entries in a single document. This document can only be retrieved from the medication service. It is not possible to publish it.
* PMLc (Medication card) [IHE Definition](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_PML.pdf) - [ch-emed-epr IG](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html): listing the on-going treatments in an aggregated form. This document can only be retrieved from the medication service. It is not possible to publish it.

## Workflows
### Exchanging documents
Interacting with the eMedication service essentially comes down to:

* Publishing documents ([MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html), [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html), [DIS (Dispense)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html), [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html)), with the [ITI-41 (publish document)](../transactions/iti41.md) transaction.
* Retrieving the medication list ([PML (Medication list)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html)), or card ([PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html)), with a combination of [CH:PHARM-1 (search pharmacy documents)](../transactions/chpharm1.md) and [ITI-43 (retrieve document)](../transactions/iti43.md) transactions:
    * [CH:PHARM-1 (search pharmacy documents )](../transactions/chpharm1.md) is used to get a reference to the medication list (or card) document, through the ```FindMedicationList``` (or ```FindMedicationCard```) query.
    * [ITI-43 (retrieve document)](../transactions/iti43.md) is used to retrieve the document, based on the reference obtained above. The document is generated on demand by the service.

### Situational examples
#### Prescriber workflow
During a medical consultation, the typical workflow for a prescriber (medical practitioner), and the corresponding transactions would be:

* To check the current medication of a patient during the anamnesis.
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) to retrieve the current medication plan ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).
* To prescribe new medication.
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each new medication introduced into the plan ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) to issue a new prescription.
* To provide pharmaceutical advice about the treatment ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) [COMMENT](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-comment) to add a comment on a MTP entry ([ITI-41 (publish document)](../transactions/iti41.md)).

#### Dispenser workflow
While visiting a pharmacy after a medical consultation, a patient and a pharmacist (dispenser) may have a conversation related to the current medication of the patient, the new treatments prescribed, possible drug substitutions (use of generic drug, change of the dosage form) and possible addition of new medications to the prescription.

The typical workflow for a pharmacist (dispenser), and the corresponding transactions would be:

* To check the current medication of a patient.
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) (or [PML (Medication list)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pml.html) if history is needed) to retrieve the current medication plan ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).
* To update the medication plan in case it is not up-to-date (for instance a new prescription has been issued by a prescriber not connected to the eMedication service).
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each treatment not in the plan to introduce it into it ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html).
        * [CANCEL](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-cancel) for each MTP entry that is no longer active ([ITI-41 (publish document)](../transactions/iti41.md)).
* To check and confirm the prescription made by the prescriber.
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html).
        * [OK](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-ok) for each [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) entry of the prescription that can be dispensed ([ITI-41 (publish document)](../transactions/iti41.md)).
        * [REFUSE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-refuse) for each [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) entry of the prescription that cannot be dispensed ([ITI-41 (publish document)](../transactions/iti41.md)).
* To prescribe new medication.
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each new treatment to introduce them into the plan ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) for each medication prescribed by the dispenser ([ITI-41 (publish document)](../transactions/iti41.md)).    
* To dispense the prescribed medication or appropriate generics, possibly changing the dosage form.
    * [DIS (Dispense)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_dis.html) for each medication actually dispensed.
* To generate a medication card for the patient ([ITI-41 (publish document)](../transactions/iti41.md)).
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) to create a document describing the new actual medication of the patient ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).

#### Medication manager workflow
The medication manager helps patients having difficulties with their own medications. Their role is mainly to monitor and assist patients with their medications. The workflow and transactions for medication managers is essentially the same as for patients (see below).

#### Patient workflow
In addition to checking it, a patient can update its own medication plan, to add comments on the medication, introduce self-medication or indicate that he or she has stopped taking a medication. There is no specific workflow, but the transactions could be:

* To generate a medication card.
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) to create a document describing the new actual medication ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).
* To add self-medication treatments.
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each new self-medication treatment ([ITI-41 (publish document)](../transactions/iti41.md)).
* To update existing self-medication treatments or add a comment on any treatment.
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html).
        * [SUSPEND](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-suspend) for each MTP entry the patient is no longer taking ([ITI-41 (publish document)](../transactions/iti41.md)).
        * [COMMENT](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-comment) possibly on each MTP entry, to add details, or explain the reasons of a [SUSPEND](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-suspend) ([ITI-41 (publish document)](../transactions/iti41.md)).

#### Hospital workflow
During a visit at a hospital, the medication is handled by the hospital's information system. Interactions with the e-medication service typically happen at admission and release.
##### Admission
The typical workflow at admission, and the corresponding transactions would be:

* To check the current medication of a patient.
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) to retrieve the current medication plan ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).
* To update the medication plan in case it is not up-to-date (for instance a new prescription has been issued by a prescriber not connected to the eMedication service) ([ITI-41 (publish document)](../transactions/iti41.md)).
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each  treatment not in the plan to introduce it into it ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html).
        * [CANCEL](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-cancel) for each MTP entry that is no longer active ([ITI-41 (publish document)](../transactions/iti41.md)).
        * [SUSPEND](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-suspend) for each MTP entry that should be suspended ([ITI-41 (publish document)](../transactions/iti41.md)).
        
##### Release
At release, the medication plan might be entirely revised: former medication stopped or altered, new medication introduced. The typical workflow and corresponding transactions would therefore be a combination of:

* Prescribing new medication.
    * Export [MTP (Medication Treatment Plan)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) for each new medication introduced into the plan ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PRE (Prescription)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) to issue a new prescription ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) [COMMENT](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-comment) possibly on each new MTP entry ([ITI-41 (publish document)](../transactions/iti41.md)).
* Updating former medication
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) [CANCEL](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-cancel) for each former MTP entry whose treatment should be permanently canceled. ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) [CHANGE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-change) for each former MTP entry that should be updated ([ITI-41 (publish document)](../transactions/iti41.md)).
    * [PADV (Pharmaceutical advice)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html) [OK](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_padv.html#padv-ok) for each former MTP entry whose treatment was suspended and should be now resumed ([ITI-41 (publish document)](../transactions/iti41.md)).
* Generating a medication card for the patient.
    * Import [PMLc (Medication card)](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pmlc.html) to create a document describing the new actual medication of the patient ([CH:PHARM-1 (```FindMedicationCard```)](../transactions/chpharm1.md) then [ITI-43 (retrieve document)](../transactions/iti43.md)).