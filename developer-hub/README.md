# RHDH Operator - Basic Quickstart Deployment for OpenShift integrated with RHBK

#### Prerequisites
- An OpenShift cluster with the Developer Hub Operator installed.
- `oc` CLI tool authenticated to your cluster.

#### Deployment Steps

1. Create the configmap that enableds the Red Hat build of Keycloak dynamic plugin
   ```
   oc create configmap rhbk-dynamic-plugin --from-file dynamic-plugins.yaml
   ```

2. Create the app config that contains the RHBK configuration
   ```
   oc create configmap rhbk-app-config --from-file app-config.yaml
   ```

3. Deploy Backstage CR
   ```
   oc apply -f backstage.yaml
   ```