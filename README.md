# Red Hat Developer Hub (RHDH) Integration with Red Hat Single Sign-On (RH-SSO)

This repository provides a complete guide and the necessary manifests to install and integrate the Red Hat Developer Hub with Red Hat Single Sign-On on OpenShift. 

## 🏗️ Architecture Overview
The integration consists of two main parts:
1. **Red Hat Single Sign-On (RH-SSO):** Acts as the Identity Provider (IdP) for authentication and the source of truth for Users.
2. **Red Hat Developer Hub (RHDH):** The developer portal that uses Keycloak for OIDC authentication and synchronizes its software catalog with Keycloak's organizational data.

## 📁 Repository Structure
* **`rhsso/`**: Contains manifests for deploying Keycloak and the `rhdh` realm configuration.
* **`developer-hub/`**: Contains the RHDH Custom Resource, dynamic plugin configurations, and app-level settings.

## 🛠️ Prerequisites
* The `oc` CLI tool authenticated to your cluster.

## 🚀 Deployment Roadmap

To achieve a successful integration, follow the documentation in each subdirectory in this specific order:

### Phase 1: Install Red Hat Single Sign-On and Red Hat Developer Hub Operators
1. Create the necessary resources for the Red Hat Single Sign-On and Red Hat Developer Hub Operators
   ```
   oc apply -f operators.yaml
   ```
2. Approve the install plan for Red Hat Single Sign-On
   ```   
   oc get installplan -n rhsso
   oc patch installplan/<install-plan-name> -n rhsso --type merge --patch '{"spec":{"approved":true}}'
   ```
   **NOTE:** Sometimes take some few minutes to the install plan appear due the OLM activies like downloading images and more.
3. Approve the install plan for Red Hat Developer Hub
   ```   
   oc get installplan -n rhdh-operator
   oc patch installplan/<install-plan-name> -n rhdh-operator --type merge --patch '{"spec":{"approved":true}}'
   ```
   The result is the Red Hat Single Sign-On installed in the `rhsso` namespace and the Red Hat Developer Hub installed in the `rhdh-operator` namespace.
      ![Operator Installed](./operators.png)

### Phase 2: Identity Provider Setup
Navigate to the [RHSSO directory](./rhsso/README.md) to:
1. Create the Red Hat Single Sign-On instance

### Phase 3: Developer Hub Setup
Once Keycloak is running and configured, navigate to the [Developer Hub directory](./developer-hub/README.md) to:
1. Configure `app-config.yaml` with the OIDC endpoints and secrets generated in Phase 1.
2. Enable the Keycloak dynamic plugin via ConfigMap.
3. Deploy the RHDH instance via the Backstage Custom Resource.
4. Import the pre-configured `rhsso/rhdh-realm.yaml` that contains a `rhdh` realm and client.
