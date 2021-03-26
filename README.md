
This repository is a docker-compose setup for a quickstart with the [SecurityRAT](https://github.com/SecurityRAT/SecurityRAT) tool.

**Important: the setup is not suitable for a production environment!**

## How to run

1. Clone this project
1. [Optional] You can modify the database settings as described in the **Configuration** section below.
1. Run `docker-compose up --remove-orphans` from the project's root directory.
1. After all services have started, navigate to http://localhost:9002 in your browser.
1. Authenticate with one of the default users `admin/admin` or `user/user`
1. Update the database with the requirements set included as SQL dump ([OWASP ASVS 3.0.1 set](https://github.com/SecurityRAT/Security-Requirements/blob/master/owasp_asvs_3_0_1.sql)) in the _mariadb-service_ image. Do this by running this command in a new terminal:

    ```sh
    docker exec securityrat-mariadb sh -c './var/dumpRequirements.sh'
    ```

## Configuration

The docker-compose file already contains default configuration, hence the following settings are optional:

### Database settings

1. Change the default database settings using the following environment variables in the **docker-compose.yml**.

    ```yaml
    environment:
        MYSQL_DATABASE: # database name
        MYSQL\_ROOT\_PASSWORD: # the root password.
        MYSQL\_USER: # This user has full priviledge over the MYSQL\_DATABASE
        MYSQL\_PASSWORD: #Password of the MYSQL\_USER
    ```

1. Change the _securiryrat_ service configuration in **docker-compose.yml** to match the modifications from the previous step.

    ```yaml
    environment:
        - SPRING\_DATASOURCE\_URL=jdbc:mysql://mariadb-service:3306/${MYSQL\_DATABASE}?useUnicode=true&characterEncoding=utf8&useSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC&createDatabaseIfNotExist=true
        - SPRING\_DATASOURCE\_USERNAME=${MYSQL\_USER} 
        - SPRING\_DATASOURCE\_PASSWORD=${MYSQL\_PASSWORD}
    ```

## Cleaning up

After stopping the app, run `docker-compose down` from the project's root directory to remove the created containers.
