# Dosage instructions

[ArtDecor template](https://art-decor.org/art-decor/decor-templates--cdachemed-?id=2.16.756.5.30.1.1.10.4.35).

## Dose regime

The dose regime is described by a templateId.

!!! bug inline end "Not fully supported"

    Only the normal and split regimes are supported as of now.

| Regime | templateId | Description |
| ----------------- | ------------------------------- | ----------- |
| Normal            | 1.3.6.1.4.1.19376.1.5.3.1.4.7.1 |  |
| Tapered doses     | 1.3.6.1.4.1.19376.1.5.3.1.4.8   |  |
| Split doses       | 1.3.6.1.4.1.19376.1.5.3.1.4.9   |  |
| Conditional doses | 1.3.6.1.4.1.19376.1.5.3.1.4.10  |  |
| Combination medication components | 1.3.6.1.4.1.19376.1.5.3.1.4.11 |  |

In normal dose regime, the dosage instructions can be given in structured form (with `#!xml <hl7:effectiveTime>`, `#!xml <hl7:routeCode>`, `#!xml <hl7:approachSiteCode>`, `#!xml <hl7:doseQuantity>` and `#!xml <hl7:rateQuantity>`) or in narrative form (with the appropriate `#!xml <hl7:entryRelationship>`), but not both at the same time.

## Start and stop



## Repeat number

The repeat number is mandatory and can be used to limit the number of allowed refills of the prescription item. It does not count the initial dispense, only the following ones (refills).

```xml title="Usage of the repeat number"
<hl7:repeatNumber value="0" /><!-- No refill is allowed, only the initial dispense -->
<hl7:repeatNumber value="2" /><!-- Two refills are allowed (total: three dispenses) -->
<hl7:repeatNumber nullFlavor="NI" /><!-- The refills are unlimited -->
```

The refill quantity is defined as:

## Route of administration

## Approach site code

!!! bug "Not implemented"

    This is not yet described in CDA-CH-EMED. A value set of common sites is to be created and adopted by eHealthSuisse.

## Dose quantity

## Rate quantity

!!! bug "Not implemented"

    This is not yet described in CDA-CH-EMED. As it mainly concerns perfusions and are uncommon in the XXX, it's not a priority to support it.

## Narrative dosage instructions

The narrative dosage instructions can only appear in the normal dose regime.

```xml title="Usage of the narrative dosage instructions"

```

## Examples
