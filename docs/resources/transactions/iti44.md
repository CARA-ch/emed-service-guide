# Patient Identity Feed HL7 V3 [ITI-44]

The eMedication service exposes an [ITI-44](https://profiles.ihe.net/ITI/TF/Volume2/ITI-44.html) endpoint (see [endpoints](../../references/endpoints/index.md)) that allows clients to register a patient in the eMedication's own PIX service.

The transaction follows the IHE profile specifications, with the following particularities:

- Only `Patient Registry Record Added` messages are supported.
- Swiss EPR restrictions to the `Patient Registry Record Added` message, as per the [Swiss National Extensions to the IHE Technical Framework](https://www.bag.admin.ch/dam/bag/de/dokumente/nat-gesundheitsstrategien/strategie-ehealth/gesetzgebung-elektronisches-patientendossier/gesetze/anhang_5_ergaenzung_1_epdv_edi_ausgabe_4.pdf.download.pdf/EPDV-EDI_Anhang_5_E1_DE_Ausgabe_4.pdf), section 1.7.
    - [§ 1.7.1] Must contain the patient's EPR-SPID.
    - [§ 1.7.1] Must contain an additional patient id to the EPR-SPID. In the context of the eMedication service, this can be for instance CARA's MPI-PID.
    - [§ 1.7.1] No `religiousAffiliationCode` allowed.
    - [§ 1.7.1] No `raceCode` allowed.
    - [§ 1.7.1] No `ethnicGroupCode` allowed.
    - [§ 1.7.1] No `personalRelationship` allowed.
- Note that different environments might use different OIDs for the assigning authorities. See the [OIDs page](../../references/oids/index.md).
- Upon receiving the message, the eMedication service will act int he following manner:
    - If the eMedication's PIX service recognizes the patient's EPR-SPID, the patient is already registered in the service and it will return an `OK` code with a detail stating that the registration existed already.
    - If the patient is not currently registered in the eMedication service:
        - If the patient has a PMP-PID registered in the CARA's MPI already, the eMedication service will create a new eMedication registration with this existing PMP-PID and return `OK` with proper detail.
        - If the patient does not have a PMP-PID in CARA's MPI:
            - If the patient is nevertheless registered in CARA's MPI, meaning it has a CARA MPI-PID, the eMedication service will assign a new PMP-PID to the patient, register it in CARA's MPI and create an eMedication registration for the patient with the associated PMP-PID. The response will have an `OK` code and proper detail.
            - If the patient is not registered at all in CARA's MPI, the eMedication service will assign a new PMP-PID to the patient, the eMedication service will create a new registration for this patient in CARA's MPI with the associated PMP-PID and finally create an eMedication registration with the associated PMP-PID. The response will have an `OK` code with proper detail. Note that this is the only case in which the patient information conveyed with the `Patient Registry Record Added` message, other than the patient ids, will be used.

!!! warning

    Please note that the enrollment process will not be completed until an [APPC](../appc/index.md) document has been provided after adding the patient to the eMedication PIX service. Clients shall not start an ITI-44 transaction unless they can provide an APPC document for the patient immediately after successfully finishing this PIX feed transaction.
