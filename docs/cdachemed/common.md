# The common parts

!!! error "The support for the CDA-CH-EMED format has been dropped"

    Since January, 2023, the eMedication aggregator only supports the FHIR-based CH-EMED format. This page will removed in the future.

## Effective time

<span class="must-support">Must support</span>.

## Authors

<span class="must-support">Must support</span>.
Authors are/can be present in the document header, in the section and in the item entries.

The author in the __document header__ is the author of the whole document, while the author in the __section__ is the one that authored the item entry. If both are the same, the section author shall not be provided. They may be different if, for example, a secretary creates a document based on the prescription of a doctor. In that case, the document author is the secretary while the entry author is the doctor.

<!-- What about softwares? When are they the author? -->
Authors can be human people or softwares. Document authors of medication lists and medication cards will often be softwares, while authors of _MTP_, _PRE_ and _DIS_ sections shall always be human.

When the __item entry__ is contained in its original containing document (e.g. an _MTP_ item entry in an _MTP_ document), the authors shall not be provided. They're provided when the item entry is contained in another document (_PML_ or _PMLC_), and their meaning is described in the dedicated pages.
