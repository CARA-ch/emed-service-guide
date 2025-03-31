# ITI-68

The [ITI-68 (Retrieve Document)](https://fhir.ch/ig/ch-epr-fhir/iti-68.html) transaction allows a Document Consumer (e.g. a primary system) to retrieve a document from the Document Responder (i.e. the eMedication service) as specified by the [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.html) implementation guide.

This transaction provides an [MHD](https://profiles.ihe.net/ITI/MHD/index.html)-based alternative, as profiled by [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.html) to the [ITI-41](iti41.html) transaction. Note that the same restrictions and business logic as for said XDS transaction apply to this MHD equivalent.

## Security Considerations

Note that beside the security considerations expressed (directly or indirectly) by the CH EPR FHIR profile, the eMedication service implementation supports only, for now, the [SAML Token option](https://profiles.ihe.net/ITI/IUA/#342-iua-actor-options), which means that the authorization token conveyed with the HTTP request must be an XUA token.
