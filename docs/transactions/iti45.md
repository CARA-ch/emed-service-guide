# PIXV3 Query [ITI-45]

The eMedication service exposes an [ITI-45](https://profiles.ihe.net/ITI/TF/Volume2/ITI-45.html) endpoint (see [endpoints](../endpoints.md)) that allows clients to query the service for patient ids of registered patients, that is, patients currently enrolled in the eMedication service.

This transaction follows IHE's specifications with the following particularities:

- The eMedication service recognizes 3 possible data sources for patient ids (see [OIDs](../oids.md) for actual values):
    - CARA's assigning authority.
    - The Swiss Central Compensation Office's assignign authority.
    - The eMedication service's own assigning authority.

!!!warning

    Please note that even though the PMP-PIDs are added and stored as well in CARA's MPI, clients are encouraged to query the eMedication PIX service to query for (or confirm) the PMP-PID. This is because CARA's MPI might contain PMP-PIDs for patients that are no longer enrolled in the eMedication service and while the eMedicaiton PIX service will no longer recognize such patient ids, CARA's PIX service will continue to return those.

!!!warning

    A patient's enrollment in the eMedication service requires registration in its PIX service __as well as__ a valid [APPC](../appc/index.md) document.
