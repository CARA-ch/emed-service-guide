# CARA's eMedication service Integration Guide

!!! info

    This is a work in progress. You are welcome to open issues in the 
    [GitHub repository](https://github.com/CARA-ch/emed-service-guide) for any question, mistake or missing information.

Welcome to the Integration Guide for the eMedication service of CARA's community. This document serves as a resource to help facilitate the retrieval of key resources for integrators.

## Aggregator

The aggregator is a key process in the eMedication service. Its role is to aggregate different information from different sources and to provide a global view of medication and its history to enhance medical anamnesis. It is quite useful for generating treatment cards. 

The following graph shows an architecture with the aggregator based on the [Swiss EPR Architecture](https://www.e-health-suisse.ch/payload/api/documents/file/EPD-Architektur_EN.pdf) (which contains the definitions of all the following services).

<figure>
	<a href="./assets/aggregator_architecture.svg">
		<div style="text-align: center;">
			<img src="./assets/aggregator_architecture.svg" alt="Aggregator architecture" width="80%">
		</div>
	</a>
	<figcaption>Architecture of the aggregator based on the Swiss EPR Architecture</figcaption>
</figure>

## Next Steps

<div class="grid cards" markdown>

-   __Resources__

    ---

    Informations about transactions, workflows, documents, formats that are implemented by the aggregator

    [See Resources](./resources/index.md)

-   __References__

    ---

	Details about Endpoints, OIDs, authorship and timestamps

    [See References](./references/index.md)

-   __Useful Links__

    ---

    List of different resources useful for integrators

    [See Useful Links](./other/useful_links.md)

-   __Upcoming Changes__

    ---

    Informations about currently deployed versions, known issues, changelog and next release dates

    [See Upcoming Changes](./other/upcoming_changes.md)

</div>
