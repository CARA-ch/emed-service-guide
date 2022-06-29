# CDA-CH-EMED

Most of the specifications are described by:

1. [IHE PCC TF-2 §6.3.4.16](https://www.ihe.net/uploadedFiles/Documents/PCC/IHE_PCC_TF_Vol2.pdf)
2. IHE PHARM profiles (MTP, PRE, DIS, PADV, PML, CMPD)
3. [ArtDecor CDA-CH-EMED](https://art-decor.org/art-decor/decor-templates--cdachemed-?section=templates)

!!! warning

    This integration guide will not cover all details of these specifications and integrators SHOULD read them or they won't be able to properly understand and integrate. It isn't a goal of this guide to fully describe them.

- [The common parts](common.md)
- [MTP](mtp.md): introducing a treatment in the plan.
- [PRE](pre.md): prescribing a medication.
- [DIS](dis.md): dispensing a medication.
- [PADV](padv.md): modifying the state of the treatment.
- [PML](pml.md): listing multiple entries in a single document.
- [PMLC](pmlc.md): listing the on-going treatments in an aggregated form.
- [Dosage instructions](dosage.md)
- [Product and packaging](product.md)
- [Workflow](workflow.md)

## Validation

There are multiple validation aspects for CDA-CH-EMED documents.

1. The XML Schema for CDAs.
2. The Schematron for CDA-CH-EMED.
3. The Husky digesters for CARA's CDA-CH-EMED.
4. The PDF standard required by CDA-CH.

Husky provides them all: the validator [CdaChEmedValidator](https://project-husky.github.io/husky/org/husky/emed/ch/cda/validation/CdaChEmedValidator.html) checks the points 1, 2 and 4.
The digester [CceDocumentDigester](https://project-husky.github.io/husky/org/husky/emed/ch/cda/digesters/CceDocumentDigester.html) is available but requires a deeper integration for documents others than MTP and PMLC. 

The validators 1 and 2 are available in [Gazelle](https://ehealthsuisse.ihe-europe.net/EVSClient/cda/validator.seam?standard=CDACH&extension=CDACH).

Some other points are currently only validated by CARA's aggregator and there's no way to test them except by publishing document in it.
A demo platform is available for such tests.
