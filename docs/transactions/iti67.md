# ITI-67

The [ITI-67 (Find Document References)](https://fhir.ch/ig/ch-epr-fhir/iti-67.html) transaction allows Document Consumer actors (e.g. primary systems) to query a Document Responder (i.e. the eMedication service backend) for a list of `DocumentReference` resources that satisfy a set of parameters.

This transaction provides an [MHD](https://profiles.ihe.net/ITI/MHD/index.html)-based alternative, as profiled by [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.html) to the [ITI-18 (Registry Stored Query)](iti18.html) transaction. Note that metadata and business logic constraints on ITI-18 for the eMedication service apply as well for this transaction.

The result of the query is a [Bundle](https://fhir.ch/ig/ch-epr-fhir/StructureDefinition-ch-mhd-documentreference-comprehensive-bundle.html) (so not directly a collection) containing the [DocumentReference](https://fhir.ch/ig/ch-epr-fhir/StructureDefinition-ch-mhd-documentreference-comprehensive.html) resources that match the query parameters (if any).

## Further Constraints

The eMedication service does not for now support the `identifier` parameter. Note that this parameter is in fact not required by the [`XDS on FHIR`](https://profiles.ihe.net/ITI/MHD/ITI-67.html#23674131-xds-on-fhir-option) option to which the eMedication service is grouped with and whose list of mapped parameters are the ones supported by the service.

## Security Considerations

Note that beside the security considerations expressed (directly or indirectly) by the CH EPR FHIR profile, the eMedication service implementation supports only, for now, the [SAML Token option](https://profiles.ihe.net/ITI/IUA/#342-iua-actor-options), which means that the authorization token conveyed with the HTTP request must be an XUA token.
