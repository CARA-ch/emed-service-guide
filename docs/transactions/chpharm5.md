# CH:PHARM-5

This transaction is the RESTful alternative to the [CH:PHARM-1](chpharm1.html) transaction, as defined by the defined by IHE Pharmacy in [the CMPD profile](https://www.ihe.net/uploadedFiles/Documents/Pharmacy/IHE_Pharmacy_Suppl_CMPD.pdf) but with some Swiss extensions. The same business logic and constraints as for CH:PHARM-1 applies.

As for the [ITI-67](iti67.html) request, the eMedication service backend is grouped with the XDS on FHIR option, and hence the `identifier` query parameter is not supported. All other parameters specified by PHARM-5 are supported.

## Security Considerations

Note that beside the security considerations expressed (directly or indirectly) by parent profiles, the eMedication service implementation supports only, for now, the [SAML Token option](https://profiles.ihe.net/ITI/IUA/#342-iua-actor-options), which means that the authorization token conveyed with the HTTP request must be an XUA token.
