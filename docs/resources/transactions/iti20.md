# Record Audit Event [ITI-20]

Audit events, as specified by IHE's [Audit Trail and Node Authentication (ATNA)](https://profiles.ihe.net/ITI/TF/Volume1/ch-9.html) profile, are required by the Swiss EPR regulations. To align with these, the eMedication service complies itself with these regulations and has its own an Audit Record Repository for all ATNA logs related to the service. Clients of the eMedication service __MUST__ record the necessary audit events during each transaction with the eMedication service by means of an [ITI-20 transaction](https://profiles.ihe.net/ITI/TF/Volume2/ITI-20.html) to the endpoint exposed by the eMedication service's Audit Record Repository (see the [endpoints page](../../references/endpoints/index.md)).

## Recording audit events

The eMedication service supports only syslog messages. [RESTful ATNA](https://www.ihe.net/uploadedFiles/Documents/ITI/IHE_ITI_Suppl_RESTful-ATNA.pdf) support is foreseen but not available for the time being.

### Syslog Interaction

The eMedication service exposes a TCP port (see the [endpoints page](../../references/endpoints/index.md)). This port accepts only two-way TLS connections and will process only syslog messages that comply with the [ITI-20 profile](https://profiles.ihe.net/ITI/TF/Volume2/ITI-20.html). All valid syslog messages, as per IHE's profile, are recorded in the eMedication Audit Record Repository.

Additionally, the Audit Record Repository will try to translate, if possible and as a best effort, all received audit messages to FHIR [AuditEvent](https://hl7.org/fhir/r4/auditevent.html) objects and more specifically, to [CH ATC](https://fhir.ch/ig/ch-atc/index.html) compliant `AuditEvent` objects. Note that it is the responsability of the systems recording audit events to generate complete and compliant messages as per the regulations and that the Audit Record Repository offers some basic transformations to help with CH ATC compliance as a nice-to-have extra on a best-effort basis and not as an obligation.

### RESTful ATNA Feed

The audit record repository supports the FHIR feed of `AuditEvent` resources. Although a validation (and when possible, certain transformations to improve compliance) against CH EPR FHIR or CH:ATC profiles is done, no audit events are rejected in case of no-conformities. The feed is simply done by performing a [create](https://www.hl7.org/fhir/http.html#create) interaction, as defined by the FHIR standard, posting an `AuditEvent` resource to the relevant [endpoint](../../references/endpoints/index.md).
