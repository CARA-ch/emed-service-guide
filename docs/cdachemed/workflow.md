# Workflow

!!! error "The support for the CDA-CH-EMED format has been dropped"

    Since January, 2023, the eMedication aggregator only supports the FHIR-based CH-EMED format. This page will removed in the future.

An MTP item is to insert a medication into the patient's treatment plan and instruct the patient to take it, even if it wasn't prescribed or dispensed: the patient may already have it.

A PRE item is purely a logistic document. It instructs the pharmacist to dispense that medication and allow reimbursment by insurances. Paper prescriptions combine these two usages.

A DIS item is to indicate the medication has been dispensed. It's useful to pharmacists to determine if a prescription may still be dispensed.

A PADV item is used to modify the content or the status, or to comment the previous items.

## Submitting documents

All references in the submitted CDA document SHALL be known to the aggregator, so other users can access it too.

### Restrictions by role

| Role                    | Read access | Write access   | Subject to access rules |
|-------------------------|-------------|----------------|-------------------------|
| Patient                 | ✅           | ✅ / restricted | ❌                       |
| Representative          | ✅           | ✅ / restricted | ✅                       |
| Healthcare professional | ✅           | ✅              | ✅                       |
| Assistant               | ✅           | ✅              | ✅ (responsible)         |
| Technical user          | ❌           | ✅              | ❌                       |
| Document administrator  | ✅           | ✅              | ❌                       |
| Policy administrator    | ❌           | ❌              | ❌                       |

Restrictions for patients/representatives include:

- they cannot provide a PRE or DIS document;
- they cannot fill the "fulfilment instructions" in MTP entries;
- they cannot fill the "function code" in the document author;
- restrictions on PADV usage?
- restrictions on replace/delete documents?

### DocumentEntry

See specifications for [encoding of those fields](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.2.html#4.2.3) and [cardinalities for the sending actor (column "XDS DS")](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.3.html#4.3.1).

| DocumentEntry              | Description |
|----------------------------|-------------|
| author                     | Mapping from the CDA document author |
| availabilityStatus         | Only `urn:oasis:names:tc:ebxml-regrep:StatusType:Approved` accepted |
| classCode                  |  |
| comments                   |  |
| confidentialityCode        | Only `normal` accepted |
| creationTime               | Mapping from the CDA document effective time |
| deletionStatus             | Swiss extension. Only `urn:e-health-suisse:2019:deletionStatus:deletionNotRequested` accepted |
| documentAvailability       | Only `online` accepted |
| entryUUID                  | Same as _logicalID_ |
| eventCodeList              |  |
| formatCode                 |  |
| hash                       |  |
| healthcareFacilityTypeCode |  |
| homeCommunityId            | Fixed by the community you're submitting to |
| languageCode               | Mapping from the CDA language code |
| legalAuthenticator         |  |
| logicalID                  | Same as _entryUUID_ |
| mimeType                   | `text/xml` |
| objectType                 | Only `stable` accepted |
| originalProviderRole       | Swiss extension |
| patientId                  |  |
| practiceSettingCode        |  |
| referenceIdList            | Ignored by the aggregator |
| repositoryUniqueId         | Fixed by the community you're submitting to |
| serviceStartTime           |  |
| serviceStopTime            |  |
| size                       |  |
| sourcePatientId            |  |
| sourcePatientInfo          | Ignored by the aggregator |
| title                      | Mapping from the CDA document title. Mandatory. |
| typeCode                   |  |
| uniqueId                   |  |
| URI                        | Makes no sense |
| version                    | Only `1` accepted |
