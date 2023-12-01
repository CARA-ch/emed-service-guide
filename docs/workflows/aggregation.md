# Aggregation
## Concepts
|Term|Definition|References|
|----|----------|----------|
|Medication plan (current medication)|The medication plan is the documentation of all the medication information for a patient at a certain point of time.<br/>The medication plan can be presented as a [medication card](https://github.com/ehealthsuisse/EPD-by-example/blob/main/files/MedicationCardDocument.md) or as a [eMediplan](https://emediplan.ch/) (eMediplan is not supported by the service, it is another way of displaying the medication plan).|See [IHE Pharmacy Community Medication List (PML)](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_PML.pdf) and IPAG's definition ([FR](https://www.fmh.ch/files/pdf24/rapport-gtip-ipag-sur-la-cybermedication.pdf), [DE](https://www.fmh.ch/files/pdf24/ipag-bericht-emedikation.pdf)), § 2.1 and § 2.5.|
|Medication treatment|A medication treatment corresponds to one single medication the patient was planned to take in the past or is planned to take presently or in the future, including its name, dosage, frequency of intake, etc.<br/>Medication treatments are created every time a [MTP document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) is received.<br/> This service considers two kinds of medication treatments: "simple" and "prescribed".<br/> Simple medication treatments are created with just one [MTP document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) (by the patient or a healthcare professional).<br/> Prescribed medication treatments are created by healthcare professionals only, with a [MTP document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) followed by a [PRE document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html).|See [IHE Pharmacy MTP supplement](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_MTP.pdf), as well as [HL7's Medication Treatment Plan FHIR document](https://build.fhir.org/ig/hl7ch/ch-emed//medication-treatment-plan-document.html).|
|Medication treatment instance|A medication treatment instance is a concept of this eMedication service.<br/>  A medication treatment can change over time: the dosage, medication, active substance, comments, etc. can be updated.<br/> The medication treatment instance represents the state of a medication at a current point of time. <br/>Medication treatment instance creation depends on the type of medication treatment. For "simple" (ie. not prescribed) treatments, a single medication treatment is created when the [MTP document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) is received. <br/> For "prescribed" medication treatments (ie. medication treatments for which there is a [MTP document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_mtp.html) and at least one  [PRE document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html)), a new instance is created every time a [PRE document](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html) is received.<br/>Each medication treatment instance appears in a single line within the medication card. Instances associated with the same treatment are visually grouped.|-|
|Medication statement|A medication statement represents a given medication (form, dosage, GTIN/ATC, ingredients etc.)|See the [definition in the implementation guide](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/StructureDefinition-ch-emed-epr-medicationstatement-treatmentplan.html).|
|Aggregation|The aggregation is the process of determining the current state of each medication treatment within a patient's medication plan, based on the existing medication treatment instances.<br/>This is done depending on the document triggering the aggregation, either by updating existing medication treatments /  medication treatment instances, or by creating new medication treatment instances|-|

## Triggers
Aggregation occurs when:
* A new [emed document](../emed/index.md) is provided via the [ITI-41 transaction](../transactions/iti41.md).
* An [emed document](../emed/index.md) is removed via the [ITI-57 transaction](../transactions/iti57.md).
* An [emed document](../emed/index.md) is replaced via the [ITI-41 transaction](../transactions/iti41.md) and the [replace](https://profiles.ihe.net/ITI/TF/Volume3/ch-4.1.html#4.1.2.2) association.

## Scope
When triggered, aggregation occurs for the medication treatment referenced by the triggering document.

When the triggering document is a [PRE](https://build.fhir.org/ig/CARA-ch/ch-emed-epr/document_pre.html), it might reference several medication treatments (multi-prescription). In this case the service applies the aggregation process to each referenced medication treatment.

## Aggregation process
### Overview
The table below shows an overview of the elements updated or created during the aggregation process.
![aggregation](../assets/aggregation.svg)

### Medication card
![Medication card](../assets/medicationcard.jpg)