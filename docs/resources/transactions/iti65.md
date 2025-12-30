# ITI-65

The [ITI-65 (Provide Document Bundle)](https://fhir.ch/ig/ch-epr-fhir/iti-65.html) transaction allows a Document Source (e.g. a primary system) to send a document with its associated metadata to the Document Recipient (i.e. the eMedication service backend).

This transaction provides an [MHD](https://profiles.ihe.net/ITI/MHD/index.html)-based alternative, as profiled by [CH EPR FHIR](https://fhir.ch/ig/ch-epr-fhir/index.html), to the [ITI-41](iti41.md) transaction, and the same metadata and business logic constraints defined for that transaction on our service guide apply.

## Security Considerations

Note that beside the security considerations expressed (directly or indirectly) by the CH EPR FHIR profile, the eMedication service implementation supports only, for now, the [SAML Token option](https://profiles.ihe.net/ITI/IUA/#342-iua-actor-options), which means that the authorization token conveyed with the HTTP request must be an XUA token.
