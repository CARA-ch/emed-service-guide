# ITI-83

The [ITI-83 (Mobile Patient Identifier Cross-reference Query)](https://profiles.ihe.net/ITI/PIXm/ITI-83.html) transaction allows a _Patient Identifier Cross-reference Consumer_ (e.g. a primary system app) to query the _Patient Identifier Cross-reference Manager_ (i.e. the eMedication service) for patient identifiers.

This transaction, based directly on IHE's PIXm profile and not on the CH EPR FHIR equivalent, provides a RESTful alternative to the [ITI-45](iti45.md) transaction. The same business logic and constraints apply.

## Security Considerations

This transaction requires no authorization token, neither JWT nor XUA.