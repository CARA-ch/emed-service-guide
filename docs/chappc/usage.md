# Usage of the CH-APPC format

This is how the CH-APPC format is used in the eMedication aggregator.

## Patient

## Representative

!!! bug "Not implemented"

    This is not supported yet because CARA's plateform does not expose its representative directory.

## Healthcare professional

## Assistant

Assistant cannot be directly targeted by an access rule. Access rules are evaluated against the assistant's responsible person (an healthcare professional with a GLN). This behavior cannot be modified.

## Technical user

## Document administrator

Document administrators have a read-write access to all eMedication documents of all patient records. They can't read or write policy documents. This behavior cannot be modified.

## Policy administrator

Policy administrators have a read-write access to all policy documents of all patient records. They can't read or write eMedication documents. This behavior cannot be modified.
