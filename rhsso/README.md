# RHSSO Operator - Basic Quickstart Deployment for OpenShift

This guide covers the deployment of Red Hat Single Sign-On Operator configured with a `rhdh` realm and `rhdh` client configured.

## Prerequisites
- An OpenShift cluster with the Keycloak Operator installed.
- `oc` CLI tool authenticated to your cluster.

## Deployment Steps

1. Deploy Keycloak CR
Apply the Keycloak Custom Resource (CR). This triggers the Operator to configure the Keycloak instance using the PostgreSQL host postgres-db and the TLS secret created in the previous step.
   ```
   oc apply -f rhsso/keycloak.yaml -n rhsso
   ```

2. Access the Keycloak route with the username and password to login in to the rhsso console
   ```
   echo "Keycloak Console: https://$(oc get routes/keycloak -o jsonpath='{.spec.host}' -n rhsso && echo)"
   echo "Username: $(oc get secret credential-rhsso-instance -o jsonpath='{.data.ADMIN_USERNAME}' -n rhsso | base64 --decode && echo)"
   echo "Password: $(oc get secret credential-rhsso-instance -o jsonpath='{.data.ADMIN_PASSWORD}' -n rhsso | base64 --decode && echo)"
   ```