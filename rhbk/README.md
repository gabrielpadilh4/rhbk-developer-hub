# RHBK Operator - Basic Quickstart Deployment for OpenShift

This guide covers the deployment of Red Hat build of Keycloak Operator configured with a `rhdh` realm and `rhdh` client configured.

## Prerequisites
- An OpenShift cluster with the Keycloak Operator installed.
- `oc` CLI tool authenticated to your cluster.

## Deployment Steps

1. Provision the database
Deploy the PostgreSQL StatefulSet, Service, and the associated credentials secret.
   ```
   oc apply -f rhbk/postgresql.yaml -n rhbk
   ```
   **Note**: The current PostgreSQL configuration uses `emptyDir` for storage. This is suitable for development but will not persist data if the database pod is deleted.
   **Note**: Sometimes a Openshift cluster may not have the image `postgresql:15-el9` in the internal registry, check for a valid image with `oc get images -n openshift-image-registry | grep postgresql` and update the image tag under `rhbk/postgresql.yaml`.

2. Deploy Keycloak CR
Apply the Keycloak Custom Resource (CR). This triggers the Operator to configure the Keycloak instance using the PostgreSQL host postgres-db and the TLS secret created in the previous step.
   ```
   oc apply -f rhbk/keycloak.yaml -n rhbk
   ```

3. Expose Red Hat build of Keycloak instance
   ``` 
   oc create route edge rhbk-route --service keycloak-instance-service --insecure-policy Redirect --hostname=rhbk.$(oc get route console -n openshift-console -o jsonpath='{.spec.host}' | sed 's/console-openshift-console.//' && echo) -n rhbk
   ``` 

4. Access the Keycloak route with the username and password to login in to the RHBK console
   ```
   echo "Keycloak Console: https://$(oc get routes/rhbk-route -o jsonpath='{.spec.host}' -n rhbk && echo)"
   echo "Username: $(oc get secret keycloak-instance-initial-admin -o jsonpath='{.data.username}' -n rhbk | base64 --decode && echo)"
   echo "Password: $(oc get secret keycloak-instance-initial-admin -o jsonpath='{.data.password}' -n rhbk | base64 --decode && echo)"
   ```