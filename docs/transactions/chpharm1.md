# CH:PHARM-1

The PMP doesn't support CMA documents and these should not be expected in any response.

### FindMedicationTreatmentPlans

Implemented, as per IHE specs.

### FindPrescriptions

Implemented, as per IHE specs.

### FindDispenses

Implemented, as per IHE specs.

### FindMedicationAdministrations

🚫 Not implemented, CMA documents are not supported.

### FindPrescriptionsForValidation

❓ Return only prescriptions that are temporary?

### FindPrescriptionsForDispense

Return only prescriptions that are actually "open|valid" (i.e. dispensable).

### FindMedicationList

Only documents that are linked to an MTP are considered in this query.

### FindMedicationCard

- *$XDSDocumentEntryType*: stable and/or on-demand. Stable documents are PML documents that have been generated and saved, on-demand are documents that can be generated. On-demand is required, as there's no stable PML in the PMP. By default, all types are returned. At max one on-demand document is returned.