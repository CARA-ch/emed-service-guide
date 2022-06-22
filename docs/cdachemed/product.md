# Product and packaging

## Code

<span class="must-support">Must support</span>.
The GTIN or ATC code that caracterizes the medication package (GTIN) or the medication substance (ATC).

```xml title="Example usage"
<code code="7680538751228" codeSystem="2.51.1.1" codeSystemName="GTIN" displayName="TRIATEC Tabl 2.5 mg 100 Stk" />
<code code="M04AC01" codeSystem="2.16.840.1.113883.6.73" codeSystemName="ATC" displayName="Colchicine" /> <!-- (1) -->
```

1.  Colchicine was not authorized by Swissmedic but still recommended by the [Schweizerische Gesellschaft für Rheumatologie](https://www.rheuma-net.ch/de/dok/sgr-dokumente/behandlung/therapie/other-therapies/519-colchicin/file) and can be ordered by pharmacists. It may or may not be reimbursed though. Hi Colctab, you're ruining my example :)

## Name

## Form code

This code represents the pharmaceutical dose form (e.g. tablet, capsule, liquid).

??? info "Recommended usage"

    table

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

## As content: code

This SHALL be a GTIN; ATC is not authorized because it does not describe a specific packaging.

## As content: name



## As content: form code

It represents the form of the package. Not used, a dedicated value set is needed (it's not equivalent to the medication form code).

## As content: capacity quantity

The capacity of the packaging.

!!! warning "CARA: additional requirement"

    The eMedication service restricts the unit to the ["RegularUnitCode (Ambu)" value set](https://art-decor.org/art-decor/decor-valuesets--cdachemed-?id=2.16.756.5.30.1.127.77.12.11.3).

## Ingredients

!!! info "Not supported"

    Currently, ingredients are ignored by the eMedication service.
