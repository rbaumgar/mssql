
# rhel-mssql
Run Microsoft SQL Server on RHEL 7 within OpenShift.

Create an OpenShift image with Microsoft SQL Server.

    oc new-project mssql
    oc create -f https://raw.githubusercontent.com/rbaumgar/mssql/mssql.yaml

with the following command you will see the build

    oc get pod

The logile can be streamed with

    oc log -f rhel7-mssql-1-build

Check the created image

    oc get is rhel7-mssql


## Referenze
This is based on the Dockerfile from https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/RHEL
