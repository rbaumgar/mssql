
# rhel-mssql
Run Microsoft SQL Server on RHEL 7 within OpenShift.

## Create Image
Create an OpenShift image with Microsoft SQL Server.

    oc new-project mssql
    oc create -f https://raw.githubusercontent.com/rbaumgar/mssql/mssql.yaml

with the following command you will see the build

    oc get pods

The logile can be streamed with

    oc log -f rhel7-mssql-1-build

Check the created image

    oc get is rhel7-mssql

## Deploy Image
Create a new deployment with two parameters. 
ACCEPT_EULA is mandatory!
SA_PASSWORD is the database password.

    oc new-app --name=mssql --image-stream=rhel7-mssql -e ACCEPT_EULA=Y -e SA_PASSWORD=Welcome1
    
with the folloging command you will see the new created pod mssql-1-xxxxx

    oc get pods
    
check the startup log with

    oc logs mssql-1-xxxxx
 
 with following command you can forward the mssql server port to the local host
 
    oc port-forward mssql-1-63k20 1433:1433
    
I tested with SQLectron, https://sqlectron.github.io/
e.g. connect to localhost port 1433 with user sa and password(SA_PASSWORD)
select @@version
go

## Open Tasks
### Terminal acces does not work
sqlcmd from the Openshift Terminal does not work.
    
    sh-4.2$ cd /opt/mssql-tools/bin/
    sh-4.2$ ./sqlcmd -s localhost -U sa -P Welcome1                                                                                                                                                         
    Sqlcmd: Error: Microsoft ODBC Driver 13 for SQL Server : Driver's SQLAllocHandle on SQL_HANDLE_HENV failed.      
    
### persistant volume   
Mount a persistant volume at /var/opt/mssql

## Reference
This is based on the Dockerfile from https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/RHEL
