This is a minimal small openliberty and mssql demo using the application from this [repo](https://github.com/Azure-Samples/open-liberty-on-aro/blob/master/guides/howto-integrate-azure-managed-databases.md).

Rather than use the OpenLiberty operator, which is not currently available in 4.9, it uses a simple Deployment object delivered via kustomize.

To use this demo, run the following steps:

1. Deploy database
`$ oc apply -k database/base`

2. Wait for database pod to start, once available you will need to manually create the database. You can rsh into the pod and use sqlcmd but I prefeer to use port forwarding and use my local tools (mssql-cli on linux)

```
$ oc port-forward service/sqlserver 1433:1433

# In new terminal window
$ mssql-cli
user: sa
password: r3dh4t1!

> CREATE DATABASE cafedb
```

3. Deploy the application
`$ oc apply -k app/base`

Once the application is deployed an working you can start working through improvements:

*. Create liveness and readiness checks for the application and database
*. Explore using the openliberty operator once [available](https://github.com/OpenLiberty/open-liberty-operator/issues/251) in 4.9 or if using an earlier version of OpenShift have at it
*. Introduce OpenShift GitOps as a way to deploy and manage the application