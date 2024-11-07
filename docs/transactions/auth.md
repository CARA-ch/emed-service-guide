# Authentication and Authorization

## Authentication

The eMedication service follows the same principle as the EPR for authentication. This means that for all patient-related transactions (XDS, MHD and ATC transactions), a [Cross Enterprise User Assertion (XUA)](https://profiles.ihe.net/ITI/TF/Volume1/ch-13.html) must be [provided](https://profiles.ihe.net/ITI/TF/Volume2/ITI-40.html) with the request. Obtaining an XUA assertion from the EPR platform will both authenticate and authorize the claim from the EPR platform perspective. This allows the eMedication service to recognize that the client system's user is recognized by the EPR platform as being who he/she is and in the specified quality (e.g. a healthcare professional). In the rest of this page, authorization will be discussed as regarding the access rights of this _EPR-authorized_ user to the patient's eMedication record. See the EPR by example pages for mor information on EPR [authentication](https://ehealthsuisse.github.io/EPR-by-example/AuthenticateUser/) and [authorization](https://ehealthsuisse.github.io/EPR-by-example/GetXAssertion/) tokens.

## eMedication Record Authorization

Access to the eMedication records is done in an all-or-nothing approach: a person is either allowed to access all the documents in a patient's eMedication record, and contribute to it - that is, to provide new documents -, or said person has no access at all.

Patients specify the access rules to their eMedication record. These are stored by the eMedication service in an [Advanced Patient Privacy Consents (APPC)](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_APPC.pdf) document as specified by the eMedication service [here](../appc/index.md). Every patient in the eMedication service has a valid registered APPC document specifying the access rules. These access rules can be modified by a patient at any time by replacing the APPC document in the eMedication service with a new version by means of an [ITI-41](iti41.md) transaction.

This APPC document is registered in the eMedication service as any other eMedication document, and hence can be also queried for via [ITI-18](iti18.md) and retrieved via [ITI-43](iti43.md) transactions. The APPC document is visible to patients only and can only be provided by patients themselves.

### Rules
The following sub-sections detail how these access rules are used by the eMedication service.

#### Patient

Patients have always full access to their own eMedication record and this access cannot be affected by the rules in the APPC document.

#### Representatives

!!! bug "Not implemented"

    This is not supported yet because CARA's platform does not expose its representative directory.

Representatives can be granted a read-write access to all documents, through their representative identifier (which is community-specific).

#### Healthcare Professionals

Healthcare professionals (HCPs) can be granted or denied a read-write access to all eMedication documents, through their GLN or their HPD groups. For more information on how to specify these rules, see the eMedication service [APPC document page](../appc/index.md).

#### HCP Assistants

Healthcare professional assistants cannot be directly targeted by APPC access rules. Access rights of HCP assistants is determined by evaluating the assistant's responsible person's GLN and HPD groups.

#### Technical Users

!!! warning "Under discussions"

    The technical users access rules are still under discussion and may evolve.

Technical users can publish documents, and may search and retrieve medication card documents if and only if the following two conditions are true:

1. the TCU belongs to an organization that is whitelisted by the eMedication service.
2. the patient has granted access to an organization that is part of the whitelisted organization.
<!-- TODO: or only top-level? -->
<!-- TODO: is it possible to deny a TCU through its responsible GLN? -->

This behavior cannot be modified.

#### Document Administrators

Document administrators have a read-write access to all eMedication documents of all patient records.
They can't read or write policy documents.
This behavior cannot be modified by APPC rules.

#### Policy Administrators

Policy administrators have a read-write access to all policy documents of all patient records.
They can't read or write eMedication documents.
This behavior cannot be modified by APPC rules.

#### Emergency Access

Emergency access (_break-the-glass_) is only possible for healthcare professionals and assistants. It can be either globally allowed or forbidden by a specific rule in the APPC document. In absence of a explicit rule in the APPC document, the eMedication service assumes emergency access to the patient's eMedication record is allowed.

Emergency access, if enabled, allows an HCP or assistant with an emergency-access-authorized XUA token by the EPR platform access to the patient's eMedication record even if the HCP would not be normally authorized by application of the policies contained in the patient's APPC document.

## ATNA

The eMedication service generates audit event logs for all its internal activity as well as all the transactions with the client systems, as specified by the [Audit Trail and Node Authentication (ATNA)](https://profiles.ihe.net/ITI/TF/Volume1/ch-9.html) profile and as per the Swiss EPR regulations, and stored in its own Audit Record Repository. Additionally, all audit events that are related to a patient - and, therefore, all the transaction-related events - generated by the eMedication service are compliant with the [CH ATC](https://fhir.ch/ig/ch-atc/index.html) profile, as required by the EPR regulations. These patient audit events can be retrieved by client systems by means of a [CH ATC transaction](chatc.md), supported by the eMedication service.

__All client systems are also obliged__ to generate audit events for all the transactions with the eMedication service and send them to the eMedication service Audit Record Repository by means of a [Record Audit Event (ITI-20)](iti20.md) transaction. All patient-related events should be CH ATC compliant.

## Enrollment and Opt-out

### Enrollment Requirements
In order to be enrolled, a patient must fulfill the following requirements:

- The patient must have an active EPR and the system enrolling the patient must know the associated EPR-SPID.
- The system must be in a position to immediately [provide](iti41.md) a valid [APPC document](../appc/index.md) for the enrolling patient.

### Enrollment Process
In order to register a patient in the eMedication service, a client system must perform the following steps:

1. Perform a [PIX feed](iti44.md) to register the patient id in the eMedication service.
2. [Provide](iti41.md) the [APPC document](../appc/index.md) for the patient.

!!! warning

    __Systems must absolutely ensure they can provide the APPC document immediately after completing the ITI-44 transaction__. Failure to do so will create a dangling registration in which the eMedication service will have a registered PMP-PID for the patient but it will not recognize its registration (ITI-45 will not recognize the patient ids) until an APPC document is provided. No other transactions than an ITI-41 to provide the APPC document will be allowed in the meantime.

!!! warning

    These rules are currently under revision by CARA, particularly regarding the patient's formal consent to use the eMedication service, and might change before, during or after completion of the pilot program of the eMedication service.

## Opt-out

Patients can opt out of the eMedication service at any time. This is simply done by deleting their current APPC document via an [ITI-57](iti57.md) transaction. Completion of this transaction will permanently delete the enterity of the patient's eMedication record. 

!!! warning

    The removal of the patient's eMedication record is irreversible. After deletion of the record, the patient will be allowed to open an eMedication record again at any time, but the contents of the previous eMedication record cannot and will not be restored.
