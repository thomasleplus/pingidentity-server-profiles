# Purpose
This documents explains how server profiles work for PingIdentity docker images.
Included in this repo are several examples for getting started with some basic profiles.

## What's in it?
Server profiles aim at providing a common convention to adapt the common images into workable containers in all circumstances.

## Documentation
 * The complete collection of documentation for Ping Identity’s DevOps GitHub repositories is located on [Gitbook](https://pingidentity-devops.gitbook.io/devops/)
 * Please navigate to Ping Identity's DevOps [page](https://www.pingidentity.com/content/developer/en/devops.html) for additional resources

## Layout
### PingFederate
- instance
    Place any file that need to be present with the runtime of the product, abiding with the file layout of the product.
    - Some useful examples:
        - instance/server/default/data/drop-in-deployer/data.zip may be used to apply a configuration archive exported to a container.
        This may be really convenient for going from a PingFederate instance to deployed PingFederate containers
        - server/default/deploy/OAuthPlayground.war
        automatically deploy the OAuthPlayground web application
        - instance/server/default/conf/pingfederate.lic
        inject a PingFederate license into the running container
        - instance/server/default/conf/META-INF/hivemodule.xml
        apply a hivemodule configuration to the container

### PingAccess
- instance
    Place any file that need to be present with the runtime of the product, abiding with the file layout of the product.

### PingDirectory
- dsconfig
    Provide dsconfig configuration fragments.
When the container starts, all the fragments will be appended in the order in which they are numbered and applied in one large batch.
Files must be named with two digits a dash, a label and with the .dsconfig extension

- data
    Provide ldif data fragments.
When the container starts, all the provided files will be imported in the order in which they're named.
Files must be named with two digits a dash, the backend name (the default is userRoot), a label and with the .ldif extension for plain LDIF or .ldif.gz for a gzip compressed file
For example:
    - 10-userRoot-skeleton.ldif
    - 20-userRoot-users.ldif.gz
    - 30-userRoot-groups.ldif
- hooks
    In this directory you can put a set of custom scripts that will be called a key points during the container startup sequence.
    - On every container start event:
        - 00-immediate-startup.sh is called immediately upon container startup
    - Only the first time a container starts:
        - 10-before-copying-bits.sh is called 
        - 20-before-setup.sh is called immediately before the setup tool is executed
        - 30-before-configuration.sh is called immediately before the dsconfig tool is executed
        - 40-before-import.sh is called immediately before the import-ldif is executed
    - On every container start event:
        - 50-before-post-start.sh is called before the built-in postStart.sh script is executed in the background
        - 80-post-start.sh is called instead of the built-in postStart.sh script if it is provided

- instance
    Place any file that need to be present with the runtime of the product, abiding with the file layout of the product.
    You may place custom schema in instance/config/schema/
    You may provide your certificates in a JKS keystore with these two files:
        instance/config/keystore
        instance/config/keystore.pin
    You may provide your certificates in a PKCS#12 keystore with these two files:
        instance/config/keystore.p12
        instance/config/keystore.pin
    You may provide your trusted certificates in a JKS trust store by providing this file:
        instance/config/truststore 
    You may provide your trusted certificates in a JKS trust store by providing this file:
        instance/config/truststore.p12
    Optionally, if the trustore is pin-protected, provide the 
        instance/config/truststore.pin

### PingDataSync
- dsconfig
    Provide dsconfig configuration fragments.
When the container starts, all the fragments will be appended in the order in which they are numbered and applied in one large batch.
Files must be named with two digits a dash, a label and with the .dsconfig extension
        
- instance
    Place any file that need to be present with the runtime of the product, abiding with the file layout of the product.
    You may place custom schema in instance/config/schema/
    You may provide your certificates in a JKS keystore with these two files:
        instance/config/keystore
        instance/config/keystore.pin
    You may provide your certificates in a PKCS#12 keystore with these two files:
        instance/config/keystore.p12
        instance/config/keystore.pin
    You may provide your trusted certificates in a JKS trust store by providing this file:
        instance/config/truststore 
    You may provide your trusted certificates in a JKS trust store by providing this file:
        instance/config/truststore.p12
    Optionally, if the trustore is pin-protected, provide the 
        instance/config/truststore.pin

## Docker images

* A complete listing of Ping Identity's public images used in these examples are available at [Docker Hub](https://hub.docker.com/u/pingidentity/)

## Troubleshooting
This repoistory is in active development and has not been offically released.
If you experiece issues with this project, please feel free to log it by opening an issue.
