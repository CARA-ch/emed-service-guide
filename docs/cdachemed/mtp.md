# MTP document

## Header

## Sections

## Item entry

It's an `#!xml <hl7:substanceAdministration>`.

### Dosage instructions

See the [dedicated page](dosage.md).

### Product and package

See the [dedicated page](product.md).

### Authors

They're forbidden in a MTP document and SHALL be provided in PML and PMLC documents.
The first author contains the first document author of the original MTP document that contained this MTP entry.
The second author contains the section author of the original MTP section that contained this MTP entry, but only if the later is different than the former.
Their use is described in [PML](pml.md) and [PMLC](pmlc.md) pages.

### Reference to the Treatment Reason Entry Content Module

Not used, see below for the treatment reason as free text.

### Treatment reason

Swiss extension.
The treatment reason as a free text.
Authors SHOULD keep it as simple and short as possible (e.g. "blood clog", "hypertension").

### Reference to the original Medication Treatment Plan Item

Forbidden in a MTP or PML document, mandatory in a PMLC document.

### Patient medication instructions

The patient medication instructions are comments from the author to the patient. It SHALL not contain human readable dosage instructions (see the [dosage page](dosage.md) for that use), it may contain general comments (e.g. "take with food").

### Fulfilment instructions

The instructions are comments from the author to the prescriber and/or dispenser. It SHALL not be provided by a patient or representative.

### Amount of units of the consumable to dispense

Not supported currently.

### Substitution permission

<span class="should-support">Should support</span>.
Whether the dispenser can substitute the prescribed medicine/package by another that is deemed equivalent, for medical or logistical reasons.
By default, substitution is permitted.
It SHALL not appear more than once.

!!! warning "CARA: additional requirement"

    The eMedication services restricts the substitution permissions to the following two values:
    
    | Permission | Description |
    | ---------- | ----------- |
    | `E`        | Equivalent, subsitution is authorized |
    | `NONE`     | None, substitution is disallowed      |

### ID of parent container

Forbidden in a MTP or PMLC document, mandatory in a PML document. It contains the ID of the document that originally introduced this MTP entry.

### Precondition Criterion

Not used.

### Annotation comment

### In reserve

<span class="should-support">Should support</span>. Swiss extension, only described in ArtDecor.
Whether the patient has to unconditionaly take the medication (regular) or if the decision is up to the patient (e.g. only in case of pain; in reserve).
The conditions under which the patient can take the medicine can be described in the _Patient Medication Instructions_ free text element.
By default, the medication is a regular one.

<!-- TODO parties qui ne sont pas remplissables par le patient -->
