# MTP document

## Header

## Sections

## Item entry

It's an `#!xml <hl7:substanceAdministration>`.

### Dosage instructions

See the [dedicated page](dosage.md).

## Product and package

See the [dedicated page](product.md).

## Authors

They're forbidden in a MTP document and optional (but recommended) in PML and PMLC documents. The first author contains the first document author of the original MTP document that contained this MTP entry. The second author contains the section author of the original MTP section that contained this MTP entry, but only if the later is different than the former.
<!-- TODO: what does it represent in a PMLC? -->

## Substitution permission

Whether the dispenser can substitute the prescribed medicine/package by another that is deemed equivalent, for medical or logistical reasons.
By default, substitution is permitted.

!!! warning "CARA: additional requirement"

    The eMedication services restricts the substitution permissions to the following two values:
    
    | Permission | Description |
    | ---------- | ----------- |
    | E          | Equivalent, subsitution is authorized |
    | NONE       | None, substitution is disallowed      |

## In reserve

Whether the patient has to unconditionaly take the medication (regular) or if the decision is up to the patient (e.g. only in case of pain; in reserve).
The conditions under which the patient can take the medicine can be described in the _Patient Medication Instructions_ free text element.
By default, the medication is a regular one.

## ID of parent container

Forbidden in a MTP document, mandatory in a PML document. It contains the ID of the document that originally introduced this MTP entry.
<!-- TODO: what about PMLC? -->

### Reference to the original Medication Treatment Plan Item

Forbidden in a MTP or PML document, mandatory in a PMLC document. It contains the reference to the MTP document and/or MTP entry that originally introduced this medication.

### Reference to the Treatment Reason Entry Content Module

Not used.

### ID of parent container

Not used.

### Precondition Criterion

Not used.


