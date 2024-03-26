# Firely Server running with Docker Compose

Firely's [FHIR Server](https://fire.ly/products/firely-server/), with configuration and volumes prepared for easier configuration.

Currently, this is using only FHIR v4. Looking forward to move to v5 :)

## Docker Compose

Uses 2 services:
- Firely's FHIR Server
- SQL Server for persistence of operational and administration data

### Map custom resources into Firely Server container

Firely Server in `docker-compose.yml` uses several volumes, mapped into `firely-configuration` directory. To run the service, this directory must include:

- `license` folder with a license `firelyserver-license.json` inside:
    - Free tier can be gotten from [Firely Server Trial](https://fire.ly/firely-server-trial/);
    - The license is ignored by Git, as it must not be shared by source control;

- `plugins` folder:
    - Any custom plugins (DLLs) must be put inside this folder, to be used by the Server;
    - Custom plugins' namespaces must also be referenced within `appsettings`, to be picked up by the Server;
    - Currently its content is ignored by Git, to avoid having built DLLs in source control (instead, these can be built locally, in a CI/CD process, etc.);

- `settings` folder with:
    - `Appsettings` and `logsettings` JSON files;
    - An example of an environment-specific `appsettings` file, with an example to exclude Importing operations (`appsettings.instance.exclude_import.json`)

- `vonk-import` folder to load additional conformance resources;

- `vonk-imported` folder to to save the import history of conformance resources:
    - Its content is ignored by Git, to allow each local environment to do the initial loading into their persistence infrastructure (in this case, SQL Server).
