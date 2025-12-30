# CH:MHD-1

The [CH:MHD-1 (Update Document Metadata)](https://fhir.ch/ig/ch-epr-fhir/ch-mhd-1.html) transaction allows a Document Source actor (e.g. a primary system) to update document metadata on the Document Recipient (i.e. the eMedication service). What this means in practice, for the eMedication service, is that, as for the [ITI-57](iti57.md) transaction this is an MHD-like alternative to, a Document Source is allowed to flag a document as to be deleted by the eMedication service. The same kind of business logic and constraints apply as for the XDS counterpart.

Because of this, the only attribute that it is allowed to be modified from the original document metadata (`DocumentReference`), is the [`deletionStatus`](https://fhir.ch/ig/ch-epr-fhir/StructureDefinition-ch-mhd-documentreference-comprehensive-definitions.html#DocumentReference.extension:deletionStatus) extension of the document reference. Not only this is the only allowed change, but because of this, it is mandatory to be set as [`urn:e-health-suisse:2019:deletionStatus:deletionRequested`](https://fhir.ch/ig/ch-term/3.1.0/CodeSystem-2.16.756.5.30.1.127.3.10.18.html#2.16.756.5.30.1.127.3.10.18-urn.58e-health-suisse.582019.58deletionStatus.58deletionRequested).

## Processing of Deletion Requests

For now, the eMedication service processes deletion requests immediately. This means that a document for which the metadata update is accepted, will be immediately deleted and hence neither the document nor its metadata will be available any more.

## Security Considerations

Note that beside the security considerations expressed (directly or indirectly) by the CH EPR FHIR profile, the eMedication service implementation supports only, for now, the [SAML Token option](https://profiles.ihe.net/ITI/IUA/#342-iua-actor-options), which means that the authorization token conveyed with the HTTP request must be an XUA token.
