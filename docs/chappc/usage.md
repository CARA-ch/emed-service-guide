# Usage of the CH-APPC format

This is how the CH-APPC format is used in the eMedication aggregator.

## Patient

A patient has a full read-write access to its record.
This behavior cannot be modified.

## Representative

!!! bug "Not implemented"

    This is not supported yet because CARA's plateform does not expose its representative directory.

## Healthcare professional

Healthcare professionals can be granted or denied a read-write access to all eMedication documents, through their GLN or their organizations.

## Assistant

Assistant cannot be directly targeted by an access rule.
Access rules are evaluated against the assistant's responsible person (an healthcare professional with a GLN), and the assistant's organizations (as provided in the SAML token).
This behavior cannot be modified.

## Technical user

A discussion is ongoing to give TCUs write access to all eMedication documents and read access to the medication card only (through search and retrieve transactions).

The TCU would be granted access if:
1. the TCU organization has been whitelisted by the eMedication service;
2. the patient has given read-write access to an organization;
3. the TCU is contained in that organization, or in that organization's parent(s).

## Document administrator

Document administrators have a read-write access to all eMedication documents of all patient records.
They can't read or write policy documents.
This behavior cannot be modified.

## Policy administrator

Policy administrators have a read-write access to all policy documents of all patient records.
They can't read or write eMedication documents.
This behavior cannot be modified.

## Emergency access

Emergency access ("break-the-glass") is only possible for healthcare professionals and assistants.
It is globally allowed or forbidden by the APPC document.
