## Notes:

**This guide is very preliminary and has had minimal testing. Use at your own risk.**

This guide is intended only for those who need to install UNMS on a system that runs Docker, but is NOT able to run the official installation scripts. Most users should follow instructions at [[ Installation-&-Update ]].

This installation guide references tools like `useradd`, `usermod`, `curl` and `envsubst`. You may need to substitute alternative tools available on your system.

All commands must be executed as `root`.

## Prerequisites:
- Docker Engine 1.10.0 or later
- Docker Compose 1.9.0 or later

## Installation:

1) Create or choose a user account for UNMS and add it to group `docker`
    ```
    UNMS_USER=unms

    useradd -m "${UNMS_USER}"
    usermod -aG docker "${UNMS_USER}"
    ```

0) Download and unpack the UNMS installation package to `/home/${UNMS_USER}/app`
    ```
    UNMS_VERSION=0.7.16

    mkdir /home/${UNMS_USER}/app
    cd /home/${UNMS_USER}/app
    curl -sS "https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/unms-${UNMS_VERSION}.tar.gz" | tar xz
    ```

0) Substitute all variables in `docker-compose.yml.template` to create `docker-compose.yml`
    ```
    export VERSION=${UNMS_VERSION}           # UNMS version to install
    export USER_ID=$(id -u ${UNMS_USER})     # UID of the UNMS user account
    export HTTP_PORT=80                      # HTTP port for UNMS
    export HTTPS_PORT=443                    # HTTPS port for UNMS
    export PUBLIC_HTTPS_PORT=443             # Port where UNMS is exposed to users
    export BEHIND_REVERSE_PROXY=false        # true if running behind a reverse proxy
    export SSL_CERT=                         # SSL cert filename (optional)
    export SSL_CERT_KEY=                     # SSL cert key filename (optional)
    export SSL_CERT_CA=                      # SSL cert CA filename (optional)
    export DOCKER_IMAGE=ubnt/unms
    export BRANCH=master
    export CONFIG_DIR=/home/${UNMS_USER}/app/conf
    export DATA_DIR=/home/${UNMS_USER}/data
    export PROD=true
    export DEMO=false
    export HOST_TAG=

    cd /home/${UNMS_USER}/app 
    envsubst < docker-compose.yml.template > docker-compose.yml
    ```

0) Create data volumes under `/home/${UNMS_USER}/data`
    ```
    DATA_DIR="/home/${UNMS_USER}/data"

    mkdir "${DATA_DIR}"
    mkdir "${DATA_DIR}/config-backups"
    mkdir "${DATA_DIR}/images"
    mkdir "${DATA_DIR}/firmwares"
    mkdir "${DATA_DIR}/firmwares/manual"
    mkdir "${DATA_DIR}/firmwares/ubnt"
    mkdir "${DATA_DIR}/import"
    mkdir "${DATA_DIR}/unms-backups"
    mkdir "${DATA_DIR}/update"
    mkdir "${DATA_DIR}/redis"
    mkdir "${DATA_DIR}/logs"
    mkdir "${DATA_DIR}/postgres"
    ````

0) Create or link a volume for SSL certificates
    
    To let UNMS register a certificate automatically:
    ```
    mkdir "${DATA_DIR}/cert"
    ```
    or, to supply UNMS with an existing certificate:
    ```
    ln -sT "/my-certificates" "${DATA_DIR}/cert"
    ```

0) Set file ownership and permissions
    ```
    chmod -R 700 "${DATA_DIR}"
    chown -R "${UNMS_USER}" "/home/${UNMS_USER}"
    ```

0) Run Docker Compose
    ```
    cd /home/${UNMS_USER}/app;
    /usr/local/bin/docker-compose up -d
    ```

0) Verify that UNMS is up and running
    ```
    docker ps
    ```
    Status of all UNMS containers should be `Up`. In case any of the containers are `Restarting` or not running, or the UNMS URL is not accessible, consult the log files located under `/home/${UNMS_USER}/data/logs`