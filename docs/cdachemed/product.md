# Product and packaging

!!! error "The support for the CDA-CH-EMED format has been dropped"

    Since January, 2023, the eMedication aggregator only supports the FHIR-based CH-EMED format. This page will removed in the future.

## Code

<span class="must-support">Must support</span>.
The GTIN or ATC code that caracterizes the medication package (GTIN) or the medication substance (ATC) SHALL be provided.

!!! warning "CARA: additional requirement"

    Products without ATC or GTIN codes are currently not supported. Magistral preparations may have an ATC code, they're partially supported.

<!-- TODO only medication or other products that have a GTIN? -->

```xml title="Example usage"
<code code="7680538751228" codeSystem="2.51.1.1" codeSystemName="GTIN" displayName="TRIATEC Tabl 2.5 mg 100 Stk" />
<code code="M04AC01" codeSystem="2.16.840.1.113883.6.73" codeSystemName="ATC" displayName="Colchicine" /> <!-- (1) -->
```

1.  Colchicine was not authorized by Swissmedic but still recommended by the [Schweizerische Gesellschaft für Rheumatologie](https://www.rheuma-net.ch/de/dok/sgr-dokumente/behandlung/therapie/other-therapies/519-colchicin/file) and can be ordered by pharmacists. It may or may not be reimbursed though. Hi Colctab, you're ruining my example :)

## Name

<span class="must-support">Must support</span>.
The element SHALL contain the name of the medication (e.g., "Adol 500mg Caplet").
The medication may be either:

- a brand/product or 
- described as a generic/scientific name <!-- or -->
<!-- - a descriptor of a magistral preparation/compound medicine -->

<!-- If the medicine has no brand name (e.g., magistral preparations, compound medicine, …) `#!xml nullFlavor="NA"` SHALL be used. -->

## Form code (galenic form)

This code represents the galenic form (e.g. tablet, capsule, liquid). It's optional and can be omitted when the information can be found with the GTIN or ATC code.
<!-- If the medicine is uncoded (e.g., magistral preparations, compound medicine, …) `#!xml nullFlavor="NA"` SHALL be used. -->
<!-- Recommended usage -->

## Lot number text

The lot number text is a free text used to indicate the medication lot number.

!!! abstract "Usage in MTP and PRE"

    The main usage is controlled trials, so usually not in the scope of the eMedication service; it's not expected of healthcare professionals to fill it.

!!! abstract "Usage in DIS"

    Dispensers may provide it for documentation.

## Expiration time

The medication expiration time.

!!! abstract "Usage in MTP and PRE"

    No usage.

!!! abstract "Usage in DIS"

    Dispensers SHOULD provide it, as it'll enable patient applications to alert them about incomming expiration of active treatment to allow timely re-dispensiation and avoid treatment interuption or expired medication usage.

## As content

This structure describes the packaging of the medication and MAY be present.
It represents the primary description of the packaging of the medicine (e.g., the medicine is packaged in ampoules of 50ml volume each) and may include additional packaging information of how many of the primary packaged items are within an outer package (e.g., 5 ampoules are packaged in a box).
The primary description of the package SHOULD be consistent with the given pharmaceutical dose form (`#!xml <pharm:formCode>` of the medication, see "Form Code").
Example: a consistent pharmaceutical dose form to the package form "Ampoules" would be e.g., "Solution for injection".

## As content: code

This SHALL be a GTIN; ATC is not authorized because it does not describe a specific packaging.

## As content: name

<span class="must-support">Must support</span>.
In case the package describes a product, and the package has a brand name, it SHOULD be described in this `#!xml <pharm:name>` element (e.g., Xylocaine 1% with Adrenaline Inj, 5 injections package).

## As content: form code

It represents the form of the package. Not used, a dedicated value set is needed (it's not equivalent to the medication form code).

## As content: capacity quantity

The capacity of the packaging.

!!! warning "CARA: additional requirement"

    The eMedication service restricts the unit to the ["RegularUnitCode (Ambu)" value set](https://art-decor.org/art-decor/decor-valuesets--cdachemed-?id=2.16.756.5.30.1.127.77.12.11.3).

## Ingredients

!!! info "Not supported"

    Currently, ingredients are ignored by the eMedication service. They're 'must support' in IPAG.
