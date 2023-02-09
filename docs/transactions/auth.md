# Authentication and authorization

This is how the CH-APPC format is used in the eMedication aggregator.

## Rules

### Patient

A patient has a full read-write access to its record.
This behavior cannot be modified.

### Healthcare professional

Healthcare professionals can be granted or denied a read-write access to all eMedication documents, through their GLN or their organizations.

### Assistant

Assistant cannot be directly targeted by an access rule.
Access rules are evaluated against the assistant's responsible person (an healthcare professional with a GLN), and the assistant's organizations (as provided in the SAML token).
This behavior cannot be modified.

### Technical user

Technical users can publish documents, search and retrieve medication card documents if and only if the following two conditions are true:

1. the TCU belongs to an organization that is whitelisted by the eMedication service.
2. the patient has granted access to an organization that is part of the whitelisted organization.

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
It is globally allowed or forbidden by the APPC document.

### Representative

!!! bug "Not implemented"

    This is not supported yet because CARA's platform does not expose its representative directory.
