# Packaging

## Code

The GTIN or ATC code that caracterizes the medication package (GTIN) or the medication substance (ATC).

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

    Dispensers should provide it, as it'll enable patient applications to alert them about incomming expiration of active treatment to allow timely re-dispensiation and avoid treatment interuption or expired medication usage.

## As content: code

This shall be a GTIN.

## As content: name

## As content: form code

## As content: capacity quantity

!!! warning "CARA: additional requirement"

    The eMedication service restricts the unit to the ["RegularUnitCode (Ambu)" value set](https://art-decor.org/art-decor/decor-valuesets--cdachemed-?id=2.16.756.5.30.1.127.77.12.11.3).

## Ingredients

!!! info "Not supported"

    Currently, ingredients are ignored by the eMedication service.
