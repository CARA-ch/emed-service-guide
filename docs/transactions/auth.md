# Authentication and authorization

This is how the PMP-APPC format (policy document) is used in the eMedication aggregator.

## Rules

### Patient

A patient has a full read-write access to its record.
This behavior cannot be modified.

### Healthcare professional

Healthcare professionals can be granted or denied a read-write access to all eMedication documents, through their GLN or their HPD groups.

### Assistant

Assistant cannot be directly targeted by an access rule.
Access rules are evaluated against the assistant's responsible person's GLN or HPD groups.
This behavior cannot be modified.

### Technical user

!!! warning "Under discussions"

    The technical users access rules are still under discussion and may evolve.

Technical users can publish documents, and may search and retrieve medication card documents if and only if the following two conditions are true:

1. the TCU belongs to an organization that is whitelisted by the eMedication service.
2. the patient has granted access to an organization that is part of the whitelisted organization.
<!-- TODO: or only top-level? -->
<!-- TODO: is it possible to deny a TCU through its responsible GLN? -->

This behavior cannot be modified.

### Document administrator

Document administrators have a read-write access to all eMedication documents of all patient records.
They can't read or write policy documents.
This behavior cannot be modified.

### Policy administrator

Policy administrators have a read-write access to all policy documents of all patient records.
They can't read or write eMedication documents.
This behavior cannot be modified.

### Emergency access

Emergency access ("break-the-glass") is only possible for healthcare professionals and assistants.
It is globally allowed or forbidden by a specific rule in the policy document.

### Representative

!!! bug "Not implemented"

    This is not supported yet because CARA's platform does not expose its representative directory.

Representatives can be granted a read-write access to all documents, through their representative identifier (which is community-specific).

## ATNA

The service generates ATNA messages as required by IHE and Annex 5.1.
They will be sent to CARA's EPR Audit Record Repository if it is possible.
Clients are expected to also generate logs and send them to the same ARR.

As of yet, it's unsure if die Post's platform can accept these audit messages.
If not, another ARR will have to be deployed by CARA.
