# MTP document

## Header

## Sections

## Item entry

### Authors

They're forbidden in a MTP document and SHALL be provided in PML and PMLC documents.
The first author contains the first document author of the original MTP document that contained this MTP entry.
The second author contains the section author of the original MTP section that contained this MTP entry, but only if the later is different than the former.
Their use is described in [PML](pml.md) and [PMLC](pmlc.md) pages.


### Patient medication instructions

The patient medication instructions are comments from the author to the patient.
It SHALL not contain human readable dosage instructions (see the [dosage page](dosage.md) for that use), it may contain additional information on intake (e.g. "take with food"), on storage (e.g. "store at ambient temperature").

```xml title="Example usage of the patient medication instructions"
<entryRelationship typeCode="SUBJ" inversionInd="true">
    <act classCode="ACT" moodCode="INT">
        <templateId root="1.3.6.1.4.1.19376.1.5.3.1.4.3" />
        <templateId root="2.16.840.1.113883.10.20.1.49" />
        <code code="PINSTRUCT" codeSystem="1.3.6.1.4.1.19376.1.5.3.2" codeSystemName="IHEActCode" />
        <text>
            Swallow the tablets with some water. <!--(1)-->
            <reference value="#ref" />
        </text>
        <statusCode code="completed"/>
    </act>
</entryRelationship>
```

  1.  Change this text as needed.

### Fulfillment instructions

The instructions are comments from the author to the prescriber and/or dispenser.
It SHALL not be provided by a patient or representative.

### ID of parent container

Forbidden in a MTP or PMLC document, mandatory in a PML document. It contains the ID of the document that originally introduced this MTP entry.

### Annotation comment

Use case not clear yet.

### In reserve

<span class="should-support">Should support</span>. Swiss extension, only described in ArtDecor.
This is an entry to specify if the medication is "in reserve" (Reservemedikation; médicament en réserve; to be taken by the patient only if the need arises - e.g. pains) or if it's regular (Grundmedikation; traitement de base/de fond?; to be always taken).
The conditions under which the patient can take the medicine can be described in the _Patient Medication Instructions_ free text element.
By default, the medication is a regular one.
If the medication is in reserve, the structured dosage instructions represent the maximum dosage the patient can take.

```xml title="Example usage of the in reserve extension"
<entryRelationship typeCode="COMP">
    <act classCode="ACT" moodCode="DEF">
        <templateId root="2.16.756.5.30.1.1.10.10" />
        <code code="225761000" codeSystem="2.16.840.1.113883.6.96" codeSystemName="SNOMED Clinical Terms" displayName="As required (qualifier value)" /> <!--(1)-->
        <statusCode code="completed"/>
    </act>
</entryRelationship>
```

  1.  Change this code as needed.

<!-- TODO parties qui ne sont pas remplissables par le patient -->
