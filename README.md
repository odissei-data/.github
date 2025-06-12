# ODISSEI - Open Data Infrastructure for Social Science and Economic Innovations 

![ODISSEI banner](https://odissei-data.nl/wp-content/uploads/2024/05/banner-ODISSEI-1536x415.jpeg)


# ODISSEI Portal 

https://portal.odissei.nl/

## ODISSEI Portal User Guide

See https://doi.org/10.5281/zenodo.15236529.

## ODISSEI Portal Infrastructure Documentation

### 1. Application Stacks & Environments

List of repositories for each application stack.

| **Application Stack**   | **Repository URL**                                                | **Latest** | **Deployment Method**                               |
|-------------------------|-------------------------------------------------------------------|------------|-----------------------------------------------------|
| ODISSEI Dataverse stack | <https://github.com/odissei-data/odissei-devstack>                | master     | Deployed manually                                   |
| SKOSMOS stack           | <https://github.com/NatLibFi/Skosmos>                             | 2.18.1     | Setup by devstack repo                              |
| Workflow orchestrator   | <https://github.com/odissei-data/ingestion-workflow-orchestrator> | 3.1.2      | Automatic deployment on production via CI/CD        |
| Microservices           | https://github.com/odissei-data/dataverse-importer                | master     | Microservice used to import datasets into Dataverse |

#### ODISSEI Portal

The ODISSEI Portal runs on three environments: Production, Staging, and
Devstack.

To run the Portal, two dockerized application stacks, are required:

- Dataverse portal stack: uses a SOLR, Postgres, and Payara containers.

- Skosmos stack: uses a Skosmos, Fuseki, and Varnish container.

- Traefik container: used as reversed proxy. It uses Letsencrypt for
  automatic SSL certificate renewal. It also shows an overview of the
  Prefect server and labs-demo server that run the Workflow Orchestrator
  and microservices used by the Orchestrator respectively.


#### Workflow Orchestrator

The Workflow Orchestrator is used to automatically ingest metadata from
the different data providers**,** present in the ODISSEI Portal. The
ingestion-workflow-orchestrator application stack is behind the Workflow
Orchestrator, using Prefect and Postgres DB to coordinate the actions of
metadata ingestion and enrichment microservices for Orchestrator

### 2. Server Inventory

List of servers and their roles:

| **Server Name** | **Environment** | **Operating System** | **Location** | **Specifications**           |
|-----------------|-----------------|----------------------|--------------|------------------------------|
| odissei-02      | Production      | Ubuntu 22.04         | Azure        | 4 vCPUs, 16GB RAM, 500GB SSD |
| odissei-01      | Staging         | Ubuntu 22.04         | Azure        | 4 vCPUs, 16GB RAM, 250GB SSD |
| devstack        | Development     | Ubuntu 22.04         | SURF R-cloud | 8 vCPUs, 32GB RAM, 250GB SSD |
| prefect         | Production      | Ubuntu 24.04         | Azure        | 2 vCPUs, 8GB RAM, 124GB SSD  |
| labs-demo       | Production      | Ubuntu 22.04         | Azure        | 4 vCPUs, 16GB RAM, 30GB SSD  |

### 3. Applications and Services

#### Production Server: odissei-02

This server is used to host the production ODISSEI Portal
(<https://portal.odissei.nl>) and Skosmos vocabulary browser
(<https://skosmos.odissei.nl>). It is setup with a traefik reverse
proxy. This server runs in Microsoft Azure and can only be accessed by
DANS personnel.

| **Service**   | **Stack**               | **Version**             | **Deployment repository**                                                          | **Notes**                      |
|---------------|-------------------------|-------------------------|------------------------------------------------------------------------------------|--------------------------------|
| SOLR          | ODISSEI Dataverse stack | v9.x                    | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Used for indexing and search   |
| PostgreSQL    | ODISSEI Dataverse stack | v16                     | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Stores metadata and user data  |
| Payara        | ODISSEI Dataverse stack | gdcc/dataverse:alpha    | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Runs the Dataverse application |
| Skosmos       | Skosmos                 | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Vocabulary browser             |
| Fuseki        | Skosmos                 | stain/jena-fuseki:5.1.0 | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | RDF triple store               |
| Varnish Cache | Skosmos                 | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Caching layer                  |

#### Staging Server: odissei-01

This server is used to host the staging ODISSEI Portal
(<https://portal.staging.odissei.nl>) and Skosmos vocabulary browser
(<https://skosmos.staging.odissei.nl>). This server runs in Microsoft
Azure and can only be accessed by DANS personnel.

| **Service**   | **Stack**               | **Version**             | **Deployment repository**                                                          | **Notes**                      |
|---------------|-------------------------|-------------------------|------------------------------------------------------------------------------------|--------------------------------|
| SOLR          | ODISSEI Dataverse stack | 9.x                     | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Used for indexing and search   |
| PostgreSQL    | ODISSEI Dataverse stack | 16                      | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Stores metadata and user data  |
| Payara        | ODISSEI Dataverse stack | gdcc/dataverse:alpha    | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Runs the Dataverse application |
| Skosmos       | Skosmos                 | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Vocabulary browser             |
| Fuseki        | Skosmos                 | stain/jena-fuseki:5.1.0 | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | RDF triple store               |
| Varnish Cache | Skosmos                 | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Caching layer                  |

#### Development Server: devstack

This server is used by ODISSEI developers for testing and
experimentation. It host the staging ODISSEI Portal
([https://portal.devstack.odissei.nl](https://portal.sattodissei.nl/))
and Skosmos vocabulary browser
([https://skosmos.devstack.odissei.nl](https://skosmos.devstack.odissei.nl/)).
To access this server, a user needs to be added to the
<span class="mark">ODISSEI collaboration</span> on SURF Research Cloud
as a developer. They can then add their public ssh key to their profile
and will gain automatic access to the server using ssh with their
username.

| **Service**   | **Stack**               | **Version**             | **Deployment repository**                                                          | **Notes**                                               |
|---------------|-------------------------|-------------------------|------------------------------------------------------------------------------------|---------------------------------------------------------|
| SOLR          | ODISSEI Dataverse stack | 9.x                     | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Used for indexing and search                            |
| PostgreSQL    | ODISSEI Dataverse stack | 16                      | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Stores metadata and user data                           |
| Payara        | ODISSEI Dataverse stack | gdcc/dataverse:alpha    | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Runs the Dataverse application                          |
| Skosmos       | sd-skosmos              | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Vocabulary browser                                      |
| Fuseki        | sd-skosmos              | stain/jena-fuseki:5.1.0 | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | RDF triple store                                        |
| Varnish Cache | sd-skosmos              | Latest                  | [odissei-dataverse-stack](https://github.com/odissei-data/odissei-dataverse-stack) | Caching layer                                           |
| Prefect       | workflow-orchestrator   | 2.1.0                   |                                                                                    | Tool to ingest metadata into Dataverse using workflows. |

#### Prefect Server: prefect

This server hosts the Prefect workflow orchestrator. The orchestrator is
used to automatically ingest dataset metadata. For each data provider
there is an associated Prefect workflow, dedicated to ingesting their
datasets' metadata onto the ODISSEI Portal (Dataverse instance). Common
metadata tasks - harvesting, mapping, and enrichment - have been
encapsulated in microservices that are run separately from the
orchestrator. The orchestrator schedules its workflows to run daily to
keep the metadata in the portal up to date. This server runs in
Microsoft Azure and can only be accessed by DANS personnel.

The metadata in the production ODISSEI Portal is kept up-to-date by the
workflow orchestrator running on
[https://prefect.dansdemo.nl](https://prefect.dansdemo.nl/). The
metadata in the staging and devstack ODISSEI Portal is only updated
manually, which can be done via the orchestrator on
[https://prefect.dansdemo.nl](https://prefect.dansdemo.nl/) or a local
orchestrator setup.

| **Service** | **Stack**             | **Version** | **Deployment repository** | **Notes**                                               |
|-------------|-----------------------|-------------|---------------------------|---------------------------------------------------------|
| Prefect     | workflow-orchestrator | 3.1.2       |                           | Tool to ingest metadata into Dataverse using workflows. |

#### Microservices Server: labs-demo

This server hosts the metadata microservices. These microservices are
all containerized and deployed via a CI/CD setup, which tests the code,
creates and pushes a new docker image when a GitHub tag is created. This
server runs in Microsoft Azure and can only be accessed by DANS
personnel.

| **Service**              | **Version** | **API documentation**                                               | **Git Repo**                                                                                           |
|--------------------------|-------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Metadata Fetcher         |             | [Metadata Fetcher](https://dataverse-fetcher.labs.dansdemo.nl/docs) | [dataverse-metadata-fetcher](https://github.com/odissei-data/dataverse-metadata-fetcher)               |
| Dataverse Mapper         |             | [Dataverse Mapper](https://dataverse-mapper.labs.dansdemo.nl/docs)  | [dataverse-mapper](https://github.com/odissei-data/dataverse-mapper)                                   |
| Dans Transformer Service |             | [Transformer Service](https://transformer.labs.dans.knaw.nl/docs)   | [dans-transformer-service](https://github.com/ekoi/dans-transformer-service)                           |
| Metadata Refiner         |             | [Metadata Refiner](https://metadata-enhancer.labs.dansdemo.nl/docs) | [metadata-refiner](https://github.com/odissei-data/metadata-refiner)                                   |
| Metadata Enhancer        |             | [Metadata Enhancer](https://metadata-refiner.labs.dansdemo.nl/docs) | [metadata-enhancer](https://github.com/odissei-data/metadata-enhancer)                                 |
| Email Sanitizer          |             | [Email Sanitizer](https://emailsanitizer.labs.dansdemo.nl/docs)     | [email-sanitize-microservice](https://github.com/thomasve-DANS/email-sanitize-microservice)            |
| Version Tracker          |             | [Version Tracker](https://version-tracker.labs.dansdemo.nl/docs)    | [version-tracker](https://github.com/odissei-data/version-tracker)                                     |
| DOI Minter               |             | [DOI Minter](https://dataciteminter.labs.dansdemo.nl/docs)          | [submitmd2dc-service](https://github.com/ekoi/submitmd2dc-service/tree/using-dans-transformer-service) |
| Semantic Enrichment      |             |                                                                     | [semantic-enrichment](https://github.com/Dans-labs/semantic-enrichment)                                |
| OAI-PMH Harvester        |             | [ODISSEI Harvester](https://oai-harvester.labs.dansdemo.nl/docs)    | [odissei-harvester](https://github.com/odissei-data/odissei-harvester)                                 |
| OAI Enrichment Service   |             | [OAI Enricher Service](https://oai-service.labs.dansdemo.nl/docs)   | [oai-enricher-service](https://github.com/ekoi/oai-enricher-service)                                   |

### 4. Networking & Security

- **Networking:**

  - Traefik container with automatic SSL certificate renewal using
    > letsencrypt.

### 5. Monitoring & Logging

- **Monitoring:** ODISSEI production portal website is monitored in
  zabbix.

- **Logging:** No centralized logging. \`docker logs {container name}\`

### 6. Backup

- **Database Backups:** Cronjob that makes backups every day. Kept on
  server itself.
