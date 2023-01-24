# PMLC document

!!! error "The support for the CDA-CH-EMED format has been dropped"

    Since January, 2023, the eMedication aggregator only supports the FHIR-based CH-EMED format. This page will removed in the future.

## Header

## Sections

## Item entry

As described in the [MTP](mtp.md) page, except for the following differences.

It's a consolidated MTP entry, meaning that all changes brought by PADV, PRE or DIS entries are applied to it (e.g. dosage or product changes). The last non-empty value applies.

### Id

The entry ID is a new one, different from the ID of the original MTP entry.

### Authors

1. The first author represents the author of the last section (of any kind) in the treatment.
2. The second author represents the author of the last section (of type _MTP_, _PRE_, or any type of _PADV_ except _COMMENT_ that apply to an _MTP_ or _PRE_) in this treatment. This is the last "medical" participant. If it's the same author as 1., it shall not be provided.

### ID of parent container

Forbidden.

### Reference to the original Medication Treatment Plan Item

Mandatory. It contains the reference to the MTP document and/or MTP entry that originally introduced this medication.

