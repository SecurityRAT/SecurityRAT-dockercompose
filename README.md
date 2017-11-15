This is a docker-compose repository for a quick&easy start if you want to play around with the SecurityRAT tool (otherwise available at https://github.com/SecurityRAT/SecurityRAT).

**Important: the setup is not suitable for a production environment!**

## Configuration
Configuration wise, nothing must be done since the application can be as well started with the default settings. The means that the following configurations are optional:

### Database settings
1. First change the appropriate value in the mysql-service:
    ```yaml
    environment: 
        MYSQL_DATABASE: # database name
        MYSQL\_ROOT\_PASSWORD: # the root password.
        MYSQL\_USER: # This user has full priviledge over the MYSQL\_DATABASE
        MYSQL\_PASSWORD: #Password of the MYSQL\_USER
    ```
1. Change the values in the _securiryrat_ service to match does made above
    ```yaml
    environment:
        - SPRING\_DATASOURCE\_URL=jdbc:mysql://mysql-service:3306/${MYSQL\_DATABASE}?useUnicode=true&characterEncoding=utf8&useSSL=false
        - SPRING\_DATASOURCE\_USERNAME=${MYSQL\_USER} 
        - SPRING\_DATASOURCE\_PASSWORD=${MYSQL\_PASSWORD}
    ```
## How to run
1. Clone this project
1. Before running a new version of SecurityRAT, make sure to remove the old latest docker image. You can achieve this by running `docker rmi securityrat/securityrat:latest`.
1. Open a terminal and run `docker-compose build` from the project's root directory. This will build the latest mysql image used by the securityrat service.
1. Run `docker-compose up --remove-orphans` from the project's root directory.
1. After all services have started, navigate to https://localhost:9002 in your browser
1. Authenticate with one of the default users `admin/admin` or `user/user`
1. _Optional:_ In order to play around with already existing data. We provide a list of [requirements](https://github.com/SecurityRAT/Security-Requirements/blob/master/requirements.sql) as SQL dump. This dump has already being copied to the mysql image build in step 2. In other to execute this, open another terminal and run `docker exec securityrat-mysql sh -c './var/dumpRequirements.sh'`

## Cleaning up
After stopping the app, run the following command from the project's root directory:

    docker-compose down

