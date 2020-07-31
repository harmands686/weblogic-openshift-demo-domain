
===============================
This sample project contains the scripts that are required to build the weblogic domain image from base weblogic binaries.
Dockerfile in this project extends the Oracle WebLogic Server image by creating a sample WLS 12.2.1.3 domain and cluster. Utility scripts are copied into the image, enabling users to plug Node Manager automatically into the Administration Server running on another container.

## How to Build
**NOTE:** The image is based on a WebLogic Server image in the docker-images project: `oracle/weblogic:12.2.1.3-developer`. Build that image to your local repository before building this sample.
This sample deploys a simple, one-page web application contained in a ZIP archive. This archive needs to be built (one time only) before building the Docker image.

    $ ./build-archive.sh

The sample requires the Admin Host, Admin Port and Admin Name. It also requires the Managed Server port and the domain debug port. The ports will be EXPOSED through Docker. The other arguments are persisted in the image to be used when running a container. If an attribute is not provided as a `--build-arg` on the `build` command, the following defaults are set.

```
CUSTOM_ADMIN_NAME = admin-server
 The value is persisted to the image as ADMIN_NAME

CUSTOM_ADMIN_HOST = wlsadmin
 The value is persisted to the image as ADMIN_HOST

CUSTOM_ADMIN_PORT = 7001
 The value is persisted to the image as ADMIN_PORT

CUSTOM_MANAGED_SERVER_PORT = 8001
 The value is persisted to the image as MANAGED_SERVER_PORT

CUSTOM_DEBUG_PORT = 8453
 The value is persisted to the image as DEBUG_PORT

CUSTOM_DOMAIN_NAME = base_domain
 The value is persisted to the image as DOMAIN_NAME
```

To build this sample keeping the defaults, run:

    $ docker build \
          --build-arg WDT_MODEL=simple-topology.yaml \
          --build-arg WDT_ARCHIVE=archive.zip \
          --build-arg WDT_VARIABLE=properties/docker-build/domain.properties \
          --force-rm=true \
          -t demo-domain:1.0 .
This will use the model, variable, and archive files in the sample directory and create a weblogic domain image.
